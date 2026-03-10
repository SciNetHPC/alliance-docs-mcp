---
title: "MrBayes/fr"
url: "https://docs.alliancecan.ca/wiki/MrBayes/fr"
category: "General"
last_modified: "2025-07-23T15:18:50Z"
page_id: 25993
display_title: "MrBayes"
language: "fr"
---

MrBayes est un programme d'inférence bayésienne et de choix de modèles sur une large gamme de modèles phylogénétiques et évolutifs. MrBayes utilise les méthodes de Monte Carlo par chaîne de Markov (MCMC) pour estimer la distribution a posteriori des paramètres du modèle.

== Trouver les modules disponibles==

Pour savoir comment trouver et sélectionner une version de MrBayes avec les commandes  module, consultez Utiliser_des_modules.

== Exemples ==

=== Travailler en séquentiel ===
Le script suivant demande un (1) seul cœur CPU (--cpus-per-task=1).
Dans cet exemple, on utilise un fichier en entrée (ici, primates.nex) fourni avec MrBayes.

Vous pouvez soumettre le script de la tâche avec

=== Travailler en parallèle ===
MrBayes permet d'utiliser des GPU et plusieurs cœurs sur plusieurs nœuds.

==== MPI ====
Le prochain script demande un total de huit (8) cœurs CPU, sur un ou plusieurs nœuds.
Il utilise un fichier en entrée (ici, primates.nex) fourni avec MrBayes.

Le script pour la tâche peut être soumis avec

==== GPU ====
Le script suivant demande un GPU et utilise un fichier en entrée (ici, primates.nex) fourni par MrBayes.

Vous pouvez soumettre le script de la tâche avec

== Utiliser des points de contrôle ==
Pour les tâches qui exigent beaucoup de temps d'exécution, nous vous recommandons de répartir le travail dans plusieurs petites tâches parce que les tâches de longue durée sont plus susceptibles  d'être interrompues par une panne de matériel ou des travaux de maintenance. Heureusement, MrBayes offre un mécanisme pour créer des points de contrôle qui vous permettent d'enregistrer le résultat d'une tâche et de poursuivre le travail avec une autre tâche.

Dans l'exemple suivant, le calcul se fait en deux temps dans deux tâches qui sont soumises l'une à la suite de l'autre. Nous avons créé les fichiers   job1.nex et job2.nex qui sont identiques, sauf pour la commande append sur la dernière ligne du deuxième script.

Créez ensuite le script pour la tâche. Dans cet exemple nous utilisons un vecteur de tâches. Dans ce cas-ci nous n'avons besoin que d'un (1) script et d'une (1) commande sbatch pour lancer les deux (2) tâches et donc l'ensemble des calculs. Voir Vecteur de tâches pour plus d'information au sujet du paramètre --array
et la variable $SLURM_ARRAY_TASK_ID.

Vous pouvez soumettre l'exemple avec