---
title: "CP2K/fr"
url: "https://docs.alliancecan.ca/wiki/CP2K/fr"
category: "General"
last_modified: "2023-04-25T15:58:52Z"
page_id: 17089
display_title: "CP2K"
language: "fr"
---

CP2K est un paquet logiciel pour la chimie quantique et la physique des solides qui permet de faire des simulations atomistiques de systèmes solides, liquides, moléculaires, périodiques, matériels, cristallins et biologiques.

== Versions ==

La plus récente version installée est CPK2 8.2. Pour charger le module compilé avec GCC, lancez la commande

 module load StdEnv/2020  gcc/9.3.0  openmpi/4.0.3 cp2k/8.2

Vous pouvez aussi utiliser la version compilée avec Intel, mais elle semble moins stable car elle plante à l'occasion pour des raisons inconnues.

 module load StdEnv/2020  intel/2020.1.217  openmpi/4.0.3 cp2k/8.2

== Exemple de tâche ==

Nous utilisons ici l'exemple de calcul statique tiré du site web de CP2K.

Connectez-vous à une grappe et téléchargez les fichiers requis avec

 wget https://www.cp2k.org/_media/static_calculation.tgz
 tar xvfz static_calculation.tgz
 cd static_calculation/sample_output_no_smearing

Dans ce répertoire, créez le script de tâche suivant en utilisant le nom de votre compte.

Pour soumettre cette tâche, lancez

 sbatch mpi_job.sh

Pour vérifier que la tâche est terminée, lancez

 sq

Votre tâche est terminée si elle ne paraît pas dans la liste.

Le résultat de CP2K sera dans le fichier Si_bulk8.out. Il y aura aussi un fichier de résultats nommé slurm-*.out qui sera vide si le calcul s'est effectué sans erreurs.

== Fils et MPI ==

À partir de la version 8.2, l'installation de CP2K fournit l'exécutable cp2k.popt et l'exécutable OpenMP/MPI cp2k.psmp qui peuvent améliorer la performance de certains calculs. Avec notre test, nous avons obtenu une amélioration de 10 % avec l'essai QS/H2O-512.inp en utilisant 2 fils par processus MPI, en comparaison de l'exécution de cp2k.popt en MPI seul; dans les deux cas, le total de cœurs CPU était identique.

L'exemple ci-dessous est un fichier OpenMP/MPI pour la soumission d'une tâche sur Béluga. Sur les autres grappes, modifiez le nombre de tâches pour correspondre au nombre de cœurs disponibles sur les nœuds de chaque grappe. La différence en performance avec l'utilisation de fils dépend du problème traité. Dans certains cas, l'exécutable cp2k.psmp peut prendre plus de temps et il est important de faire des essais avec votre code pour pouvoir choisir la meilleure option.