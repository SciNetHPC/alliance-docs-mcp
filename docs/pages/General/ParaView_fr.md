---
title: "ParaView/fr"
url: "https://docs.alliancecan.ca/wiki/ParaView/fr"
category: "General"
last_modified: "2025-10-21T18:40:54Z"
page_id: 10602
display_title: "ParaView"
language: "fr"
---

__FORCETOC__

= Visualisation à distance sur nos grappes =

== Introduction ==

Nous décrivons ici comment visualiser à distance votre ensemble de données situé sur une de nos grappes. Votre flux de travail serait semblable à un des scénarios suivants :

# Si votre ensemble de données n'est que de quelques Go (soit l'ensemble de données complet sans compopsante de temps, soit une seule étape d'une simulation qui dépend du temps), vous pouvez le visualiser de manière interactive en utilisant un petit nombre de CPU. Dans le flux de travail, vous démarrez une session de bureau à distance via JupyterHub ou Open OnDemand, selon la grappe, et vous exécutez ParaView de manière interactive. Pour les détails, voir sous l'onglet Courtes tâches interactives.
# Si vous souhaitez visualiser de manière interactive un ensemble de données plus grand, nous vous recommandons d'utiliser une configuration client-serveur où le client ParaView se trouve sur votre ordinateur et le serveur est en parallèle dans une tâche soumise à une grappe par Slurm. La taille de l'ensemble de données dépend de la grappe : sur Trillium, seules les tâches sur nœuds entiers par multiples de 192 cœurs sont autorisées. Votre ensemble de données doit donc être entre 50Go et 100Go pour exploiter efficacement les 192 cœurs. Sur Fir, Narval, Nibi et Rorqual, vous pouvez visualiser des ensembles de données beaucoup plus petits avec un seul cœur, même si l'utilisation de plusieurs cœurs en parallèle accélère le rendu. Cette configuration étant plus complexe, JupyterHub ou Open OnDemand est généralement recommandé pour les petits ensembles de données avant de tenter une configuration client-serveur. Pour les détails, voir sous l'onglet Longues tâches interactives.
# Idéalement, toutes les visualisations produites, comme la génération de 1 000 images pour une vidéo, devraient être scriptées et exécutées en lots, hors écran, sur les grappes,  en effectuant le rendu directement dans des fichiers sans ouvrir de fenêtres interactives. Les deux premiers onglets doivent être vus comme des étapes interactives pour configurer votre visualisation et l'enregistrer sous forme de script Python ParaView, qui peut ensuite être exécuté comme une tâche en lots sur la grappe, soit séquentiellement, soit plus souvent en parallèle. Pour les détails, voir sous l'onglet Production en lots.

== Utilisation de GPU ==

Dans tous les cas, n'utilisez pas les GPU H100, car ils ne sont pas optimisés pour le rendu graphique. Bien que les cartes H100 puissent exécuter des applications OpenGL et Vulkan, elles n'utilisent que 2 des 66 contrôleurs de fils d'exécution (ce nombre peut varier), ce qui entraîne une utilisation du GPU à environ 3%. Cela est non seulement une utilisation inacceptable de la grappe, mais aussi produit des rendus à des vitesses comparables à celles d'un GPU d'ordinateur portable milieu de gamme. Notez que les instances MIG (partitions statiques de GPU) ne peuvent pas exécuter d'API graphiques telles qu'OpenGL ou Vulkan.

Si un rendu GPU est absolument nécessaire (bien que cela ne soit envisageable que dans des cas très spécifiques), utilisez les nœuds AMD MI300A de Nibi ou des GPU NVIDIA plus anciens (par exemple T4) lorsqu'ils sont disponibles. Nous documenterons sur cette page toutes les options de rendu autres que sur H100.

== Flux de travail ==

Ouvrez l'onglet qui décrit le type de votre flux de travail.

Nous décrivons ici la visualisation interactive avec le bureau à distance via JupyterHub et Open OnDemand. Si vous utilisez Fir, Rorqual ou Narval, veuillez consulter l'une des sections JupyterLab ci-dessous. Si vous utilisez Nibi ou Trillium, veuillez faire défiler la page jusqu'à l'une des sections Open OnDemand ci-dessous.

== Avec un seul cœur via JupyterLab ==

Sur Fir, Rorqual ou Narval, vous pouvez lancer une instance JupyterLab via un portail.

# Connectez-vous à  JupyterHub sur une des grappes en utilisant votre compte avec l'Alliance.
# Dans le formulaire Server Options  :
## sous Account sélectionnez un des comptes CPU (n'utilisez pas de GPU);
## sous GPU configuration sélectionnez None;
## sous Number of Cores sélectionnez  1;
## sous Time entrez la durée de votre session JupyterLab;
## sous Memory entrez une valeur en vous basant surla quantité maximale de données à traiter en une seule fois;
## sous User interface sélectionnez  JupyterLab;
## appuyez sur Start. En arrière-plan, l'ordonnanceur Slurm soumet la tâche à la grappe.
# Après environ une minute, la tâche sera lancée et le tableau de bord de JupyterLab sera affiché dans votre navigateur.

Vous avez maintenant deux options, dont

  Sur le côté gauche, sous l'onglet Software Modules, chargez le module paraview/6.0.0.
  Un bouton ParaView (VNC)  devrait s'afficher, Cliquez sur ce bouton pour démarrer ParaView dans un bureau virtuel.

      Si ParaView ne démarre pas automatiquement, cliquez sur le raccourci qui se trouve sur le bureau.

Autrement, dans le tableau de bord de JupyterLab

  Cliquez sur le bouton de votre bureau préféré pour ouvrir une session dans un bureau virtuel.
  Dans ce  bureau virtuel, lancez un terminal (habituellement via Applications > System ...) et entrez

Une fenêtre ParaView devrait s'afficher et vous pouvez commencer.

== Avec plusieurs cœurs via JupyterLab ==

Puisque ParaView n'est pas multifil, plusieurs cœurs ne peuvent pas être utilisés directement. Certains filtres, tels que l'affichage d'isolignes (contouring), l'écrêtage (clipping) ou le rééchantillonnage, prennent en charge le multifil via certaines fonctions de VTK utilisant TBB ou OpenMP en arrière-plan. Cependant, pour un rendu véritablement parallèle, vous devez connecter le client ParaView avec un seul cœur à un serveur ParaView parallèle. Les deux peuvent être lancés dans JupyterLab, comme indiqué ci-dessous.

En comparaison de la procédure ci-dessus pour la visualisation  avec un seul cœur via JupyterLab, les principales différences sont :

* Dans Server Options-->Number of Cores , sélectionnez le nombre de cœurs souhaité, par exemple 4.
* Sous Memory, adaptez votre requête en conséquence, par exemple pour 4 cœurs, sélectionnez 14 400Mo de mémoire (donc 3 600 Mo par cœur).
* Au démarrage de votre session JupyterLab, vous aurez accès à une tâche MPI avec 4 CPU.
* Ouvrez votre bureau virtuel préféré, puis un terminal à l'intérieur de celui-ci et entrez

:et ensuite

* Ensuite, dans le bureau virtuel, ouvrez un autre terminal et entrez

* Dans l'interface de ParaView, cliquez sur le bouton Connect, ensuite
*# cliquez sur Add Server;
*# sélectionnez Server Type = Client/Server;
*# définissez Host = localhost (au lieu du nom du nœud de calcul);
*# définissez Port = 11111 (comme dans le Connection URL de l'exemple ci-dessus);
*# sélectionnez Startup Type = Manual.
* cliquez encore sur Connect pour connecter le client ParaView distant au serveur parallèle distant  (exécutés tous deux dans la session JupyterLab).
* Vous pouvez maintenant charger un ensemble de données pour un rendu en parallèle sur 4 cœurs.

Pour vérifier que vous effectuez un rendu parallèle, vous pouvez colorer votre ensemble de données avec la variable Process Id (cette variable n'est pas disponible en mode séquentiel).

== Avec un seul cœur via Open OnDemand ==

Sur Nibi et Trillium, vous pouvez lancer une instance Open OnDemand à partir d'un portail en utilisant votre compte avec l'Alliance. Connectez-vous à https://ondemand.sharcnet.ca (pour Nibi) ou à https://ondemand.scinet.utoronto.ca (pour Trillium).

Une fois la connexion établie, allez à Desktop dans le menu. Sur Nibi il se trouve sous Compute Nodes | Nibi Desktop. Spécifiez un compte Slurm pour CPU seulement ainsi que d'autres ressources (1 CPU), puis cliquez sur Launch. Attendez que la tâche démarre (Starting devrait changer à Running), puis cliquez sur Launch Nibi Desktop. Sur le bureau, ouvrez un terminal et entrez

Chargez votre ensemble de données et vous pouvez maintenant travailler sur votre visualisation.

== Visualisation avec plusieurs cœurs via Open OnDemand ==

Puisque ParaView n'est pas multifil, plusieurs cœurs ne peuvent pas être utilisés directement. Certains filtres, tels que le contouring, le clipping ou le rééchantillonnage, prennent en charge le multifil via des arrière-plans VTK comme TBB ou OpenMP. Cependant, pour un rendu véritablement parallèle, vous devez connecter le client ParaView avec un seul cœur à un serveur ParaView parallèle. Les deux peuvent être lancés dans JupyterLab, comme indiqué ci-dessous.

Suivez les mêmes étapes que pour Open OnDemand en séquentiel ci-dessus. Lorsque vous spécifiez des ressources, Open OnDemand de Nibi vous permet de demander jusqu'à 128Go de mémoire et jusqu'à 8 cœurs.

Supposons que vous ayez spécifié 4 cœurs. Dans votre session Open OnDemand, vous aurez accès à une tâche MPI avec 4 CPU. Ouvrez un terminal sur votre bureau virtuel et entrez

:et ensuite

Toujours dans le bureau virtuel, lancez un autre terminal et entrez

Dans l'interface de ParaView,
* cliquez sur le bouton Connect, ensuite
*# cliquez sur Add Server;
*# sélectionnez Server Type = Client/Server;
*# définissez Host = localhost (au lieu du nom du nœud de calcul);
*# définissez Port = 11111 (comme dans le Connection URL de l'exemple ci-dessus);
*# sélectionnez Startup Type = Manual.
* cliquez encore sur Connect pour connecter le client ParaView distant au serveur parallèle distant (exécutés tous deux dans la session Compute Desktop).
* Vous pouvez maintenant charger un ensemble de données pour un rendu en parallèle sur 4 cœurs.

Pour vérifier que vous effectuez un rendu parallèle, vous pouvez colorer votre jeu de données avec la variable Process Id (cette variable n'est pas disponible en mode séquentiel).

Nous décrivons ici la configuration client-serveur interactive sur tous nos clusters HPC (Rorqual, Nibi, Fir, Trillium et Narval), où un client s'exécute sur votre ordinateur et le serveur à distance sur la grappe.

Remarque 1 : La même version majeure doit être installée sur le client local et sur l'ordinateur hôte à distance; dans le cas contraire, certaines incompatibilités peuvent empêcher la connexion client-serveur. Par exemple, pour utiliser la version 6.0.0 du serveur ParaView sur nos grappes, vous avez besoin de la version client 6.0.0 sur votre ordinateur.

Remarque 2 : Un paramètre important dans les préférences de ParaView est Render View -> Remote/Parallel Rendering Options -> Remote Render Threshold. Si vous utilisez la valeur par défaut (20Mo) ou une valeur similaire, un petit rendu sera effectué sur le GPU de votre ordinateur, la rotation avec la souris sera rapide, mais tout rendu moindrement intensif (près de 20Mo) sera envoyé sur votre ordinateur et, selon votre connexion, la visualisation pourrait être lente. Si vous utilisez plutôt 0Mo, tout le rendu se fera à distance, rotation comprise. Vous utiliserez donc les ressources de la grappe pour tout, ce qui est avantageux pour le traitement de données volumineuses, mais moins pour l'interactivité. Testez différentes valeurs pour trouver la mieux adaptée.

Vous pouvez effectuer à la fois le tramage (rasterization) et le lancer de rayons (ray tracing) sur les CPU de la grappe en allouant autant de cœurs que nécessaire à votre rendu. Les bibliothèques modernes pour CPU telles qu'OSPRay et OpenSWR offrent des performances similaires à celles des rendus sur GPU. De plus, comme le serveur ParaView utilise MPI pour le traitement en mémoire distribuée pour les jeux de données très volumineux, il est possible d'effectuer un rendu parallèle sur un grand nombre de cœurs CPU, que ce soit sur un seul nœud ou plusieurs.

Le moyen le plus simple d'estimer le nombre de cœurs nécessaires est de calculer la quantité de mémoire nécessaire à votre rendu et de la diviser par environ 3,5Go/cœur. Par exemple, un ensemble de données de 40Go (chargé au complet en mémoire, par exemple dans un même pas de temps) nécessiterait au moins 12 cœurs pour traiter efficacement les données. Le rendu logiciel demandant beaucoup de puissance CPU, nous déconseillons d'allouer plus de 4Go/cœur. De plus, il est important de prévoir de la mémoire pour les filtres et pour le  traitement des données (par exemple, la conversion d'un ensemble de données structuré en un ensemble de données non structuré multipliera votre empreinte mémoire par trois environ). Selon votre flux de travail, vous pouvez démarrer ce rendu avec 32 ou 64 cœurs. Si votre serveur ParaView est subitement interrompu lors du traitement de ces données, vous devrez augmenter le nombre de cœurs.

Remarque 3 : Sur Trillium, vous devez demander des nœuds entiers, c'est-à-dire par multiples de 192 cœurs. Par conséquent, l'exemple minimal sur Trillium nécessitera 192 cœurs.

1. Sur votre poste de travail, installez la même version de ParaView que celle que vous utiliserez sur la grappe; connectez-vous ensuite à la grappe et lancez une tâche interactive parallèle avec plusieurs cœurs CPU.

1:00:0 --ntasks... --mem-per-cpu3600 --accountdef-someprof}}

:Sur Trillium, supposant que vous utilisez un seul nœud pour la visualisation, la commande est

1:00:0 --ntasks192 --accountdef-someprof}}

:La tâche interactive devrait démarrer automatiquement sur un des nœuds CPU.

2. À l'invite de commande qui s'exécute dans votre tâche, chargez le module ParaView et démarrez le serveur. Notez que sur Trillium, vous devez charger StdEnv/2023 avant de tenter de charger paraview/6.0.0.

:et ensuite

:Attendez que le serveur soit prêt à accepter la connexion client.

3. Prenez note du nœud (ici fc3066) et du port (habituellement 11111); dans un autre terminal sur votre ordinateur Mac/Linux (sous Windows, utilisez un émulateur de terminal), liez le port 11111 à votre ordinateur et le même port au nœud de calcul (assurez-vous d'utiliser le bon nœud de calcul). Notez que fir doit être remplacé par le nom de la grappe Rorqual, Fir, Trillium ou Narval. Pour Nibi, voir la remarque ci-dessous.

Remarque 4 : Nibi restreint la communication entre les nœuds par l'utilisation de SSH  (autre port que 11111), en plus de bloquer certains échanges lors de la connexion client-serveur initiale. Pour Nibi, utilisez plutôt la commande

pour rediriger au port SSH en deux étapes, via le nœud de connexion. L'indicateur T désactive l'allocation du pseudo-terminal et joue un rôle important dans la connexion initiale. Par contre, il désactive aussi les invites interactives dans l'interpréteur et il est normal que vous ne voyez rien en sortie après cette commande.

4. Sur votre ordinateur, démarrez ParaView; allez à File -> Connect (ou cliquez sur le bouton vert Connect dans la barre d'outils); cliquez sur Add Server. Configurez la connexion vers votre port local 11111 en ayant des paramètres semblables à name = fir, server type = Client/Server, host = localhost, port = 11111; cliquez sur Configure; cliquez sur Manual puis sur Save.
:Une fois que la connexion est ajoutée à la configuration, sélectionnez le serveur dans la liste affichée et cliquez sur Connect. Dans la première fenêtre de terminal, le message  Accepting connection ... est suivi de Client connected.

5. Ouvrez un fichier dans ParaView (il vous dirigera vers le système de fichiers distant) et visualisez-le comme d'habitude.

Pour vérifier que vous effectuez un rendu parallèle, vous pouvez colorer votre jeu de données avec la variable Process Id (cette variable n'est pas disponible en mode séquentiel).

Pour des tâches de visualisation longues, intensives et automatisées, nous vous recommandons fortement de passer à une visualisation par lots hors écran. ParaView prend en charge les scripts Python en entrée, ce qui vous permet de programmer votre flux de travail de visualisation et de le lui soumettre via une tâche de calcul standard, possiblement parallèle, sur une grappe. Si vous avez besoin d'assistance, contactez le soutien technique.

Pour un rendu séquentiel, la procédure devrait ressembler à

: où le script de tâche serial.sh ressemblerait à

Pour un rendu parallèle, la procédure devrait ressembler à

: où le script distributed.sh serait semblable à

= Visualisation client-serveur sur une machine virtuelle =

Nous décrivons ici la configuration et le flux de travail pour exécuter un serveur ParaView sur une machine virtuelle dans un  nuage. Cette approche est moins courante et ne doit être utilisée que si vous avez besoin d'une configuration personnalisée non prise en charge par ParaView installé sur la grappe.

== Prérequis ==

La page Cloud : Guide de démarrage décrit la création d'une machine virtuelle. Une fois connecté à la machine virtuelle, vous devrez installer certains paquets pour pouvoir compiler ParaView et VisIt; par exemple, sur une instance CentOS, entrez

Si vous avez votre propre paire de clés SSH (et non la clé générée pour le nuage), vous pouvez copier votre clé publique dans la machine virtuelle pour simplifier la connexion; pour ce faire, lancez la commande suivante sur votre ordinateur :

 ssh -i ~/.ssh/cloudwestkey.pem centos@vm.ip.address 'cat >>.ssh/authorized_keys'}}

== Compiler avec OSMesa ==

Comme les machines virtuelles n'ont pas accès à un GPU, et c'est le cas pour la plupart dans Arbutus, il faut compiler ParaView avec OSMesa pour obtenir un rendu hors écran (offscreen rendering). La configuration par défaut de OSMesa active OpenSWR, la librairie logicielle de tramage (rasterization) d'Intel qui permet d'opérer OpenGL. Le résultat sera un serveur ParaView qui utilise OSMesa pour construire un rendu sans X hors écran avec un CPU, mais avec les pilotes llvmpipe et SWR plus récents et plus rapides. Nous recommandons SWR.

Retournez sur la machine virtuelle et compilez cmake::

Ensuite, compilez llvm.

cd
 wget https://github.com/llvm/llvm-project/releases/download/llvmorg-21.1.0/LLVM-21.1.0-Linux-X64.tar.xz
 # unpack and cd there
 mkdir -p build && cd build
 cmake \
  -DCMAKE_BUILD_TYPE=Release \
  -DLLVM_BUILD_LLVM_DYLIB=ON \
  -DLLVM_ENABLE_RTTI=ON \
  -DLLVM_INSTALL_UTILS=ON \
  -DLLVM_TARGETS_TO_BUILD:STRING=X86 \
  ..
 make
 sudo make install

Ensuite, compilez Mesa avec OSMesa.

cd
 wget https://archive.mesa3d.org/mesa-25.2.3.tar.xz
 # unpack and cd there
 ./configure \
  --enable-opengl --disable-gles1 --disable-gles2 \
  --disable-va --disable-xvmc --disable-vdpau \
  --enable-shared-glapi \
  --disable-texture-float \
  --enable-gallium-llvm --enable-llvm-shared-libs \
  --with-gallium-drivers=swrast,swr \
  --disable-dri \
  --disable-egl --disable-gbm \
  --disable-glx \
  --disable-osmesa --enable-gallium-osmesa
 make
 sudo make install

Enfin, compilez le serveur ParaView.

cd
 wget https://www.paraview.org/files/v6.0/ParaView-v6.0.0.tar.gz
 # unpack and cd there
 mkdir -p build && cd build
 cmake \
      -DCMAKE_BUILD_TYPE=Release \
      -DCMAKE_INSTALL_PREFIX=/home/centos/paraview \
      -DPARAVIEW_USE_MPI=OFF \
      -DPARAVIEW_ENABLE_PYTHON=ON \
      -DPARAVIEW_BUILD_QT_GUI=OFF \
      -DVTK_OPENGL_HAS_OSMESA=ON \
      -DVTK_USE_OFFSCREEN=ON \
      -DVTK_USE_X=OFF \
      ..
 make
 make install

== Mode client-serveur ==

Vous pouvez maintenant lancer le serveur ParaView sur la machine virtuelle avec SWR.

./paraview/bin/pvserver --force-offscreen-rendering --opengl-window-backend OSMesa

Retournez sur votre ordinateur et créez un tunnel SSH partant du port local 11111 vers le port 11111 de la machine virtuelle.

ssh centos@vm.ip.address -L 11111:localhost:11111

Enfin, lancez le client ParaView sur votre ordinateur et connectez-vous à localhost:11111. Si tout fonctionne bien, vous pourrez ouvrir les fichiers de la machine virtuelle. Pendant que l'opération de rendu s'effectue, le message SWR detected AVX2 devrait s'afficher sur la console.