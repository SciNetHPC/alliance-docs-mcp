---
title: "OpenMM/fr"
url: "https://docs.alliancecan.ca/wiki/OpenMM/fr"
category: "General"
last_modified: "2024-10-18T21:22:27Z"
page_id: 19329
display_title: "OpenMM"
language: "fr"
---

=Introduction=
OpenMMSite Web OpenMM : https://openmm.org/ est une boîte d'outils conçue pour la simulation moléculaire. On peut l'utiliser seule comme application pour effectuer des simulations ou comme bibliothèque que vous appelez à partir de votre code. OpenMM est un paquet unique de par sa très grande flexibilité des champs de force personnalisés et des algorithmes de résolution (ou d’intégration), son ouverture et son excellente performance, en particulier avec les GPU récents.

=Simulation avec topologies AMBER et fichiers de redémarrage=

== Préparer l'environnement virtuel Python ==

Cet exemple utilise le module openmm/7.7.0.

1. Créez et activez l'environnement virtuel Python.

2. Installez les modules Python ParmEd et netCDF4.
3.4.3 netCDF4
}}

== Soumettre une tâche ==
Le script suivant est pour une tâche de simulation qui utilise un GPU.

Ici, openmm_input.py est un script Python qui charge des fichiers Amber, crée le système de simulation OpenMM, configure l'intégration et exécute les dynamiques ( voir cet exemple).

==Performance et étalonnage benchmarking==

Le guide Molecular Dynamics Performance Guide a été créé par une équipe d'ACENET. Le guide décrit les conditions optimales pour exécuter aussi des tâches sur nos grappes avec AMBER, GROMACS et NAMD.

Sur la plateforme CUDA, OpenMM n'a besoin que d'un CPU par GPU parce que les CPU ne sont pas utilisés pour les calculs. OpenMM peut utiliser plusieurs GPU dans un nœud, mais il est plus efficace de faire les simulations avec un seul GPU. Comme le démontrent les essais sur Narval et ceux sur Cedar, la vitesse de simulation avec plusieurs GPU est légèrement augmentée sur les nœuds avec NvLink où les GPU sont directement connectés. Sans NvLink, la vitesse de simulation augmente très peu avec des GPU P100 (essais sur Cedar).