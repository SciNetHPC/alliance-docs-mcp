---
title: "Biomolecular simulation/fr"
url: "https://docs.alliancecan.ca/wiki/Biomolecular_simulation/fr"
category: "General"
last_modified: "2025-02-10T22:26:56Z"
page_id: 5663
display_title: "Simulation biomoléculaire"
language: "fr"
---

== Généralités ==

La simulation biomoléculaireRon O. Dror, Robert M. Dirks, J.P. Grossman, Huafeng Xu, and David E. Shaw. "Biomolecular Simulation: A Computational Microscope for Molecular Biology." Annual Review of Biophysics,  41:429-452, 2012. https://doi.org/10.1146/annurev-biophys-042910-155245  est l'application de la simulation en dynamique moléculaire à la recherche biochimique. Parmi les processus qui peuvent être modélisés, on trouve le repliement des protéines, les liaisons médicamenteuses, le transport membranaire et les modifications conformationnelles essentielles à la fonction protéinique.

La simulation biomoléculaire est considérée comme étant un sous-domaine de la chimie computationnelle; son champ d'action est cependant assez spécialisé pour que nous disposions d'une équipe d'experts dédiés. Consultez aussi la liste des ressources disponibles en chimie computationnelle.

== Logiciels ==

Les paquets logiciels suivants sont disponibles via avec nos ressources.

* AMBER
* GROMACS
* NAMD
* DL_POLY
* HOOMD-blue
* LAMMPS
* OpenKIM (Knowledgebase of Interatomic Models)
* OpenMM
* PLUMED, bibliothèque de code pour le développement relatif au calcul de l'énergie libre dans les simulations en dynamique moléculaire (voir aussi GROMACS)
* Rosetta
* DSSP
* VMD

=== Wheels Python ===

Calcul Canada offre des wheels Python qui peuvent être installés dans des environnements virtuels Python; ces wheels sont très utiles en simulation biomoléculaire et dynamique moléculaire.

La liste suivante contient une sélection des wheels les plus utiles, mais ne doit pas être considérée comme complète :

* ACPYPE: AnteChamber PYthon Parser interfacE outil servant à générer des topologies de composés chimiques.
* MDAnalysis, bibliothèque Python orientée objet pour l'analyse de trajectoires dans les simulations de dynamique moléculaires dans plusieurs formats.
* MDTraj, qui peut aussi lire, écrire et analyser des trajectoires par quelques lignes de code Python, dans une grande variété de formats.
* Biopython, ensemble d'outils gratuits pour les calculs biologiques.
* foyer, paquet pour déterminer le type des atomes et appliquer et disséminer les champs de force.
* mBuild, langage hiérarchique pour construire des molécules basées sur des composantes.
* mdsynthesis, ensemble d’outils de manipulation et d'analyse des données de dynamique moléculaire.
* nglview, collection d'outils en ligne pour la visualisation en moléculaire.
* ParmEd, outil général pour l'analyse des systèmes biomoléculaires avec des paquets de simulation populaires.
* PyRETIS, bibliothèque Python pour les simulations d'événements rares, avec une emphase sur l'échantillonnage d'interfaces de transition et d'interfaces de transition avec échange de réplication.

Voyez liste des wheels disponibles et la commande commande avail_wheels pour savoir ce qui est disponible.

Si vous avez besoin d'autres paquets Python ou des versions plus récentes, contactez le soutien technique.

== Formation ==

Des ateliers de formation sont donnés par notre équipe nationale pour la modélisation et la simulation moléculaires; les dates seront annoncées à l'avance.

Vous pouvez aussi prendre connaissance du matériel de formation par les liens suivants :

# Practical considerations for Molecular Dynamics
#       Visualizing Structures with VMD
#         Running Molecular Dynamics with Amber on our clusters
#         Analyzing Molecular Dynamics Data with PYTRAJ

==Performance et étalonnage benchmarking==

Le guide Molecular Dynamics Performance Guide a été créé par une équipe d'ACENET. Le guide décrit les conditions optimales pour exécuter aussi des tâches sur nos grappes avec AMBER, GROMACS, NAMD et OpenMM.

== Références ==
Biomolecular Simulation: A Computational Microscope for Molecular Biology