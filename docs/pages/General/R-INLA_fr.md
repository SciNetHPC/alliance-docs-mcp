---
title: "R-INLA/fr"
url: "https://docs.alliancecan.ca/wiki/R-INLA/fr"
category: "General"
last_modified: "2025-07-17T20:15:45Z"
page_id: 28935
display_title: "R-INLA"
language: "fr"
---

R-INLA est un paquet en langage Rr qui utilise une méthodologie d'approximation de l’inférence bayésienne.

== Installation ==

L'installation est un peu plus compliquée que celle pour les autres paquets R parce que d'autres exécutables précompilés doivent être téléchargées pour assurer la compatibilité avec nos environnements logiciels standards.

Les scripts ci-dessous ont été testés avec les versions qu'ils utilisent.
Puisque R installe toujours la dernière versions des paquets, les versions des modules devront être ajustées au besoin.

Commentaires dans le script

* (1) Pour charger les modules requis. Il faut aussi charger les mêmes modules dans le script pour la tâche.
* (2) Pour installer le paquet R-INLA et ses dépendances.
* (3) Pour installer les exécutables précompilés requis par R-INLA.
* (4) Pour corriger les exécutables précompilés pour qu'ils soient compatibles avec nos environnements logiciels standards.