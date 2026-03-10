---
title: "GLOST/fr"
url: "https://docs.alliancecan.ca/wiki/GLOST/fr"
category: "General"
last_modified: "2025-03-05T22:22:57Z"
page_id: 7862
display_title: "GLOST"
language: "fr"
---

= Introduction =

GLOST (pour Greedy Launcher Of Small Tasks) est un outil pour exécuter un grand nombre de tâches séquentielles de courte durée ou de durée variable, ou avec des jeux de paramètres (parameter sweep). Son fonctionnement est semblable à celui de GNU parallel ou d’un vecteur de tâches, mais avec une syntaxe simplifiée.

GLOST utilise l’enveloppe (wrapper) glost_launch  et les commandes MPI  srun, mpiexec et mpirun. Un fichier texte nommé list_glost_tasks.txt regroupe les tâches et est employé comme argument pour l’enveloppe glost_launch.

GLOST est particulièrement utile dans les cas suivants :

* plusieurs tâches séquentielles de durée comparable,
* plusieurs tâches séquentielles de courte durée,
* tâches séquentielles avec paramètres variables (parameter sweep).

Le principe est de grouper plusieurs tâches séquentielles et de les faire exécuter dans une tâche MPI pouvant utiliser plusieurs cœurs (un ou plusieurs nœuds). Avec moins de tâches dans la queue,  l’ordonnanceur sera moins sollicité.

Vous pourriez considérer d'utiliser plutôt le paquet logiciel  META développé par une de nos équipes et qui comporte d'importants avantages par rapport à GLOST. Avec META, le temps d'attente total peut être beaucoup plus court; la surcharge imposée est moindre (moins de cycles CPU gaspillés); un mécanisme pratique permet de resoumettre les calculs qui ont échoué ou qui n'ont jamais été exécutés; et META peut traiter autant les tâches séquentielles que les tâches multifils, MPI, GPU et hybrides.

NOTE : Lisez cette page au complet pour savoir si cet outil peut servir dans vos travaux. Si c’est le cas, vous pourrez demander l'assistance de l’équipe technique pour modifier vos processus.

= Avantages =

Selon leur durée et leur nombre, plusieurs tâches séquentielles sont groupées dans une ou plusieurs tâches MPI.

Le fait de soumettre plusieurs tâches séquentielles en même temps peut ralentir l’ordonnanceur et causer de longs délais de réponse et des interruptions fréquentes dans l’exécution de sbatch ou squeue. La solution de GLOST est de grouper toutes les tâches séquentielles dans un même fichier nommé list_glost_tasks.txt et de soumettre une tâche MPI avec l’enveloppe glost_launch. Ceci diminue de beaucoup le nombre de tâches dans la queue et produit donc moins de demandes à traiter par l’ordonnanceur que si les tâches étaient soumises séparément. Pour soumettre plusieurs tâches séquentielles sans délai, GLOST atténue le fardeau de Slurm.

Avec GLOST, l’utilisateur  soumet et traite quelques tâches MPI plutôt que des centaines ou des milliers de tâches séquentielles.

= Modules =

GLOST utilise OpenMPI pour grouper des tâches séquentielles dans une tâche MPI. Vous devez charger OpenMPI et le module GLOST correspondant. Pour plus d’information, consultez Utiliser des modules. Pour voir les modules GLOST disponibles, utilisez la commande module spider glost. Avant de soumettre une tâche, assurez-vous de pouvoir charger GLOST et les autres modules nécessaires à l’exécution de votre application.

$  module spider glost/0.3.1

--------------------------------------------------------------------------------------------------------------------------------------
  glost: glost/0.3.1
--------------------------------------------------------------------------------------------------------------------------------------
    Description:
      This is GLOST, the Greedy Launcher Of Small Tasks.

    Properties:
      Tools for development / Outils de développement

    You will need to load all module(s) on any one of the lines below before the "glost/0.3.1" module is available to load.

      StdEnv/2023  gcc/12.3  openmpi/4.1.5
      StdEnv/2023  intel/2023.2.1  openmpi/4.1.5

    Help:

      Description
      ===========
      This is GLOST, the Greedy Launcher Of Small Tasks.

      More information
      ================
       - Homepage: https://github.com/cea-hpc/glost

Si un module OpenMPI se trouve déjà dans votre environnement, ce qui est le cas pour l’environnement par défaut, ajouter  module load glost à la liste des modules dont vous avez besoin est suffisant pour activer GLOST. Pour vous assurer que GLOST et les autres modules sont présents, lancez la commande module list.

= Utilisation =

== Syntaxe ==

Les formes suivantes sont possibles :

srun glost_launch list_glost_tasks.txt

mpiexec glost_launch list_glost_tasks.txt

mpirun glost_launch list_glost_tasks.txt

==Nombre de cœurs et nombre de tâches==

Les tâches séquentielles sont attribuées aux cœurs disponibles par une distribution cyclique. L’enveloppe (wrapper) GLOST commence par la première tâche (ou ligne dans la liste) et lui assigne un processeur. Ceci est répété jusqu’à la fin de la liste ou jusqu’à ce que la durée de la tâche soit atteinte. Le nombre de cœurs ne correspond pas nécessairement au nombre de tâches listées. Cependant, pour optimiser les ressources, assurez-vous que les tâches ont une durée d’exécution similaire et qu’elles peuvent être distribuées également sur le nombre de cœurs demandés. Examinons les cas suivants :

* Avec un grand nombre de tâches séquentielles très courtes (par exemple des centaines ou des milliers de tâches de quelques minutes chacune), soumettez une ou plusieurs tâches GLOST pour les exécuter en utilisant un nombre limité de cœurs. Vous pouvez soumettre les tâches avec une courte durée et par nœud afin de profiter du remplissage (backfilling) et de l’ordonnanceur.
* Avec des dizaines à des centaines de tâches relativement courtes (environ une heure), vous pouvez les grouper dans une ou plusieurs tâches GLOST.
* Avec plusieurs tâches de longue durée ayant des temps d’exécution similaires, vous pouvez aussi les regrouper dans une tâche GLOST.

== Estimation du temps d’exécution ==

Avant de lancer une tâche, essayez d’estimer son temps d’exécution; ceci peut servir à estimer le temps d’exécution de la tâche GLOST.
Supposons que votre tâche GLOST comprend un nombre Njobs  de tâches similaires où chacune utilise un temps t0 sur un (1) processeur. La durée totale sera alors de t0*Njobs.

Pour utiliser maintenant un nombre de cœurs Ncores, la durée sera de wt = t0*Njobs/Ncores.

Note : Une tâche MPI est souvent conçue pour que les processeurs puissent échanger de l'information entre eux, ce qui utilise souvent une grande part du temps pour communiquer plutôt que pour effectuer les calculs. Un grand nombre de petites communications dépendantes peut diminuer la performance du code, mais GLOST utilise MPI pour lancer des tâches séquentielles uniquement et donc, le surcoût en communication est relativement rare. Vous pouvez arriver au même résultat en utilisant directement MPI, mais GLOST est presque aussi efficace en plus de vous épargner l'écriture de code MPI.

== Besoins en mémoire ==

GLOST exécute des tâches séquentielles avec MPI et la mémoire par cœur devrait être la même que la mémoire utilisée par les tâches exécutées séparément. Dans le script Slurm, utilisez --mem-per-cpu plutôt que --mem.

== Créer la liste des tâches ==

Avant de soumettre une tâche, créez un fichier texte nommé list_glost_tasks.txt avec une tâche par ligne et les commandes pour chacune des tâches. Choisir des tâches ayant une durée d’exécution similaire permet d’optimiser les ressources utilisées. Les tâches peuvent être localisées dans un seul ou plusieurs répertoires. Si les tâches sont toutes dans le même répertoire, il faut éviter que les résultats utilisent les mêmes fichiers temporaires ou les mêmes fichiers en sortie; pour ce faire, les résultats peuvent être redirigés vers un fichier avec une variable qui indique l’option ou l’argument utilisé dans l’exécution de la tâche.  Dans le cas où les tâches utilisent les mêmes fichiers temporaires ou les mêmes fichiers en sortie, vous aurez peut-être besoin de créer un répertoire pour chaque tâche (un répertoire pour chaque option ou argument correspondant à une tâche particulière).

Note : Une tâche peut contenir une ou plusieurs commandes exécutées l’une à la suite de l’autre. Les commandes doivent être séparées par  &&.

Le fichier suivant list_glost_example.txt contient huit tâches.

Note : Cet exemple de script ne contient pas de commandes et il ne peut pas être exécuté. Il montre seulement

* la syntaxe de base pour la liste de tâches list_glost_tasks.txt qui servira d’argument pour glost_launch;
* un script type pour la soumission de tâches.

La liste des tâches et le script doivent être adaptés à votre contexte.

==Liste de tâches situées dans le même répertoire ==

GLOST peut être utilisé pour exécuter un ensemble ou une liste de tâches séquentielles dans un répertoire. Il faut éviter que les résultats utilisent les mêmes fichiers temporaires ou les mêmes fichiers en sortie en ajoutant des arguments pour différencier les tâches. Le prochain exemple contient 10 tâches dont chacune contient une ou plusieurs commandes qui seront exécutées l’une à la suite de l’autre.

* La première commande définit nargument qui peut être une variable ou un paramètre pouvant par exemple être passé au programme;
* la deuxième commande exécute le programme; pour les besoins du test, nous utilisons la commande sleep 360 que vous remplacerez par la ligne de commande pour votre application, par exemple ./my_first_prog < first_input_file.txt > first_output_file.txt;
* la troisième commande et les suivantes sont optionnelles; pour les besoins du test, nous utilisons echo ${nargument}.`hostname` > log_${nargument}.txt qui imprime l’argument et hostname vers le fichier log_${nargument}.txt. Comme c’est le cas pour la deuxième commande, cette ligne sera remplacée selon votre application, par exemple par ./my_second_prog < second_input_file.txt > second_output_file.txt.

Note : Dans cet exemple, nous utilisons 2 cœurs et une liste de 10 tâches. Les deux premières tâches (correspondant aux deux premières lignes) seront assignées par GLOST aux processeurs disponibles. Quand le ou les processeurs auront terminé le traitement des deux premières tâches, ils passeront à la tâche suivante et ainsi de suite jusqu’à la fin de la liste.

== Liste de tâches situées dans des répertoires différents ==

Dans ce cas, plusieurs tâches séquentielles sont exécutées dans des répertoires distincts, ce qui peut être utile pour éviter que les tâches se terminent de façon anormale ou que les résultats se chevauchent quand un programme utilise des fichiers temporaires ou des fichiers d’entrée/sortie avec des noms identiques. Il faut s’assurer que chaque tâche à ses fichiers d’entrée et son répertoire. Il est aussi possible d’utiliser les commandes comme dans l’exemple suivant :

== Redémarrer une tâche GLOST ==

Si vous avez mal évalué la durée d’exécution de votre tâche GLOST, il est possible qu’elle doive être redémarrée pour traiter toutes les tâches. Identifiez d’abord les tâches qui ont été exécutées et supprimez les lignes correspondantes dans la liste ou créez une nouvelle liste avec les tâches non exécutées. Soumettez à nouveau le script avec la nouvelle liste comme argument à glost_launch.

== Autres exemples ==

Si vous avez l'habitude de préparer des scripts, utilisez les exemples qui suivent et modifiez-les selon votre contexte.

Après avoir chargé le module GLOST, copiez les exemples dans votre répertoire avec la commande

cp -r $EBROOTGLOST/examples Glost_Examples

Les exemples copiés seront enregistrés dans le répertoire Glost_Examples.

= Références =

* META-Farm
* GNU parallel
* Vecteurs de tâches
* MPI
* Exécuter des tâches