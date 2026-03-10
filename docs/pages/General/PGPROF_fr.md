---
title: "PGPROF/fr"
url: "https://docs.alliancecan.ca/wiki/PGPROF/fr"
category: "General"
last_modified: "2020-11-04T17:57:22Z"
page_id: 9645
display_title: "PGPROF"
language: "fr"
---

PGPROF est un outil simple mais puissant pour l'analyse de programmes parallèles écrits avec OpenMP, MPI, OpenACC ou CUDA. Le profilage peut s’effectuer en mode ligne de commande ou en mode graphique.

=Utilisation=
Avec les deux modes, le travail se fait généralement en deux étapes :
#collecte des données, par l’exécution de l’application avec le profilage activé;
#analyse, par la visualisation des données obtenues à la première étape.

== Modules d'environnement ==
Pour utiliser PGPROF, vous devez charger le module approprié.

Comme PGPROF fait partie du paquet pour le compilateur PGI, lancez la commande module avail pgi pour connaître les versions disponibles pour les modules de compilation, MPI et CUDA que vous avez déjà chargés. Pour la liste des modules PGI disponibles, lancez module -r spider '.*pgi.*'.
En date de décembre 2018, les modules disponibles sont :
* pgi/13.10
* pgi/17.3

Sélectionnez une version avec module load pgi/version; pour charger la version 17.3 du compilateur PGI, la commande est

== Compilation du code ==
Pour obtenir de PGPROF une information utile, vous devez d’abord compiler le code avec un des compilateurs PGI, soit pgcc pour C, pgc++ pour C++ , pgfortran pour Fortran. Il est possible que du code source Fortran doive être compilé avec l’indicateur -g.

== Mode ligne de commande ==

Collecte des données : La première étape est d’utiliser PGPROF pour exécuter l’application, puis d’enregistrer les données sur la performance. Dans l’exemple suivant, a.out est l’application et a.prof est le fichier où les données sont enregistrées.

Le fichier de données peut être sauvegardé et ensuite analysé en mode graphique avec la commande File > Import (voir Mode graphique) ci-dessous ou en mode ligne de commande comme suit :
Analyse : Pour l’étape de visualisation, utilisez :

Les résultats sont généralement présentés en plusieurs catégories, par exemple :
* profil de la performance du noyau (kernel) GPU
* profil de l’exécution de l’API de CUDA
* profil de l’exécution d’OpenACC
* profil de la performance des fonctions du CPU

===Options===
*Pour faire afficher les résultats d’une seule catégorie, par exemple les résultats en rapport avec le CPU : --cpu-profiling.

*Pour faire afficher d’abord les résultats pour la fonction principale, suivis de ceux pour les fonctions subordonnées : --cpu-profiling-mode top-down.

*Pour savoir quelle partie de l’application requiert le plus de temps d’exécution : --cpu-profiling-mode bottom-up où les résultats pour chacune des fonctions sont suivis par ceux de la fonction qui l’appelle, remontant jusqu’à la fonction principale.

== Mode graphique ==

En mode graphique, la collecte et l’analyse des données peuvent souvent se faire dans la même session. Il est toutefois possible d'analyser les données d'un fichier produit en ligne de commande.
Collecte de données
* Lancez le profileur  PGI.
** Comme l’interface utilisateur de PGPROF est basée sur Java, elle devrait être exécutée sur le nœud de calcul dans la session interactive plutôt que sur le nœud de connexion puisque que dernier n’a pas suffisamment de mémoire (voir Java pour plus d’information). Pour activer la redirection X11, la session interactive peut être démarrée avec salloc --x11 ... (voir Tâches interactives pour plus d'information).
* Démarrez une nouvelle session, avec File > New Session.
* Sélectionnez le fichier exécutable à profiler et ajouter les arguments de profilage, s’il y a lieu.
* Cliquez sur Next, puis Finish.
Analyse
* Dans le volet CPU Details, cliquez sur le bouton Show the top-down (callers first) call tree view.

La fenêtre de visualisation des données comporte quatre volets.
- Dans le volet supérieur, la partie de droite montre tous les événements selon le temps de leur exécution.
- GPU Details: montre la performance des noyaux (kernels) GPU.
- CPU Details: montre la performance des fonctions du CPU.
- Properties: information détaillée pour la fonction sélectionnée dans le volet supérieur.

= Références =
PGPROF est produit par PGI, une filiale de NVIDIA Corporation.
* Guide de démarrage
* Guide de l'utilisateur