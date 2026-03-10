---
title: "Faiss/fr"
url: "https://docs.alliancecan.ca/wiki/Faiss/fr"
category: "General"
last_modified: "2024-05-02T20:14:01Z"
page_id: 25437
display_title: "Faiss"
language: "fr"
---

Faiss est une bibliothèque efficace pour la recherche de similarités et le regroupement de vecteurs denses. Elle contient des algorithmes qui recherchent dans des ensembles de vecteurs de n'importe quelle taille, même ceux qui sont trop grands pour la mémoire vive. Elle contient également du code pour l'évaluation et le réglage des paramètres. Faiss est écrite en C++ avec des scripts enveloppants complets pour Python (versions 2 et 3). Certains des algorithmes les plus utiles sont implémentés sur GPU. Faiss est développée principalement par Meta AI Research avec l'aide de contributeurs externes.

__TOC__

== Liaisons Python ==
Le module contient des liaisons pour plusieurs versions de Python.
Pour connaître les versions disponibles, lancez

ou allez directement à faiss-cpu avec

où X.Y.Z désigne la version voulue.

=== Utilisation ===
1. Chargez les modules requis.

où X.Y.Z désigne la version choisie.

2. Importez Faiss.

Si la commande n'affiche rien, l'importation a réussi.

==== Paquets Python disponibles  ====
Certains paquets Python dépendent des liaisons faiss-cpu ou faiss-gpu pour être installés.
Le module faiss fournit
* faiss
* faiss-gpu
* faiss-cpu

 fgrep faiss
|result=
faiss-gpu                          1.7.4
faiss-cpu                          1.7.4
faiss                              1.7.4
}}

Avec le module faiss chargé, les dépendances des extensions ci-dessus seront satisfaites.