---
title: "Open OnDemand/fr"
url: "https://docs.alliancecan.ca/wiki/Open_OnDemand/fr"
category: "General"
last_modified: "2026-01-13T08:48:02Z"
page_id: 32254
display_title: "Open OnDemand"
language: "fr"
---

=Introduction=

Nous décrivons ici les étapes de base pour démarrer avec Open OnDemand (OOD) sur nos systèmes.

Open OnDemand est une plateforme web qui donne accès à un large éventail d'applications scientifiques et de ressources de calcul telles que Jupyter Lab, RStudio et VS Code. Elle vous permet d'interagir avec une de nos grappes via un navigateur web, sans avoir à installer de logiciel sur votre ordinateur local. Vous pourrez gérer des fichiers, soumettre et suivre des tâches, et exécuter des applications de manière interactive. Pour plus d'information sur ce projet, voir . Pour la documentation spécifique à chacune des grappes, voir

* Trillium : Guide de démarrage Open OnDemand
* Nibi, Accès via Open OnDemand (OOD)

=Se connecter au portail Open OnDemand=

Pour accéder au portail Open OnDemand, ouvrez un navigateur web et sélectionnez l'instance OnDemand, par exemple : Trillium, Vulcan ou Nibi. Saisissez votre nom d'utilisateur et votre mot de passe avec l'Alliance, puis effectuez l'authentification multifacteur via Duo ou Yubikey. La connexion étant établie, le tableau de bord Open OnDemand sera affiché. Vous pourrez alors accéder aux différents outils et applications disponibles.

=Gestion des fichers=

Un navigateur de fichiers permet de parcourir vos fichiers et répertoires du système de fichiers. Pour y accéder, cliquez sur l'onglet Files et sélectionnez le répertoire dans le menu déroulant (HOME, SCRATCH ou PROJECT). Avec l'interface,vous pouvez

* Naviguer dans vos répertoires
* Téléverser et télécharger des fichiers
* Créer des fichiers et des répertoires
* Supprimer des fichiers et des répertoires
* Modifier des fichiers existants

==Téléverser des fichiers==

La taille maximale des fichiers à téléverser est présentement de 10Go. Pour téléverser un fichier plus volumineux ou si vous rencontrez des problèmes de téléversement (par exemple, en raison d'une mauvaise connexion Internet), utilisez Globus Globus. Un bouton Globus est disponible en haut à droite de l'explorateur de fichiers  300px. Cliquez sur ce bouton pour faire afficher l'interface web de Globus où vous pourrez vous connecter avec votre nom d'utilisateur et votre mot de passe avec l'Alliance. Le chemin d'accès affiché le navigateur Open OnDemand sera identique à celui ouvert dans Globus.

== Soumettre une tâche ==

Open OnDemand offre une interface pour soumettre des tâches en lots. Sous l'onglet Jobs, sélectionnez Job Composer pour faire afficher le formulaire de soumission. Cliquez ensuite sur le bouton New Job pour faire afficher les options suivantes :

* From Default Template, pour créer une nouvelle tâche;
* From Template, pour sélectionner une tâche, par exemple MPI et OpenMP;
* 'From Specified Path, pour utiliser une tâche dans le système de fichiers;
* From Selected Job, pour copier la tâche active.

Vous pouvez spécifier les paramètres de la tâche, tels que le script et le nom du compte avec le bouton Job Options. Les autres paramètres, comme le nombre de nœuds, le nombre de cœurs, le temps d'exécution, etc., peuvent être modifiés directement dans le script en cliquant sur le bouton Open Editor. Une fois les champs obligatoires remplis, cliquez sur le bouton Submit.

Cette page affiche également l'état de votre tâche : en attente, en cours d'exécution ou terminée. Une fois la tâche terminée, vous pouvez consulter les journaux de sortie et d'erreurs en cliquant sur un fichier dans la section Folder Contents dans le panneau de droite Job Details.

=Suivi des tâches=

Pour obtenir une vue d'ensemble de toutes vos tâches en attente, utilisez l'interface de suivi des tâches. Sous l'onglet Jobs sélectionnez Active Jobs. Vous pouvez utiliser le champ Filter en haut à droite. Vous pouvez trier les colonnes en cliquant sur leur en-tête, par exemple par état (en cours, terminée, échouée, etc.). Cliquer sur le symbole > à gauche d'une tâche affiche des informations supplémentaires, telles que l'heure de début et de fin, la liste des nœuds et le compte facturé. Pour afficher toutes les tâches en attente, cliquez sur le menu déroulant en haut à droite et sélectionnez All Jobs.

== Applications interactives ==

Open OnDemand offre aussi des applications interactives exécutables directement depuis votre navigateur web. Sous l'onglet Interactive Apps, sélectionnez l'application voulue dans le menu déroulant. Ceci affiche la page de soumission des tâches, où vous pouvez choisir les paramètres de votre tâche, par exemple

* Durée de la tâche (en heures)
* Nombre de cœurs
* Quantité de mémoire à allouer (en Go)
* Notification par courriel du début de la tâche

Après avoir sélectionné les paramètres de votre tâche, cliquez sur le bouton 'Launch pour l'ajouter à la file d'attente. La page My Interactive Sessionssera affichée où vous pouvez consulter l'état de votre tâche (en file d'attente, en cours d'exécution ou terminée). Une fois la tâche affectée à un nœud et en cours d'exécution, cliquez sur le bouton Connect to ... pour lancer l'application. Celle-ci s'ouvre dans un nouvel onglet où vous pouvez travailler avec la tâche comme si elle était exécutée localement.

If you would like terminal access to the node where the application is running, to monitor the performance for example you can click on the button beside Host starting with >_. This will open a terminal window in your browser where you can run commands on the node directly.

If for whatever reason you would like to kill the job, you can do so by clicking on the red Delete button in the job panel in the My Interactive Sessions page.

    Figure 5 : Formulaire

    Figure 6 : Ouverture d'une session interactive

    Figure 7 : Application interactive

=Terminal access=

Sometimes you might prefer to use a terminal to interact with the cluster, Open OnDemand provides a web-based terminal that you can use to access the command line interface. To access the terminal, navigate to the Clusters tab and select Cluster_Name Shell Access. This will open a new tab in your browser with a terminal window where you can run commands as you would in a regular terminal session.

= Debogage =

If you encounter any errors while using an interactive Open OnDemand job, you can check the logs for more information. To access the logs, navigate to the My Interactive Sessions tab and find your active session. Click on the output.log link (see Figure. 8) to open a separate tab which displays the output of your job. This file contains the standard output and error messages generated by the job, which can help you identify any issues that may have occurred during the session. When submitting a ticket to SciNet support, please include the output.log file, your Session ID, which is displayed as a long string of characters, e.g.  8feb45fa-bc65-4846-8398-2a73c1bf8e5a, and any other relevant information to help us assist you more effectively.