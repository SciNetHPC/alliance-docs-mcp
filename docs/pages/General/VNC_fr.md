---
title: "VNC/fr"
url: "https://docs.alliancecan.ca/wiki/VNC/fr"
category: "General"
last_modified: "2026-01-15T08:24:02Z"
page_id: 8527
display_title: "VNC"
language: "fr"
---

Pour certains paquets complexes (par exemple MATLAB), il est parfois utile de démarrer à distance l'interface utilisateur graphique. Pour ce faire, on utilise généralement SSH avec redirection X11, mais la performance est souvent trop lente, tout comme avec  MobaXTerm ou Putty. Une meilleure option est d'utiliser VNC pour se connecter à un ordinateur à distance.

= Configuration =

Pour vous connecter au serveur VNC, vous devez d'abord installer un client VNC sur votre ordinateur. Nous recommandons le paquet TigerVNC disponible pour Windows, macOS et pour la plupart des distributions Linux. Nous abordons ici le téléchargement, l'installation et la configuration de TigerVNC de manière sécuritaire pour chaque système d'exploitation.

== Windows ==

Téléchargez et exécutez la plus récente version stable de l'installateur vncviewer64-x.y.z.exe à partir de la page officielle, par exemple vncviewer64-1.15.0.exe (depuis avril 2025). Vérifiez que vous avez bien téléchargé VNC Viewer et non le serveur. Pour créer des tunnels sécuritaires entre votre ordinateur et vncserver comme décrit ci-dessous, vous devez ouvrir une fenêtre de terminal et exécuter la commande SSH, ce qui peut être fait avec le standard PowerShell de Windows 10, à partir de la mise à jour 1809.

== macOS ==

Installez le plus récent paquet DMG stable à partir de la page officielle de téléchargement et cliquez sur le bouton vert Download Latest Version pour TigerVNC-1.15.0.dmg (depuis avril 2025). Quand le fichier DMG est téléchargé, faites un double-clic dessus pour l'ouvrir.  Une fenêtre contextuelle est affichée avec l'icône de TigerVNC Viewer ainsi que les fichiers LICENSE.TXT et README.rst. Glissez l'icône  tigervnc dans le dossier Applications et/ou dans le Dock. Pour supprimer la fenêtre contextuelle, vous devez démonter le fichier  DMG. Pour ce faire, ouvrez une fenêtre de New Finder; assurez-vous que View->ShowSidebar est sélectionné. Dans le menu de gauche, cliquez sur la petite flèche vers le haut près de TigerVNC-1.15.0; et fermez la fenêtre de Finder. Avec macOS Monterey 12.2, si TigerVNC plante, utilisez plutôt la dernière version.

== Linux ==

Installez TigerVNC Viewer pour votre version de Linux :

Version                	Commande
Debian, Ubuntu         	sudo apt-get install tigervnc-viewer
Fedora, CentOS, or RHEL	sudo yum install tigervnc
Gentoo                 	emerge -av net-misc/tigervnc

Lancez TigerVNC par le menu des applications ou en entrant vncviewer sur la ligne de commande de votre ordinateur.

= Connexion =

Vous avez maintenant besoin d'un serveur VNC auquel vous connecter. Il peut s'agir d'un vncserver temporaire que vous avez lancé sur un nœud de connexion ou un nœud de calcul, comme expliqué dans les sections ci-dessous.

== Nœuds de connexion ==

Avec votre ordinateur, vous pouvez utiliser des applications peu exigeantes (qui ne requièrent pas de GPU) sur un bureau VNC distant, sous condition de certaines limites de mémoire et de temps CPU. Pour ce faire, connectez-vous d'abord à un nœud de connexion. Par exemple, sur Nibi,

 [laptop:~] ssh nibi.alliancecan.ca

Lancez ensuite vncserver -list pour savoir si d'autres vncservers dont vous n'avez plus besoin sont toujours actifs sur le nœud Nibi auquel vous êtes connecté; si c'est le cas, tuez-les avec la commande pkill.

 [l4(login node):~] pkill Xvnc -u $USER

Étape 1 : Vous pouvez maintenant lancer vncserver sur le nœud de connexion, comme ci-dessous.

 [l4(login node):~] vncserver -idletimeout 86400
 Desktop 'TurboVNC: l4.nibi.sharcnet:1 (yourusername)' started on display l4.nibi.sharcnet:1
 Starting applications specified in /cvmfs/soft.computecanada.ca/gentoo/2023/x86-64-v3/usr/bin/xstartup.turbovnc
 Log file is /home/yourusername/.vnc/l4.nibi.sharcnet:1.log

Notez que la commande vncserver fournie par StdEnv/2023 est basée sur turbovnc. Lors du démarrage d'un nouveau vncserver sur un nœud de connexion, ajoutez -idletimeout seconds comme indiqué ci-dessus. Cela causera la fermeture définitive de votre vncserver (après S secondes sans connexion à VNC Viewer) si vous oubliez de fermer votre session vncviewer en cliquant sur System -> Log out sur le bureau VNC. Au premier démarrage de vncserver, vous devrez définir un mot de passe qui  pourra être modifié ultérieurement. Ce mot de passe sera requis pour vous connecter à  votre bureau avec un vncclient (tel que vncviewer). Le même mot de passe sera requis lors de l'établissement de  connexions multiples en supposant que vous ayez démarré vncserver en ajoutant l'option -alwaysshared.

Étape 2 : Déterminez maintenant quel est le port qui écoute le nouveau vncserver (dans cet exemple, il s'agit de 5901) avec la commande grep sur le fichier de journalisation.

 [l4(login node):~] grep -iE "\sport|kill" /home/yourusername/.vnc/l4.nibi.sharcnet:1.log
 25/08/2025 15:16:20 Listening for VNC connections on TCP port 5901

Vous pouvez maintenant quitter le nœud de connexion. Le vcnserver que vous avez lancé restera actif jusqu'à ce que la limite de temps définie avec l'option -idletimeout soit atteinte.

 [l4(login node):~]  exit
 [laptop:~]

Étape 3 : Sur votre bureau, démarrez un tunnel SSH. Ainsi, un port arbitraire (5905 dans cet exemple) sera transféré au port que votre serveur VNC écoute (5901 selon ce qui précède).

 [laptop:~] ssh nibi.computecanada.ca -L 5905:l4:5901

4) Sur la ligne de commande dans votre ordinateur, entrez ce qui suit pour vous connecter à vncviewer via le port qui vous a été donné.

 [laptop:~] vncviewer localhost:5905

Sous macOS ou Windows, vous pouvez aussi cliquer sur l'icône de l'application TigerVNC Viewer qui se trouve sur votre ordinateur et entrer localhost:5905 dans Connection Details. Tenez compte des limites imposées sur les nœuds de connexion en termes d'utilisation de la mémoire et du temps de CPU. Sur Graham, ces limites sont de 8Go de mémoire et de 1 heure d'utilisation de CPU par processus, selon ulimit -t -v; ceci peut varier en fonction de la grappe. Si vous avez besoin de plus de ressources, lancez votre serveur VNC sur un nœud de calcul plutôt que d'utiliser la méthode décrite plus loin.

== Nœuds de calcul ==

Si aucun nœud VDI n'est disponible, vous pouvez démarrer un serveur VNC dans un nœud de calcul puis vous y connecter à partir de votre ordinateur par redirection de port. Vous obtenez ainsi un accès dédié au serveur sans toutefois bénéficier de toutes les fonctionnalités graphiques ou de OpenGL accéléré.

1) Démarrez un serveur VNC

Avant de démarrer votre serveur VNC, connectez-vous à une grappe (par exemple Nibi) et créez une allocation sur un nœud de connexion avec la commande salloc (limite de 24 heures). Par exemple, pour demander une tâche interactive qui utilise 4 CPU et 16Go de mémoire, vous pourriez utiliser

 [l4(login node):~] salloc --time=1:00:00 --cpus-per-task=4 --mem=16000 --account=def-piusername
 salloc: Pending job allocation 1149016
 salloc: job 1149016 queued and waiting for resources
 salloc: job 1149016 has been allocated resources
 salloc: Granted job allocation 1149016
 salloc: Waiting for resource configuration
 salloc: Nodes c48 are ready for job
 [c48(compute node):~]

Une fois la tâche interactive démarrée, définissez une variable d'environnement pour éviter les erreurs répétitives.

 [c48(compute node):~] export XDG_RUNTIME_DIR=${SLURM_TMPDIR}

Ensuite, démarrez un serveur VNC avec vncserver et portez attention au nœud de calcul dans lequel la tâche est exécutée (c48 dans notre exemple). En cas de doute, utilisez la commande hostname pour vérifier de quel nœud il s'agit. Si vous faites ceci pour la première fois, une invite demandera un mot de passe pour votre serveur VNC. VOUS DEVEZ ENTRER UN MOT DE PASSE, autrement n'importe qui pourra se connecter et obtenir l'accès aux fichiers dans votre compte. Vous pourrez changer le mot de passe par la suite avec la commande vncpasswd. Revenons à l'exemple :

 [c48(compute node):~] vncserver
 Desktop 'TurboVNC: c48.nibi.sharcnet:1 (yourusername)' started on display c48.nibi.sharcnet:1
 Starting applications specified in /cvmfs/soft.computecanada.ca/gentoo/2023/x86-64-v3/usr/bin/xstartup.turbovnc
 Log file is /home/yourusername/.vnc/c48.nibi.sharcnet:1.log

Lancez la commande grep dans le fichier de journalisation pour identifier le port sur lequel votre serveur VCN écoute.

 [c48(compute node):~] grep -iE "\sport|kill" /home/yourusername/.vnc/c48.nibi.sharcnet:1.log
 26/08/2025 10:43:36 Listening for VNC connections on TCP port 5901

2) Établissez un tunnel SSH vers le serveur VNC

Une fois le serveur VNC démarré, il faut créer un tunnel sécurisé entre votre ordinateur et le nœud de calcul sur lequel vncserver est exécuté (voir l'étape précédente). Deux types de commandes peuvent être utilisées selon la grappe utilisée.

Pour toutes les grappes (sauf Nibi), vous pouvez utiliser la forme ssh username@clustername -L localforwardedport:computenode:remotelisteningport recommandée précédemment. Par exemple, si un vncserver est démarré sur le nœud de calcul  b>rc12509 de rorqual et que le port local de votre ordinateur portable à transférer est 5905, la commande appropriée est :

 [laptop:~] ssh username@rorqual.alliancecan.ca -L 5905:rc12509:5901
 Duo two-factor login for username
 Enter a passcode or select one of the following options:
 [rc12509(compute node):~]

Pour Nibi, une nouvelle forme de la commande doit être utilisée, soit ssh -J username@clustername -L localforwardedport:localhost:remotelisteningport computenode. De plus, une paire de clés SSH doit être créée sur votre ordinateur avec le contenu de la clé publique saisie dans votre fichier ~/.ssh/authorized_keys sur nibi. Cette approche fonctionne également sur toute autre grappe et pourrait donc être privilégiée. En reprenant l'exemple précédent, où c48 est le nœud de calcul sur lequel vous avez démarré vncserver et 5905 est le port local de votre ordinateur transféré, la commande pour Nibi est :

 [laptop:~] ssh -J username@nibi.alliancecan.ca -L 5905:localhost:5901 c48
 Duo two-factor login for username
 Enter a passcode or select one of the following options:
 [c48(compute node):~]

Si vous quittez le nœud auquel votre tunnel est connecté, vous ne pourrez plus vous connecter au serveur VNC avec vncviewer. Cependant, comme vncserver continuera de fonctionner, vous pourrez encore y accéder avec un nouveau tunnel. Pour plus d'information, voir Tunnels SSH.

3) Connectez-vous au serveur VNC

Sous Linux, ouvrez une nouvelle fenêtre de terminal et connectez votre client VNC à localhost:port. Dans le prochain exemple, la commande vncviewer de TigerVNC établit la connexion au serveur VNC actif sur cdr768. Vous devrez entrer le mot de passe que vous avez défini précédemment.

 [laptop:~]$ vncviewer localhost:5905
 TigerVNC viewer v1.15.0
 Built on: 2025-02-16 03:59
 Copyright (C) 1999-2025 TigerVNC team and many others (see README.rst)
 See https://www.tigervnc.org for information on TigerVNC.
 Tue Aug 26 10:59:59 2025
 DecodeManager: Detected 12 CPU core(s)
 DecodeManager: Creating 4 decoder thread(s)
 CConn:       Connected to host localhost port 5905
 CConnection: Server supports RFB protocol version 3.8
 CConnection: Using RFB protocol version 3.8
 CConnection: Choosing security type VeNCrypt(19)
 CVeNCrypt:   Choosing security type TLSVnc (258)
 Tue Aug 26 11:00:03 2025
 CConn:       Using pixel format depth 24 (32bpp) little-endian rgb888
 CConnection: Enabling continuous updates

Sous macOS ou Windows (mais pas de distribution Linux), plutôt que de lancer vncviewer par la ligne de commande, vous pouvez cliquer sur l'icône de l'application TigerVNC Viewer et entrez l'information sur le localhost:port.
Notez aussi que le port VNC par défaut utilisé par TigerVNC Viewer est 5900; par conséquent, si vous avez spécifié 5900 comme port local à transférer au démarrage de votre tunnel SSH, vous pourriez simplement spécifier localhost. Cependant, sous Windows, vous pourriez ne pas pouvoir configurer un tunnel SSH sur le port local 5900.

Une fois vncviewer connecté, un bureau Linux MATE s'affichera. Pour lancer un terminal, cliquez sur Applications -> System Tools -> MATE Terminal dans le menu du haut. Vous pouvez également ajouter un raccourci au menu supérieur en faisant un clic droit sur MATE Terminal et en cliquant sur Add this launcher to panel. Enfin, pour lancer un programme, utilisez la commande comme vous le feriez normalement dans une session bash, par exemple xclock. Pour démarrer un programme plus complexe comme MATLAB, chargez le module, puis exécutez la commande matlab.

= Autres points importants =

== Mot de passe ==

Pour réinitialiser votre mot de passe, utilisez la commande

[gra-login1:~] vncpasswd
Password:
Verify:
Would you like to enter a view-only password (y/n)? n

Vous pouvez choisir de supprimer définitivement votre configuration VNC incluant votre mot de passe en supprimant votre répertoire ~/.vnc. La prochaine fois que vous lancerez vncserver, une invite vous demandera de définir un nouveau mot de passe.

== Tuer le vncserver ==

Si un vncserver actif n'est plus nécessaire, tuez-le avec vncserver -kill :DISPLAY# comme démontré ci-dessous.

 [gra-login1:~] vncserver -list | grep -v ^$
 TurboVNC sessions:
 X DISPLAY #	PROCESS ID	NOVNC PROCESS ID
 :44	        27644
 [gra-login1:~] vncserver -kill :44
 Killing Xvnc process ID 27644

Si vous avez plusieurs vncservers actifs sur un nœud, vous pouvez les tuer TOUS du même coup avec
 [gra-login1:~] pkill Xvnc -u $USER

== Connexions multiples ==

Tous les vncservers exécutés sous votre nom d'utilisateur (sur un nœud de connexion ou de calcul) peuvent être listés avec vncserver -list. Si un vncserver a été démarré avec l'option supplémentaire -AlwaysShared, plusieurs connexions peuvent être établies en créant un nouveau tunnel et un nouveau vncviewer à partir de n'importe quel emplacement distant, par exemple

 [l4(login node):~] vncserver -idletimeout 86400 -alwaysshared | grep -v ^$
 Desktop 'TurboVNC: l4.nibi.sharcnet:1 (yourusername)' started on display l4.nibi.sharcnet:1
 Starting applications specified in /cvmfs/soft.computecanada.ca/gentoo/2023/x86-64-v3/usr/bin/xstartup.turbovnc
 Log file is /home/yourusername/.vnc/l4.nibi.sharcnet:1.log

Ainsi, on peut laisser vncviewer actif sur un ordinateur au bureau, puis se reconnecter à nouveau depuis chez soi pour accéder au même ordinateur et ensuite, par exemple, continuer à travailler de manière transparente avec les mêmes applications sans avoir à les fermer. Si toutefois un vncserver n'a pas été démarré avec vncserver -AlwaysShared, une seule connexion vncviewer sera possible et avant de quitter pour la maison, vous devrez fermer toutes les applications sur le bureau et fermer vncserver. Une fois à la maison, il faudra créer un nouveau bureau et ouvrir les applications pour poursuivre votre travail.

== Échecs de connexion ==

Des échecs répétés à établir une nouvelle connexion vncserver/vncviewer pourraient être causés par un tunnel SSH existant sur votre ordinateur qui bloquerait les ports. Pour déterminer la présence de tels tunnels et les tuer, ouvrez une fenêtre de terminal sur votre ordinateur et lancez la commande ps ux | grep ssh suivie de kill PID.

== Déverrouiller l'écran de veille ==

Si votre écran de veille VNC s'éteint en raison du délai d'affichage et qu'un mot de passe vous est demandé, entrez le mot de passe de votre compte sur la grappe et non le mot de passe vncserver.
Si le bureau MATE est en cours d'exécution et que l'écran de veille ne se déverrouille pas, essayez avec la commande killall -9 .mate-screensaver. Ceci ne devrait plus se produire car l'écran de veille VNC a été désactivé sur nos grappes.

== Problème de connexion ==

La connexion à gra-vdi.alliancecan.ca se fait en deux étapes :

1)
 username
 Entrez votre  mot de passe pour CCDB.
2)
 username
 Entrez votre code de passe pour l'authentification multifacteur.

Si vous entrez un nom d'utilisateur/mot de passe erroné pour l'étape 1, vous aurez quand même à passer l'étape 2. Si vous saisissez ensuite votre nom d'utilisateur/mot de passe, vous recevrez un message indiquant que la connexion est établie et vous reviendrez vers l'écran de connexion de l'étape 1. La solution est de réessayer en vous assurant d'avoir saisi la bonne combinaison nom d'utilisateur/mot de passe. Si vous ne vous souvenez plus de votre mot de passe CCDB, allez sur here pour le réinitialiser, à condition que votre compte ne soit pas en attente de renouvellement par la chercheuse principale ou le chercheur principal.

== OpenGL ==

== NIX ==

First, the salloc command must be modified to request a GPU node.  If this is not done, the program will fall back to using software-based rendering on CPUs, which is relatively much slower.  To request the first GPU node that brcomes available (and in turn minimize your queue wait time if the cluster has multiple GPU node types) simply specify:

 [l4(login node):~] salloc --time=1:00:00 --cpus-per-task=4 --gpus-per-node=1 --mem=16000 --account=def-piname

Les commandes nix et nix-env seront alors dans votre chemin pour gérer les paquets logiciels via nix dans votre environnement nix personnel.

 [l4(login node):~] salloc --time=1:00:00 --cpus-per-task=4 --gpus-per-node=t4:1 --mem=16000 --account=def-piname

Pour installer un paquet nix dans votre environnement, cliquez sur l'icône du terminal dans la barre de menu supérieure ou sélectionnez Applications -> System Tools -> Terminal. Vous pouvez maintenant chercher des programmes avec la commande nix search  et les installer avec  nix-env --install --attr . Par exemple, pour installer QGIS, utilisez

  [c48(compute node):~] vglrun -d egl PROGRAM

Then vglrun sets some extra environment variables to ensure your program will use correct virtualgl libraries.  If however your PROGRAM has already been patched to use the current cvmfs standard environment doing so will not be required.

Le moyen le plus simple est avec l'utilitaire patchelf de nix pour ajuster le binaire final; installez l'utilitaire avec nix-env --install --attr nixpkgs.patchelf. Par exemple, si vous construisez une application OpenGL pour utiliser les bibliothèques de système et que vous l'installez comme étant ~/.local/bin/myglapp>/code>, vous devez ajouter la bibliothèque VirtualGL du système comme première bibliothèque requise.

Si vous rencontrez des problèmes graphiques à l'utilisation de VNC comme décrit ci-dessus, essayez d'utiliser OpenOnDemand sur nibi ou JupyterHub sur rorqual. Les deux grappes offrent une interface web VDI automatisée avec un bureau moderne qui est conçue pour une utilisation simplifiée, ainsi que des performances matérielles améliorées et une meilleure prise en charge logicielle.