---
title: "Pgdbg/fr"
url: "https://docs.alliancecan.ca/wiki/Pgdbg/fr"
category: "General"
last_modified: "2019-11-01T19:34:38Z"
page_id: 11636
display_title: "PGDBG"
language: "fr"
---

= Description =
PGDBG est un outil simple mais puissant pour le débogage d’applications parallèles MPI et OpenMP sous Linux. L’outil fait partie du paquet du compilateur PGI et est configuré pour OpenMP avec fils parallèles. Il peut être utilisé  en mode graphique avec redirection X11 ou en mode ligne de commande.

Un débogueur GNU comme GDB conviendra pour la plupart des programmes C, C++ ou Fortran77. Cependant, GDB ne fonctionne pas très bien avec les programmes Fortran 90/95; c’est pourquoi le Portland Group a développé pgdbg.

=Guide de démarrage=

Le travail avec PFDBG s’effectue généralement en deux étapes :
#compilation : le code est compilé (avec l’option -g pour obtenir les symboles de débogage);
#exécution et débogage : le code est exécuté et les résultats sont analysés.
Le débogage peut se faire en mode ligne de commande ou en mode graphique.

== Modules d’environnement ==
Il faut d’abord charger le  module pour le paquet PGI. Pour connaître les versions disponibles pour les modules du compilateur, de MPI et de CUDA que vous avez chargés, lancez module avail pgi. Pour connaître la liste complète des modules PGI disponibles, lancez  module -r spider '.*pgi.*'. En date de décembre 2018, les versions disponibles sont
*pgi/13.10
*pgi/17.3
Pour charger un module, lancez module load pgi/version; par exemple, pour la version 17.3, la commande est

== Compilation ==
Avant d’être débogué, le code doit d’abord être compilé en ajoutant l’indicateur -g pour obtenir les renseignements utiles au débogage.

==Exécution et débogage en mode ligne de commande==
Une fois le code compilé avec les options appropriées, lancez PGDBG pour effectuer l’analyse. Par défaut, l’affichage se fait par l’interface graphique. Si toutefois vous ne voulez pas utiliser cette interface ou que vous n’avez pas la redirection X11, vous pouvez travailler en mode ligne de commande en ajoutant l’option text au lancement de PDGDB.

En mode ligne de commande, une invite est affichée.

À l’invite, lancez la commande run.

En cours d’exécution du programme, PGDBG s’attache automatiquement aux fils et décrit chacun d’eux au fur et à mesure qu’ils sont créés. En cours de débogage, PGDBG travaille sur un seul fil à la fois, le fil courant.
La commande thread sert à sélectionner le fil courant,
la commande threads liste les fils utilisés à ce moment par un programme actif.

Par exemple, pour sélectionner ID 2 comme fil courant, la commande thread serait

==Exécution et débogage en mode graphique==
L’interface graphique est utilisée par défaut. Si vous avez configuré la redirection X11, PGDBG démarre en mode graphique dans une nouvelle fenêtre.

Les éléments de l'interface graphique sont :
* barre de menus
* barre d'outils
* volet du code source
* volet des entrées sorties (I/O)
* volet de débogage.

=== Barre de menus ===
La barre de menus principale affiche File, Edit, View, Connections, Debug et Help. Il est possible de naviguer avec la souris ou avec les raccourcis-clavier.

=== Barre d'outils principale ===
La barre d'outils principale contient plusieurs boutons et quatre listes déroulantes. La première liste Current Process, montre le processus en cours, autrement dit, le fil courant. Le libellé change selon que le processus ou le fil est décrit. Quand plusieurs fils sont disponibles, cette liste sert à sélectionner le processus ou le fil qui devrait être courant.

La deuxième liste Apply détermine le groupe de processus et de fils auxquels les commandes d'action s'appliquent.
La troisième liste Display détermine le groupe de processus et de fils auxquels les commandes d'affichage de données s'appliquent.

La quatrième liste File affiche le fichier source qui contient la cible courante.

=== Volets du code source et des outils de débogage ===
Le volet du code source montre le code pour la session en cours. Ce volet et les onglets du volet de débogage sont des éléments ancrables; en double-cliquant dessus, vous pouvez les détacher de la fenêtre principale.

=== Volet des entrées/sorties du programme ===
Les résultats produits par le programme sont affichés dans ce volet. Utilisez le champ Input pour faire des entrées au programme.

=== Volet de débogage ===
Situé au bas de la fenêtre, ce volet comporte des onglets qui servent différentes fonctions de débogage et de visualisation de l'information.

= Références =
* PGI Debugger User's Guide
* site web de PGI