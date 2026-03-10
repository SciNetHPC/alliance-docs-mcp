---
title: "Using Conda in Apptainer/fr"
url: "https://docs.alliancecan.ca/wiki/Using_Conda_in_Apptainer/fr"
category: "General"
last_modified: "2025-12-03T15:06:43Z"
page_id: 32063
display_title: "Utiliser Conda avec Apptainer"
language: "fr"
---

Avant de commencer ce tutoriel, prenez note de quelques points importants :

* Même dans un conteneur, Conda ne devrait pas être votre solution préférée. Accordez plutôt la priorité aux modules de notre pile logicielle et aux wheels parmi ceux qui sont disponibles. Ces modules et wheels sont optimisés en fonction de nos systèmes et nous pouvons fournir une meilleure assistance au besoin. Pour faire ajouter un module ou un paquet, contactez le soutien technique.
* Dans ce tutoriel, nous utilisons le gestionnaire de paquets micromamba au lieu de Conda. Si vous voulez utiliser Conda, vous devez tenir compte des  conditions d'utilisation d'Anaconda et peut-être  détenir une licence commerciale.
* Dans ce tutoriel, nous créons une image pour lecture seule, c'est-à dire un fichier .sif qui contient un environnement Conda avec tout ce qu'il faut pour utiliser votre application. Il est fortement recommandé de ne pas installer interactivement un logiciel dans un conteneur avec Conda et aucune information ne sera donnée en ce sens.

La création d'une image Apptainer et l'installation d'un logiciel dans un conteneur avec Conda est un processus en trois étapes.
Il faut d'abord créer un fichier .yml qui décrit l'environnement Conda à créer dans le conteneur; dans l'exemple suivant, il s'agit de environment.yml. Ce fichier contient le nom de l'environnement à créer, la liste des paquets à installer et comment les trouver (channel).

Il faut ensuite créer un  fichier de définition pour l'image (nommé ici image.def) qui décrit les étapes pour créer l'image avec Apptainer.
#Téléchargez une image Docker de DockerHub qui contient le gestionnaire de paquets micromamba préinstallé.
#Créez dans le conteneur une copie du fichier de définition environment.yml.
#Exécutez micromamba pour configurer l'environnement environment.yml.

La dernière étape est de construire l'image Apptainer à l'aide du fichier de définition ci-dessus :
   module load apptainer
   APPTAINER_BIND=' ' apptainer build image.sif image.def

Vous pouvez maintenant tester si multiqc est disponible avec, par exemple, la commande