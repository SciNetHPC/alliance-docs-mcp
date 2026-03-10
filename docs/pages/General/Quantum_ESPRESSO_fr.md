---
title: "Quantum ESPRESSO/fr"
url: "https://docs.alliancecan.ca/wiki/Quantum_ESPRESSO/fr"
category: "General"
last_modified: "2025-08-29T13:38:02Z"
page_id: 4870
display_title: "Quantum ESPRESSO"
language: "fr"
---

__NOTOC__
Quantum ESPRESSO est une suite de codes open source pour le calcul de structures électroniques et la modélisation de matériaux à l'échelle atomique ou microscopique. Les codes sont basés sur la théorie de la fonctionnelle de la densité, les ondes planes et les pseudopotentiels.

Les codes indépendants et interopérables sont distribués sur le modèle open source. Un ensemble de routines ou de bibliothèques permettant d'effectuer des tâches plus avancées s'ajoute au noyau de composants d'origine, en plus de quelques paquets produits par d'autres contributeurs.

= Utilisation =
Pour utiliser la suite Quantum ESPRESSO, vous devez charger un module (voir Utiliser un module).
Utilisez module avail quantumespresso ou module spider quantumespresso pour voir les versions disponibles.
Chargez le module avec, par exemple, module load quantumespresso/6.6.

Dans cet exemple, on demande 32 processus, ce qui est plus que ce qui est nécessaire dans le cas du tutoriel avec le silicium. Rappelez-vous qu'il est compliqué de déterminer le nombre de processus à demander, mais que c'est vous qui devez choisir un nombre approprié. Voir aussi  Contrôle de l'ordonnancement avec MPI.

= Problèmes connus=

== Absence de fichiers de pseudopotentiels ==
Nos grappes n'ont aucun répertoire de pseudopotentiels pour Quantum ESPRESSO. Vous devez trouver ou créer vos propres fichiers et les enregistrer vous-même.

== Erreur de paramètre avec Grimme-D3 ==

Des résultats incorrects peuvent être obtenus quand vous utilisez Grimme-3 avec le baryum (Ba).  Cette erreur est due à une valeur incorrecte pour l'un des coefficients du baryum, soit le paramètre r2r4 dans le fichier du code source dft-d3/core.f90. En effet, la valeur est de 10.1567952 et non de 0.15679528. Cette erreur est confirmée dans les versions 6.2.1 à 7.1 de Quantum ESPRESSO.
"Wrong r2r4 value for Ba in the dft-d3 code", liste d'envoi de Quantum ESPRESSO, 9 juillet 2022.