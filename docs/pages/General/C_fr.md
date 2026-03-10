---
title: "C/fr"
url: "https://docs.alliancecan.ca/wiki/C/fr"
category: "General"
last_modified: "2017-05-05T20:14:15Z"
page_id: 3147
display_title: "C"
language: "fr"
---

=C=

C est un langage de programmation impératif et généraliste de haut niveau créé entre 1969 et 1973 chez Bell Labs par Dennis Ritchie. Il existe aujourd'hui des normes ISO qui ont été établies en 1989-1990 (C89 ou C90), 1999 (C99) et 2011 (C11). Pour en apprendre plus sur le langage et sur l'impact des normes ISO, voyez les liens suivants  :

* C, historique, C90.
* C99,  inclut les fonctions du langage et de la bibliothèque standard; int n'est plus le type par défaut.
*C11,  mise à jour majeure, ajout du modèle de mémoire et des fonctionnalités de concurrence (multifil, atomics, compare-and-swap).

Ces liens peuvent conduire à des pages qui contiennent des erreurs. Le document officiel peut être commandé auprès du Conseil canadien des normes.

=Meilleurs modèles de mémoire et de concurrence=

Ces modèles sont apparus dans la norme ISO de 2011; il n'y avait au préalable aucune gestion des accès concurrents à la mémoire en lecture et en écriture, par exemple en ce qui a trait aux comportements ambigus qui ont ou non été documentés par les fournisseurs de compilateurs. Nous recommandons de compiler le code C comportant de la concurrence en C11 ou plus.

= Pièges =
== Mot-clé volatile ==

Le modificateur volatile a un sens très particulier en C et C++ comme vous le constaterez en lisant cette page. L'emploi de ce modificateur est toutefois rare et se limite à certains types de code de bas niveau.

volatile est employé incorrectement en C car on le confond avec le  volatile Java qui n'a pas du tout le même sens. Le  mot-clé Java volatile correspond en C à atomic_*, où l'astérisque représente un nom de type fondamental tel que int.

== Compilateurs ==
===GCC===
L'option -O3 comprend des améliorations possiblement dangereuses, par exemple pour les fonctions de crénelage (aliasing). En cas de doute, utilisez plutôt l'option -O2. Si vous avez le temps, lisez la page man (par exemple man gcc) et cherchez -O3; vous pouvez ainsi désactiver les paramètres qui ne sont pas sûrs.

===Intel===
Les compilateurs C et C++ d'Intel risquent de causer des difficultés dans le cas d'opérations avec virgule flottante.  Prenez connaissance des pages man Intel (par exemple man icc) et utilisez les options -fp-model precise ou -fp-model source pour respecter les normes ANSI, ISO et IEEE. Pour des détails, consultez ce document.