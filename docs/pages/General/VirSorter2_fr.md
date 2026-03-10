---
title: "VirSorter2/fr"
url: "https://docs.alliancecan.ca/wiki/VirSorter2/fr"
category: "General"
last_modified: "2024-07-15T19:44:11Z"
page_id: 24650
display_title: "VirSorter2"
language: "fr"
---

__TOC__

L'outil VirSorter2 permet d’identifier les nouvelles séquences de virus.

Nous abordons ici l’installation et l’utilisation de VirSorter2 v2.2.4.

Le code source et la documentation pour VirSorter2 se trouvent sur leur page GitHub.

N’oubliez pas de citer VirSorter2 si vous l’utilisez pour vos analyses.

== Installation dans un environnement virtuel Python ==
Les étapes ci-dessous servent à installer VirSorter2 dans votre répertoire $HOME avec nos wheels Python préconstruits. Les wheels personnalisés se trouvent dans /cvmfs/soft.computecanada.ca/custom/python/wheelhouse/. Pour installer un wheel VirSorter2 dans un  environnement virtuel Python, nous utilisons la commande pip.

1. Chargez les modules nécessaires.

2. Créez et activez un environnement virtuel Python.

3. Installez VirSorter2 v2.2.4 dans l’environnement virtuel.
2.2.4
}}
4. Validez l'installation.

5. Gelez l’environnement et les éléments requis (requirements.txt).

6. Téléchargez la base de données dans votre répertoire $SCRATCH en utilisant l'option --skip-deps-install pour ne pas installer conda et aussi parce que les dépendances sont déjà installées.

== Tester VirSorter2 ==
1. Désactivez votre environnement virtuel.

2. Téléchargez l’ensemble de données dans votre répertoire $SCRATCH.

3. Créez un script pour soumettre une tâche à l’ordonnanceur.

3. Lancez une tâche interactive.
2G --cpus-per-task2 --account
}}
 salloc: Granted job allocation 1234567
 $ bash test-virsorter.sh             # Run the submission script
 $ exit                               # Terminate the allocation
 salloc: Relinquishing job allocation 1234567

Si le test est réussi, vous pouvez utiliser la commande sbatch pour soumettre une tâche avec votre propre ensemble de données.