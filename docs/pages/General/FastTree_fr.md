---
title: "FastTree/fr"
url: "https://docs.alliancecan.ca/wiki/FastTree/fr"
category: "General"
last_modified: "2024-12-17T21:21:36Z"
page_id: 27268
display_title: "FastTree"
language: "fr"
---

FastTree déduit des arbres phylogénétiques de vraisemblance maximale à partir d'alignements de séquences de nucléotides ou de protéines. FastTree peut gérer des alignements comportant jusqu'à un million de séquences dans un laps de temps et avec une consommation de mémoire raisonnables.

= Modules d'environnement =

Nous offrons des modules pour des calculs en simple précision et en double précision. Les calculs en simple précision sont plus rapides, mais ceux en double précision sont plus précis. La double précision est recommandée lorsque vous utilisez une matrice de transition fortement biaisée ou si vous souhaitez résoudre avec précision des branches très courtes.

Pour connaître la disponibilité des modules :

 module spider fasttree

Pour charger un module simple précision :

 module load fasttree/2.1.11

Pour charger un module double précision :

 module load fasttree-double/2.1.11

= Dépannage =

* Message d'erreur WARNING! This alignment consists of closely-related and very long sequences : Cela conduit généralement à des branches très courtes, parfois même de longueur négative.

= Références =

* [FastTree Page web pour FastTree]