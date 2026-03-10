---
title: "R/fr"
url: "https://docs.alliancecan.ca/wiki/R/fr"
category: "General"
last_modified: "2025-12-16T18:27:01Z"
page_id: 2634
display_title: "R"
language: "fr"
---

R est un outil de calcul statistique et de graphiques. Il s'agit d'un langage de programmation additionné d'un environnement graphique, d'un débogueur, de l'accès à certaines fonctions de système et de la possibilité d'exécuter des scripts.

Même si R n'a pas été développé pour le calcul de haute performance, sa popularité au sein de plusieurs disciplines scientifiques incluant le génie, les mathématiques, la statistique et la bio-informatique, en fait un outil essentiel sur les supercalculateurs dédiés à la recherche universitaire. Certaines fonctionnalités étant écrites en C, compilées et parallélisées par fils d'exécution, permettent d'atteindre des performances raisonnables sur un seul nœud de calcul. Grâce à la nature modulaire de R, les utilisateurs peuvent personnaliser leur configuration en installant des paquets dans leur répertoire personnel à partir du Comprehensive R Archive Network (CRAN).

Vous trouverez peut-être des informations utiles dans le billet de blogue de l'utilisatrice Julie Fortin intitulé How to run your R script with Compute Canada.

== Interpréteur ==
Chargez d'abord un module R. Comme plusieurs versions sont disponibles, consultez la liste en lançant la commande

Pour charger un module R particulier, utilisez une variante de la commande

Pour plus d'information, consultez Utiliser des modules.

Vous pouvez maintenant démarrer l'interpréteur et entrer le code R dans cet environnement.

Pour exécuter des scripts R de manière non interactive, utilisez la commande Rscript suivie du fichier contenant les commandes R :

Cette commande passera automatiquement les options appropriées pour un traitement en lot, soit --slave et --no-restore à l'interpréteur R. Ces options empêcheront la création de fichiers d'espace de travail inutiles avec --no-save lors d'un traitement en lot.

Les calculs d'une durée de plus de deux ou trois minutes ne devraient pas être exécutés par un nœud de calcul, mais être soumis à l'ordonnanceur.

Voici un exemple de script simple :

Pour plus d'information, consultez Exécuter des tâches.

== Installation des paquets R ==

=== install.packages() ===

Pour installer des paquets du CRAN, vous pouvez utiliser install.packages dans une session R interactive sur un nœud de connexion. Puisque les nœuds de calcul de la plupart de nos grappes n'ont pas accès à l'internet, il n'est pas possible d'installer les paquets R dans une tâche en lots ou dans une tâche interactive. Parce que plusieurs paquets R sont développés avec la famille de compilateurs GNU, nous vous recommandons de charger un module gcc avant de les installer et de toujours utilisez la même version du gcc.

==== Installation pour une version particulière de R ====
Par exemple, pour installer le paquet sp qui offre des classes et des méthodes pour les données spatiales, utilisez cette commande sur un nœud de connexion.

Si l'argument repos n'est pas spécifié, on vous demandera de sélectionner un miroir pour le téléchargement. Idéalement, ce miroir sera géographiquement proche de la grappe que vous utilisez.

Avant l'installation, certains paquets requièrent la définition de la variable d'environnement TMPDIR.

==== Installation pour une ou plusieurs versions de R ====
Indiquez le répertoire local, selon le module de R qui est chargé.
~/.local/R/$EBVERSIONR/
}}
Installez le paquet.
"https://cloud.r-project.org/")'}}

Dans le script de soumission, vous devez ensuite charger le module R que vous voulez et configurer le répertoire local pour la bibliothèque avec  export R_LIBS=~/.local/R/$EBVERSIONR/.

=== Dépendances ===
Certains paquets utilisent des bibliothèques qui sont déjà installées sur nos grappes. Si la bibliothèque se trouve dans la liste des logiciels disponibles, chargez le module approprié avant d'installer le paquet.

Par exemple, le paquet rgdal utilise la bibliothèque gdal. En lançant la commande module spider gdal/2.2.1 nous voyons que les modules nixpkgs et gcc sont requis. Pour savoir comment charger ce module, entrez la commande module spider gdal/3.9.1.

Si l'installation d'un paquet échoue, portez attention au message d'erreur qui pourrait indiquer d'autres modules qui seraient requis. Pour plus d'information sur les commandes de module, consultez Utiliser des modules.

===Téléchargement de paquets===
Si vous cherchez à installer un paquet que vous avez téléchargé, c'est-à-dire que vous n'avez pas utilisé install.packages(), vous pouvez l'installer comme suit. Par exemple, avec le paquet archive_package.tgz, vous exécuteriez la commande suivante dans l'interpréteur (shell)

==Appels système==

La commande R system() permet d'exécuter des commandes dans l'environnement actif, à l'intérieur de R; ceci risque de causer des problèmes sur nos grappes parce que R donne une valeur incorrecte à la variable d'environnement LD_LIBRARY_PATH. Utilisez plutôt la syntaxe system("LD_LIBRARY_PATH=$RSNT_LD_LIBRARY_PATH ") dans vos appels système.

== Arguments passés à un script R ==
Il peut parfois être utile de passer des paramètres en argument à un script R pour éviter d'avoir à modifier le script pour plusieurs tâches semblables ou de devoir gérer plusieurs copies d'un même script. Ceci peut servir pour spécifier des paramètres numériques ou le nom des fichiers en entrée ou en sortie. Par exemple, au lieu d'employer une syntaxe comme

et de changer le code à chaque fois qu'un paramètre est modifié, les paramètres peuvent être passés au script au début avec

et par la suite

Dans le prochain exemple, il doit y avoir précisément deux arguments. Le premier devrait être une chaîne de caractères représentant le nom de la variable et le deuxième devrait numéro de la variable.

Ce script peut être utilisé comme suit

==Parallélisation==

Si les processeurs de nos grappes sont on ne peut plus ordinaires, ce qui rend ces supercalculateurs intéressants, c'est qu'ils offrent des milliers de CPU sur un réseau très performant. Pour profiter de cet avantage, vous devez utiliser la programmation parallèle. Cependant, avant d'allouer beaucoup de temps et d'effort à paralléliser votre code R, assurez-vous que votre implémentation séquentielle est aussi efficiente que possible. Comme dans tout langage interprété, d'importants goulots d'étranglement (bottlenecks) sont causés par les boucles et particulièrement les boucles imbriquées, ce qui a un impact sur la performance. Lorsque possible, essayez d'utiliser les fonctions vectorielles et les autres éléments plus fonctionnels comme la famille des fonctions apply et la fonction ifelse. Vous obtiendrez souvent un gain de performance en éliminant une boucle plutôt que de paralléliser son exécution avec plusieurs cœurs CPU.

La page CRAN Task View on High-Performance and Parallel Computing with R
mentionne un grand nombre de paquets pouvant être utilisés avec R pour la programmation parallèle.
Vous trouverez une excellente vue d'ensemble et des conseils dans le contenu du
colloque de Compute Ontario du 11 octobre 2023 intitulé High-Performance Computing in R
(diapositives).

Vous trouverez d'autres renseignements et exemples dans les sous-sections ci-dessous.

Terminologie : Dans notre documentation, les termes nœud et hôte sont quelquefois employés pour désigner un ordinateur distinct; un regroupement de nœuds ou d'hôtes constitue une grappe.
nœud désigne souvent un processus de travail (worker process); un regroupement de ces processus constitue une grappe. Prenons comme exemple la citation suivante : « Following snow, a pool of worker processes listening via sockets for commands from the master is called a 'cluster' of nodes. »https://stat.ethz.ch/R-manual/R-devel/library/parallel/doc/parallel.pdf.

=== doParallel et foreach ===
====Utilisation====
Foreach peut être vu comme une interface unifiée pour tous les systèmes dorsaux (backends) comme doMC, doMPI, doParallel, doRedis, etc. et fonctionne sur toutes les plateformes pourvu que le système dorsal soit fonctionnel. doParallel agit comme interface entre foreach et le paquet parallèle et peut être chargé seul. Certains problèmes de performance connus surviennent avec foreach lors de l'exécution d'un très grand nombre de très petites tâches. Notez que l'exemple simple qui suit n'utilise pas l'appel foreach() de façon optimale.

Enregistrez le système dorsal en lui indiquant le nombre de cœurs disponibles. Si le système dorsal n'est pas enregistré, foreach assume que le nombre de cœurs est 1 et exécute les itérations de façon séquentielle.

La méthode générale pour utiliser foreach est :
# chargez foreach et le paquet dorsal;
# enregistrez le paquet dorsal;
# appelez foreach() en le laissant sur la même ligne que l'opérateur %do% (série) ou %dopar%.

====Exécution====

1. Placez le code R dans un fichier script, ici le fichier test_foreach.R.

Copiez ce qui suit dans le script job_foreach.sh.

3. Soumettez la tâche.

Pour plus d'information sur comment soumettre des tâches, consultez Exécuter des tâches.

=== doParallel et makeCluster ===
====Utilisation====
Il faut enregistrer le système dorsal (backend) en lui donnant le nom des nœuds, multiplié par le nombre voulu de processus. Par exemple, nous créerions une grappe composée des hôtes node1 node1 node2 node2. Le type de grappe PSOCK exécute des commandes par des connexions SSH vers les nœuds.

====Exécution====
1. Placer le code R dans un fichier script, ici test_makecluster.R.

2. Copiez les lignes suivantes dans un script pour soumettre la tâche, ici job_makecluster.sh.
 cut -f 1 -d '.'))
R -f test_makecluster.R
}}

Dans cet exemple, l'ordonnanceur pourrait placer les quatre processus sur un seul nœud.
Ceci peut convenir, mais si vous voulez prouver que la même tâche peut être traitée si les processus
sont placés sur des nœuds différents, ajoutez la ligne #SBATCH --ntasks-per-node=2.

3. Soumettez la tâche avec

Pour plus d'information sur comment soumettre une tâche, voyez Exécuter des tâches.

=== Rmpi ===

====Installation====
La procédure suivante installe Rmpi, une interface (wrapper) pour les routines MPI qui permet d'exécuter R en parallèle.

1. Voyez les modules R disponibles avec la commande

module spider r

2.  Sélectionnez la version de R et chargez le module OpenMPI approprié.

module load gcc/12.3
module load openmpi/4.1.5
module load r/4.5.0

3. Téléchargez la dernière version de Rmpi  en remplaçant le numéro de la version selon le cas.

wget https://cran.r-project.org/src/contrib/Rmpi_0.7-3.3.tar.gz

4. Indiquez le répertoire dans lequel vous voulez copier les fichiers; vous devez avoir une permission d'écriture pour ce répertoire. Le nom du répertoire peut être modifié.

mkdir -p ~/local/R_libs/
export R_LIBS=~/local/R_libs/

5. Lancez la commande d'installation.

R CMD INSTALL --configure-args="--with-Rmpi-include=$EBROOTOPENMPI/include   --with-Rmpi-libpath=$EBROOTOPENMPI/lib --with-Rmpi-type='OPENMPI' " Rmpi_0.7-3.3.tar.gz

Portez attention au message d'erreur qui s'affiche quand l'installation d'un paquet échoue; il pourrait indiquer d'autres modules qui seraient nécessaires.

====Exécution====

1. Placez le code R dans un fichier script, ici le fichier test.R.

2. Copiez ce qui suit dans le script job.sh.

3. Soumettez la tâche.

sbatch job.sh

Pour plus d'information sur comment soumettre des tâches, consultez Exécuter des tâches.