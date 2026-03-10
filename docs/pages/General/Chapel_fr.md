---
title: "Chapel/fr"
url: "https://docs.alliancecan.ca/wiki/Chapel/fr"
category: "General"
last_modified: "2025-07-27T17:00:28Z"
page_id: 17444
display_title: "Chapel"
language: "fr"
---

Chapel est un langage de programmation parallèle compilé de haut niveau à usage général avec des abstractions intégrées pour le parallélisme à mémoire partagée et distribuée. Chapel offre deux styles de programmation parallèle : (1) le parallélisme de tâches, où le parallélisme se fait par des tâches spécifiées par programmation, et (2) le parallélisme de données, où le parallélisme se fait en effectuant les mêmes calculs sur des sous-ensembles de données qui peuvent se trouver dans la mémoire partagée d'un nœud unique ou être distribués sur plusieurs nœuds.

Ces abstractions de haut niveau font de Chapel l'outil idéal pour apprendre la programmation parallèle pour le calcul de haute performance. Ce langage est incroyablement intuitif et s'efforce de fusionner la facilité d'utilisation de Python avec les performances des langages compilés traditionnels tels que C et Fortran. Les blocs parallèles qui prennent généralement des dizaines de lignes de code MPI peuvent être exprimés en seulement quelques lignes de code Chapel. Chapel est open source et peut fonctionner sur n'importe quel système d'exploitation de type Unix, avec une prise en charge matérielle des ordinateurs portables aux grands systèmes de CHP.

Chapel a une base d'utilisateurs relativement petite, donc de nombreuses bibliothèques qui existent pour C, C++ et Fortran n'ont pas encore été implémentées dans Chapel. Espérons que cela changera dans les années à venir, si l'adoption de Chapel continue de prendre de l'ampleur dans la communauté de CHP.

Pour plus d'information, voyez nos webinaires Chapel.

== Calculs simples ==

Le module chapel-multicore est utilisé sur nos grappes d'usage général avec un nœud unique et une mémoire partagée seulement. Vous pouvez utiliser salloc pour tester si votre code fonctionne en séquentiel.
0:30:0 --ntasks1 --mem-per-cpu3600 --accountdef-someprof
|chpl test.chpl -o test
|./test
}}
ou avec plusieurs cœurs sur un même nœud :
0:30:0 --ntasks1 --cpus-per-task3 --mem-per-cpu3600 --accountdef-someprof
|chpl test.chpl -o test
|./test
}}
Pour les tâches de production, veuillez préparer un script de soumission de tâche et le soumettre avec sbatch.

== Calculs distribués ==

Pour des tâches avec plusieurs nœuds et une mémoire hybride (partagée et distribuée) sur nos grappes InfiniBand, chargez le module chapel-ofi.

Le code suivant imprime l'information de base au sujet des nœuds disponibles dans votre tâche.

Pour exécuter ce code sur june grappe InfiniBand, vous devez charger le modulechapel-ucx.
0:30:0 --nodes4 --cpus-per-task3 --mem-per-cpu3500 --accountdef-someprof
}}

Une fois que la [[Running_jobs/fr#Tâches_interactives|] tâche interactive] est lancée, vous pouvez compiler et exécuter votre code à partir de l'invite sur le premier nœud de calcul alloué.

Pour les tâches de production, veuillez préparer un script de soumission de tâche et soumettre la tâche avec sbatch.

== Calcul distribué avec les GPU NVIDIA ==

Pour utiliser un GPU, chargez le module chapel-ucx-cuda qui supporte les GPU NVIDIA sur nos grappes InfiniBand.

Ceci est du code de base pour utiliser un GPU avec Chapel.

Pour exécuter ce code sur une grappe InfiniBand, chargez le module chapel-ucx-cuda.
0:30:0 --mem-per-cpu3500 --gpus-per-node1 --accountdef-someprof
}}

Une fois que la [[Running_jobs/fr#Tâches_interactives|] tâche interactive] est lancée, vous pouvez compiler et exécuter votre code à partir de l'invite sur le nœud de calcul alloué.

Pour les tâches de production, veuillez préparer un script de soumission de tâche et soumettre la tâche avec sbatch.