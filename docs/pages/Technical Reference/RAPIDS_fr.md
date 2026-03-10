---
title: "RAPIDS/fr"
url: "https://docs.alliancecan.ca/wiki/RAPIDS/fr"
category: "Technical Reference"
last_modified: "2025-10-08T21:53:46Z"
page_id: 16703
display_title: "RAPIDS"
language: "fr"
---

=Description=

RAPIDS est une suite de bibliothèques de logiciels open source de NVIDIA qui sert principalement à l'exécution de pipelines de science des données et d'analyse en Python avec des GPU. La suite s'appuie sur CUDA pour l'optimisation des calculs de bas niveau et fournit des API Python conviviales semblables à celles de Pandas ou Scikit-learn.

Les principales composantes sont :

* cuDF : bibliothèque Python de DataFrames GPU (selon le format en colonne Apache Arrow) pour le chargement, la fusion, l'agrégation, la sélection et autres manipulations des données.

* cuML :  suite de bibliothèques pour l’implémentation d’algorithmes d’apprentissage machine et de fonctions primitives, qui permet le partage d’API compatibles avec d’autres projets RAPIDS.

* cuGraph : bibliothèque pour l’analyse de graphiques accélérée par GPU, qui offre une fonctionnalité semblable à NetworkX et est parfaitement intégrée à la plateforme [de science des données] RAPIDS.

* Cyber Log Accelerators (CLX ou clicks) :  collection d’exemples RAPIDS dans les domaines de la sécurité, la science des données et le génie, qui permet d’appliquer rapidement RAPIDS et l’accélération GPU à des cas concrets de cybersécurité.

* cuxFilter : bibliothèque de connecteurs pour relier facilement des bibliothèques de visualisation et des DataFrames GPU et permet aussi d’utiliser interactivement des graphiques de différentes bibliothèques dans le même tableau de bord.

* cuSpatial : bibliothèque C++/Python avec accélération GPU pour les systèmes d’information géographique incluant la recherche de points à l’intérieur d’un polygone, la jointure spatiale, les systèmes de coordonnées, les primitives de forme, les distances et l’analyse de trajectoires.

* cuSignal : accélération GPU dans le traitement des signaux avec  CuPy, Numba et l’écosystème RAPID. Dans certains cas, cuSignal est un port direct de Scipy Signal pour utiliser des ressources de calcul via CuPy, mais qui contient aussi des kernels Numba CUDA pour plus d’accélération à des fonctions sélectionnées

* cuCIM : boîte à outils extensible pour l’accélération GPU des entrées/sorties, la vision par ordinateur et le traitement des primitives, principalement dans le domaine de l’ imagerie médicale.

* RAPIDS Memory Manager (RMM) : outil de gestion des allocations de mémoire pour cuDF (C++ et Python) et les autres bibliothèques RAPIDS. RMM gère aussi le remplacement des allocations de mémoire CUDA et de la mémoire des périphériques CUDA et effectue rapidement les allocations et désallocations de manière asynchrone en réservant une quantité définie de mémoire.

= Images Apptainer =

Pour créer une image Apptainer (auparavant Singularity) pour RAPIDS, il faut d’abord trouver et sélectionner une image Docker fournie par NVIDIA.

==Trouver une image Docker==

À partir de RAPIDS v23.08, les deux types d’images Docker pour RAPIDS sont base et notebooks. Pour chaque type, plusieurs images sont fournies pour les différentes combinaisons des versions de RAPIDS et de CUDA ainsi que plusieurs versiond de Python. Pour trouver une image en particulier, allez sous l'onglet Tags de chacun des sites.

* RAPIDS Base : contient un environnement RAPIDS prêt à être utilisé pour soumettre une tâche à l'ordonnanceur.
* RAPIDS Notebooks : ajoute à l'image Base un serveur Jupyter notebook et des exemples de notebooks. Utilisez ce type d'image pour travailler en mode interactif avec des noteboooks et des exemples.

==Construire une image Apptainer==

Par exemple, le tag d'une image Docker pour RAPIDS est

 nvcr.io/nvidia/rapidsai/notebooks:25.04-cuda12.0-py3.12

Avec un ordinateur qui prend en charge Apptainer, vous pouvez construire une image Apptainer (ici rapids.sif) avec la commande suivante :

[name@server ~]$ apptainer build rapids.sif docker://nvcr.io/nvidia/rapidsai/notebooks:25.04-cuda12.0-py3.12

Le processus prend habituellement de 30 à 60 minutes. Puisque la taille de l’image est grande, assurez-vous que vous avez assez de mémoire et d’espace disque sur le serveur.

=Travailler sur une grappe=

Une fois que vous avez une image Apptainer pour RAPIDS sur une de nos grappes, vous pouvez demander une session interactive sur un nœud GPU ou soumettre une tâche en lot à l 'ordonnanceur quand votre code RAPIDS est prêt.

==Travailler interactivement sur un nœud GPU==

Si l’image Apptainer a été construite avec une image Docker de type notebooks, elle inclut un serveur Jupyter Notebook et peut être employée pour explorer RAPIDS interactivement sur un nœud de calcul GPU.
Pour demander une session interactive sur un nœud de calcul GPU, par exemple
[name@cluster-login ~]$ salloc --ntasks=1 --cpus-per-task=2 --mem=10G --gpus-per-node=1 --time=1:0:0 --account=def-someuser

Quand la ressource est allouée, lancez l’interpréteur RAPIDS sur le nœud
GPU avec

[name@compute-node#### ~]$ module load apptainer
[name@compute-node#### ~]$ apptainer shell --nv rapids.sif

* l'option --nv fait le bind mount du périphérique GPU de l’hôte sur le conteneur pour que l’accès au GPU puisse se faire de l’intérieur du conteneur Apptainer.

Lorsque l’invite de l’interpréteur change pour Apptainer>, vous pouvez consulter les statistiques pour le GPU dans l'interpréteur pour vous assurer que vous avez accès au GPU.

Apptainer> nvidia-smi

Lorsque l’invite change pour Apptainer>, vous pouvez lancer le serveur Jupyter Notebook server dans l’environnement RAPIDS; ceci affichera l’URL du serveur.
Apptainer> jupyter-lab --ip $(hostname -f) --no-browser

NOTE : À partir de la version 23.08, RAPIDS n'a pas besoin d'être activé après que Conda ait démarré puisque tous les paquets sont inclus dans l'environnnement Conda de base qui est activé par défaut dans ; par exemple, vous pouvez lancer le serveur Jupyter Notebook dans l'interpréteur du conteneur.

Si un nœud de calcul n’est pas connecté directement à l’internet, il faut configurer un tunnel SSH pour faire la redirection de port entre votre ordinateur et le nœud GPU. Pour les détails, voyez comment se connecter à Jupyter Notebook.

==Soumettre une tâche RAPIDS à l'ordonnanceur==

Quand votre code RAPIDS est prêt, vous pouvez soumettre une tâche à l'ordonnanceur. La bonne pratique est d'utiliser le disque local quand vous travaillez avec un conteneur sur un nœud de calcul.

Submission script

=Références=

* RAPIDS Docs: documentation complète pour RAPIDS, comment rester en contact et rapporter les problèmes;
* RAPIDS Notebooks: exemples sur GitHub que vous pouvez utiliser;
* RAPIDS on Medium: cas d’usage et blogues.