---
title: "Including a command within the wiki/fr"
url: "https://docs.alliancecan.ca/wiki/Including_a_command_within_the_wiki/fr"
category: "General"
last_modified: "2025-05-30T21:39:07Z"
page_id: 150
display_title: "Inclure une commande dans le wiki"
language: "fr"
---

Pour inclure une commande dans le wiki, il faut utiliser le gabarit . Ce gabarit détecte la syntaxe bash. Par exemple, le code

produit le résultat :

== Caractères spéciaux "" et "" ==
Puisque  est un gabarit, les signes "=" et "|" sont interprétés par le wiki.

Pour inclure le signe "égal" utilisez .
Par exemple, le code

$HOME && make && make install}}

produit le résultat :
$HOME && make && make install}}
Pour le trait vertical, utilisez .

== Inclure un ensemble de commandes ==
Vous pouvez utiliser le gabarit  pour inclure un ensemble de commandes. Inscrivez alors chaque commande sur une seule ligne, précédée du caractère |. Par exemple,

produit le résultat :

== Modifier l'invite de commande ==
Si vous voulez modifier l'invite de commande (prompt), vous pouvez le faire en ajoutant un paramètre prompt. Par exemple :

produit le résultat :

De même,

produit le résultat :

== Afficher le résultat d'une commande ==
Vous pouvez afficher le résultat d'une commande (et d'une seule) en ajoutant l'option resultat. Par exemple,

produit le résultat :