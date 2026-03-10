---
title: "Make/fr"
url: "https://docs.alliancecan.ca/wiki/Make/fr"
category: "General"
last_modified: "2024-07-22T22:45:01Z"
page_id: 2036
display_title: "Make"
language: "fr"
---

== Description ==
make est un logiciel qui construit automatiquement des bibliothèques ou des fichiers souvent exécutables, à partir d'éléments de base tels que du code source.

La commande make interprète et exécute les instructions du fichier makefile.  À la différence d'un simple script, make exécute les commandes seulement si elles sont nécessaires.  Le but est d'arriver à un résultat (logiciel compilé ou installé, documentation créée, etc.) sans nécessairement refaire toutes les étapes.

Dans le fichier makefile se trouvent, entre autres, des informations sur les dépendances.
Par exemple, puisque l'exécutable du programme dépend des fichiers source, si certains de ces fichiers ont changé, un réassemblage du programme est nécessaire.
De la même manière, les fichiers objets dépendant de leurs fichiers sources associés, si un fichier source a été modifié, ce dernier doit être recompilé pour recréer le nouveau fichier objet.
Toutes ces dépendances doivent être incluses dans le fichier makefile.  Ainsi, il n'est plus nécessaire de recompiler tous les fichiers sources à chaque modification; la commande make  s'occupe de recompiler et réassembler uniquement ce qui est nécessaire.

== Exemples d'utilisation ==
Le principal argument de la commande make est généralement la cible.  Il s'agit de la composante que make doit construire.
Les cibles disponibles dépendent du contenu du makefile, mais certaines cibles sont très communes, par exemple all, test, check, clean et install,
qui sont souvent employées. Dans l'exemple suivant de make, aucune cible n'est spécifiée.

Le comportement typique est de tout construire, soit l'équivalent de

Les cibles test ou check sont généralement utilisées pour exécuter des tests afin de valider que l'application ou la bibliothèque compilée fonctionne correctement.  De façon générale, ces cibles sont dépendantes de la cible all.  Vous pouvez ainsi vérifier la compilation via la commande

ou

La cible clean efface tous les fichiers binaires compilés précédemment afin de reprendre la compilation de zéro.  Il existe parfois aussi la cible distclean qui efface non seulement les fichiers créés par make, mais aussi les fichiers créés lors de l'opération de configuration par configure ou cmake.  Ainsi, pour nettoyer le répertoire de compilation, vous pouvez généralement exécuter

et parfois

La cible install procède normalement à l'installation de l'application ou de la bibliothèque compilée.  L'emplacement de l'installation dépend du makefile, mais peut souvent se modifier via un paramètre additionnel prefix ainsi  :
$HOME/PROGRAM}}

Ces cibles  all, test, check, clean, distclean et install ne sont cependant que des conventions et l'auteur d'un makefile pourrait très bien choisir une autre convention.  Pour davantage d'information sur les cibles typiques, notamment supportées par toutes les applications GNU, consultez cette page.  Les options pour configurer les répertoires d'installation et autres sont quant à elles listées ici.

== Exemple de Makefile ==
L'exemple suivant, d'utilisation générale, inclut beaucoup d'explications et de commentaires.  Pour un guide approfondi sur la création de fichiers makefile, visitez le site Web GNU Make.