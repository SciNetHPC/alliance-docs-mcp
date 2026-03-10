---
title: "Visualization/fr"
url: "https://docs.alliancecan.ca/wiki/Visualization/fr"
category: "General"
last_modified: "2025-10-21T16:16:53Z"
page_id: 615
display_title: "Visualisation"
language: "fr"
---

==Paquets populaires==

=== ParaView ===
ParaView est un outil d'usage général de visualisation tridimensionnelle pour les domaines scientifiques.
Ce logiciel libre fonctionne sous Linux, Windows et Mac; traite la plupart des formats de données; offre plusieurs modes de rendu; utilise les scripts Python; et peut gérer des dizaines de milliers de processeurs pour produire des rendus à partir de grands ensembles de données.

* page wiki de l'Alliance
*  documentation
* gallery
* wiki
* Python scripting

=== VisIt ===
Semblable à ParaView, le logiciel libre d'usage général VisIt est un outil d'analyse et de visualisation tridimensionnelle, capable d'opérer sur un poste de travail ou dans un environnement CHP avec des dizaines de milliers de processeurs.

* page wiki de l'Alliance
* VisIt website
* VisIt gallery
* user community wiki
* tutorials avec sample datasets

=== VMD ===
VMD est un logiciel libre pour afficher, animer et analyser les grands systèmes moléculaires en mode tridimensionnel. C'est un outil de visualisation multiplateforme (macOS X, Linux, Windows) qui accepte les scripts Tcl et Python. Capable d'intégrer un grand nombre de plugiciels (plugins), l'application permet de travailler avec plusieurs formats de données moléculaires.

* page wiki de l'Alliance
* VMD User's Guide

=== VTK ===
VTK (Visualization Toolkit) est une boîte à outils logiciels 3D ouverts pour le traitement des images et la visualisation. Comprenant une bibliothèque de classes C++ et d'interfaces pour plusieurs langages interprétés dont Tcl/Tk, Java et Python, VTK a servi de base à plusieurs excellents logiciels de visualisation comme ParaView et VisIt.

* page wiki de l'Alliance
* tutorials

=== YT ===
YT est une bibliothèque Python pour l'analyse et la visualisation de données volumétriques multirésolution. Développée au départ pour les données de simulation en astrophysique, elle peut traiter toutes les données uniformes multirésolution sur les particules et dans des maillages non structurés cartésiens et curvilignes.

* page wiki de l'Alliance

== Utiliser nos grappes ==

Il existe plusieurs options de travail à distance. Règle générale, pour un rendu interactif, nous recommandons autant que possible la visualisation client-serveur avec des nœuds interactifs ou de haute priorité. Pour une visualisation non interactive, nous recommandons les tâches en lot avec des nœuds de calcul réguliers.

D'autres options moins efficaces sont la redirection X11 et VNC qui, dans le cas de certains paquets, sont les seules options d'interface utilisateur à distance.

=== Visualisation interactive client-serveur===

En mode client-serveur (avec ParaView et VisIt), les données sont traitées sur la grappe à distance avec le rendu sur CPU ou GPU, alors que vous travaillez avec une interface utilisateur client sur votre ordinateur. Pour configurer la visualisation client-serveur, voyez les pages ParaView et VisIt.

=== Fenêtres à distance avec redirection X11 ===

Règle générale, il faut éviter la redirection X11 pour le traitement graphique intensif puisqu'il y a beaucoup d'interactions et que la vitesse est moindre qu'avec VNC (ci-dessous). Par contre, dans certains cas, vous pouvez vous connecter à nos grappes via SSH par X11, comme indiqué ci-dessous. Un serveur X doit être installé sur votre ordinateur.

Connectez-vous à la grappe avec l'indicateur  -X/-Y pour la redirection X11. Vous pouvez démarrer votre application graphique dans le nœud de connexion (pour les petites visualisations).

   module load vmd
   vmd

Vous pouvez aussi demander des ressources interactives avec un nœud de calcul (visualisations d'envergure).

  salloc --time=1:00:0 --ntasks=1 --mem=3500 --account=def-someprof --x11

Une fois que la tâche est en exécution, démarrez l'application graphique à l'intérieur de la tâche.

  module load vmd
  vmd

Puisque le temps d'exécution dans les nœuds de connexion est limité, vous pourriez demander une tâche test afin de disposer de plus de temps pour explorer et visualiser vos données. Un avantage serait que vous auriez accès à 40 cœurs sur chacun des nœuds demandés. Pour utiliser une session de visualisation interactive, suivez les directives ci-dessous.

 Connectez-vous via SSH à trillium.alliancecan.ca avec l'indicateur -X/-Y pour la redirection X11.
 Demandez une tâche interactive.
   debugjob
Ceci vous connectera à un nœud, par exemple "niaXYZW".
 Démarrez l'application graphique (ici, VMD).

   module load vmd
   vmd

 Quittez la session de débogage.

=== Écrans virtuels avec Xvfb ===

Certaines applications insistent pour afficher les résultats sous forme graphique, mais il n'est pas vraiment nécessaire de les voir parce qu'ils sont enregistrés dans un fichier.
Pour travailler sans l'affichage des graphiques, la tâche peut être soumise par lots sur un CPU ou un GPU; pour ceci, exécutez l'application avec les commandes Xvfb (X virtual framebuffer) suivantes :

  xvfb-run

si vous travaillez avec un CPU

  xvfb-run vglrun -d egl

si vous travaillez avec un GPU. Dans ce cas, vous devez réserver un GPU (voir Ordonnancement Slurm des tâches exécutées avec GPU). Remarquez que si le GPU est surchargé, il pourrait ne pas être plus rapide qu'un CPU. L'étalonnage est donc important pour éviter d'utiliser des GPU qui sont plus coûteux.

=== Connexion à distance par VNC ===

Il peut souvent être utile de démarrer une interface utilisateur graphique pour certaines applications comme MATLAB, mais faire ceci par redirection X peut ralentir de beaucoup la connexion au serveur. Nous recommandons d'utiliser VNC pour démarrer et se connecter à distance. Pour plus d'information, voyez la page VNC.

= Formation =

Si vous êtes intéressé à organiser un atelier à votre établissement, écrivez à .

=== Ateliers d'une journée ou demi-journée ===
* VisIt workshop, HPCS 2016 à Edmonton, Marcelo Ponce et Alex Razoumov
* ParaView workshop, juillet 2017, Alex Razoumov
* Gnuplot, xmgrace, remote visualization tools (X-forwarding and VNC), python's matplotlib , école d'été 2016 en Ontario, Marcelo Ponce (SciNet, Université de Toronto)
*  Brief overview of ParaView & VisIt école d'été 2016 en Ontario, Marcelo Ponce (SciNet, Université de Toronto)

=== Séminaires Web et autres brèves présentations ===

La page Visualization Resources du partenaire de l'Ouest canadien présente des diapositives et des vidéos de plusieurs webinaires :

* YT series: “Using YT for analysis and visualization of volumetric data” (Part 1) et "Working with data objects in YT” (Part 2)
* “Scientific visualization with Plotly”
* “Novel Visualization Techniques from the 2017 Visualize This Challenge”
* “Data Visualization on Compute Canada’s Supercomputers”; recettes et démos client-serveur avec ParaView et scripts batch ParaView sur partitions CPU et GPU de nos grappes de calcul
* “Using ParaViewWeb for 3D Visualization and Data Analysis in a Web Browser”
* “Scripting and other advanced topics in VisIt visualization”
* “CPU-based rendering with OSPRay”
* “3D graphs with NetworkX, VTK, and ParaView”
* “Graph visualization with Gephi”

Autres présentations :

* Remote Graphics on SciNet's GPC system (Client-Server and VNC), rencontre du SciNet User Group d'octobre 2015, Ramses van Zon (SciNet, Université de Toronto)
* VisIt Basics, rencontre du SciNet User Group de février 2016, Marcelo Ponce (SciNet, Université de Toronto)
* Intro to Complex Networks Visualization, with Python, Marcelo Ponce (SciNet, Université de Toronto)
* Introduction to GUI Programming with Tkinter, septembre 2014, Erik Spence (SciNet, Université de Toronto)

== Trucs et astuces ==

Vous pouvez ajouter ici vos propres scripts et autres renseignements qui ne se trouvent pas dans la documentation signalée sur cette page. Ils pourraient s'avérer intéressants pour d'autres utilisateurs.

== Partenaires régionaux et autres références ==

* Page de l'équipe nationale pour la visualisation (comprend plusieurs exemples)
* Webinaires archivés, Université Simon-Fraser

=== SciNet, le CHP à l'Université de Toronto ===
* Visualization in Niagara
* visualization software
* VNC
* visualization nodes
* further resources and viz-tech talks
* using ParaView

==Dépannage==
Contactez le soutien technique.