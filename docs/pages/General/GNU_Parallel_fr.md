---
title: "GNU Parallel/fr"
url: "https://docs.alliancecan.ca/wiki/GNU_Parallel/fr"
category: "General"
last_modified: "2025-04-10T03:01:24Z"
page_id: 585
display_title: "GNU Parallel"
language: "fr"
---

== Introduction ==
parallel est un outil de la suite GNU qui permet d'exécuter plusieurs tâches séquentielles en parallèle sur un ou plusieurs nœuds. Il s'agit d'un outil particulièrement utile pour exécuter un grand nombre de tâches séquentielles de courte durée ou de durée variable, sur un jeu de paramètres (exploration de paramètres). Cette documentation traite des notions de base seulement; pour plus d'information, consultez la documentation du produit.

Par défaut, parallel maximise l'utilisation de la ressource en exécutant autant de tâches que le nombre de cœurs alloués par l'ordonnanceur. Vous pouvez changer ceci avec l'option --jobs, suivie du nombre de tâches que GNU Parallel devrait exécuter simultanément. Quand une tâche est terminée, parallel commence automatiquement la prochaine; ainsi, le nombre maximum de tâches est toujours en exécution.

==Commandes de base==
Les accolades {} indiquent les paramètres passés en argument à la commande à exécuter.  Ainsi, pour exécuter la commande gzip sur tous les fichiers texte d'un répertoire, vous utiliserez gzip  ainsi
 parallel gzip  }}

On peut aussi utiliser :::, comme dans l'exemple suivant
 ::: $(seq 1 3)
|result=
1
2
3
}}

Les commandes de GNU Parallel sont appelées jobs.  Il ne faut pas confondre ces jobs avec les tâches (aussi des jobs) qui sont des scripts exécutés par l'ordonnanceur;  dans ce contexte, les tâches exécutées avec parallel sont des sous-tâches.

== Spécifier plusieurs arguments ==
Vous pouvez aussi utiliser plusieurs arguments en les numérotant ainsi
1 2 ::: $(seq 1 3) ::: $(seq 2 3)
|result=
1 2
1 3
2 2
2 3
3 2
3 3
}}

== Utiliser le contenu d'un fichier comme liste d'arguments ==
La syntaxe :::: permet d'utiliser le contenu d'un fichier comme valeurs des arguments. Ainsi, si votre liste de paramètres est dans le fichier maliste.txt, vous pouvez faire afficher son contenu ainsi :
1 :::: maliste.txt}}

== Utiliser le contenu d'un fichier comme liste de commandes ==
Les lignes dans un fichier peuvent représenter des sous-tâches à exécuter en parallèle; dans ce cas, chaque sous-tâche doit être sur une ligne distincte.  Ainsi, si votre liste de sous-tâches se trouve dans le fichier my_commands.txt, vous pouvez la faire exécuter ainsi :

Remarquez qu'aucune commande ou argument n'est passé à Parallel. Ce mode d'utilisation est particulièrement utile si les sous-tâches contiennent des symboles spécifiques de GNU Parallel ou si les sous-commandes contiennent quelques commandes, par exemple cd dir1 && ./executable.

L'exemple suivant montre comment faire exécuter une tâche par Slurm avec GNU Parallel. La liste des commandes dans my_commands.txt sera exécutée en séquence avec 4 CPU. Dès qu'une commande est terminée, une nouvelle commande est lancée afin de toujours avoir 4 commandes en marche à la fois, jusqu'à la fin de la liste.

== Utiliser plusieurs nœuds ==

Vous pouvez aussi distribuer votre travail sur plusieurs nœuds d'une grappe, comme dans l'exemple suivant  :

Nous créons ici un fichier qui contient la liste des nœuds pour indiquer à GNU parallel quels sont ceux à utiliser pour distribuer les tâches. L'option --env nous permet de transférer à tous les nœuds une variable d'environnement particulière et l'option --workdir fait en sorte que les tâches GNU parallel seront lancées dans le même répertoire que le nœud principal.

Par exemple, quand plusieurs tâches OpenMP sont soumises ensemble avec --nodes=N, --ntasks-per-node=5 et --cpus-per-task=8, la commande suivante va gérer tous les processus à démarrer sur tous les nœuds réservés, ainsi que le nombre de fils OpenMP par processus.
$SLURM_CPUS_PER_TASK
}}

Dans ce cas, 5*N processus OpenMP travaillent simultanément et l'utilisation de CPU peut aller jusqu'à 800 %.

==Suivi des commandes exécutées ou  des commandes ayant échoué; fonctionnalités de redémarrage==
L'argument --joblog JOBLOGFILE produit le journal des commandes exécutées. Le fichier JOBLOGFILE contient alors la liste des commandes complétées avec l'heure de début, la durée, le nom du nœud de calcul et le code de sortie, par exemple
 parallel --joblog gzip.log gzip  }}

Cette fonction offre plusieurs options pour le redémarrage.  Si la commande  parallel était interrompue (c'est-à-dire que la tâche prenait plus de temps que spécifié), elle pourrait reprendre son cours en utilisant l'option --resume, par exemple
 parallel --resume --joblog gzip.log gzip  }}
Les nouvelles tâches seront ajoutées à la fin du même journal.

Si certaines des sous-commandes ont échoué (c'est-à-dire que le code de sortie est différent de zéro) et vous pensez que l'erreur est résolue, ces sous-commandes peuvent être exécutées à nouveau avec  --resume-failed, par exemple
 parallel --resume-failed --joblog gzip.log gzip  }}
Ceci exécute également les sous-tâches qui n'étaient pas prises en compte auparavant.

==Travailler avec des fichiers volumineux==
Si par exemple nous voulons compter en parallèle le nombre de caractères dans le grand fichier FASTA nommé database.fa à l'aide d'une tâche de 8 cœurs, nous devons utiliser les arguments --pipepart et --block pour gérer efficacement de grandes portions du fichier.

En variant la taille de block nous avons :

 	Quantité de cœurs	Taille de la BD	Taille des blocs	Quantité de tâches parallèles	Cœurs utilisés	Durée de la tâche
1	8                	827Mo          	10Mo            	83                           	8             	0m2.633s
2	8                	827Mo          	100Mo           	9                            	8             	0m2.042s
3	8                	827Mo          	827Mo           	1                            	1             	0m10.877s
4	8                	827Mo          	-1              	8                            	8             	0m1.734s

Nous constatons que le fait d'utiliser la bonne taille de bloc a un impact réel sur l'efficacité et la quantité de cœurs utilisés.
Sur la première ligne, la taille des blocs est trop petite et plusieurs tâches sont distribuées sur les cœurs disponibles. Sur la deuxième ligne, la taille des blocs est plus appropriée puisque la quantité de tâches se rapproche de la quantité de cœurs disponibles.
Sur la troisième ligne, la taille des blocs est trop grande et un seul cœur est utilisé sur les 8, ce qui n'est pas efficace.
Sur la dernière ligne, on remarque que de laisser GNU Parallel s'adapter et décider de lui-même de la taille des blocs est souvent plus rapide.

== Exécuter des centaines ou des milliers de simulations ==
Commencez par déterminer la quantité de ressources nécessaires pour une simulation; vous pourrez ensuite déterminer la quantité totale de ressources requises par la tâche.

Dans les exemples suivants, les scripts de soumission sont pour 1 simulation en série avec 2Go de mémoire, 1  cœur et 5 minutes, et 1000 simulations. Avec 1 cœur, la durée serait de 83,3 heures.

Avec 1 nœud de 32 cœurs, la durée serait de 6 heures. Il serait aussi possible d'utiliser plus d'un nœud (voir #Utiliser plusieurs nœuds).

=== Liste d'arguments ===
Comme mentionné dans la section #Utiliser le contenu d'un fichier comme liste d'arguments, vous pouvez utiliser un fichier qui contient tous les paramètres. Dans ce cas, les paramètres sont séparés par un caractère de tabulation (\t) et chaque ligne correspond à une simulation.

=== Liste de commandes ===
Comme mentionné dans la section #Utiliser le contenu d'un fichier comme liste de commandes, vous pouvez utiliser un fichier qui contient toutes les commandes et leurs paramètres.

=== Plusieurs arguments ===
Vous pouvez utiliser GNU Parallel pour générer les paramètres et les associer aux commandes.

==Voir aussi==
* META
* GLOST
* Vecteurs de tâches