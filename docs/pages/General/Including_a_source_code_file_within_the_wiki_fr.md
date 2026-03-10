---
title: "Including a source code file within the wiki/fr"
url: "https://docs.alliancecan.ca/wiki/Including_a_source_code_file_within_the_wiki/fr"
category: "General"
last_modified: "2018-02-14T15:01:52Z"
page_id: 4975
display_title: "Inclure un fichier de code source dans le wiki"
language: "fr"
---

__NOTOC__

Tel que mentionné à la page Inclure du code source dans le wiki, les balises   servent à inclure du code.
Si vous désirez que le code soit à part du texte, utilisez le gabarit . Ce gabarit prend le nom (paramètre name), la langue (paramètre lang) et le contenu (paramètre contents) du fichier comme arguments. Ce gabarit utilise par défaut le langage bash.

Par exemple,

donne le résultat suivant

== Caractères spéciaux ː Trait vertical  et signe d'égalité ==
Les scripts bash contiennent souvent des caractères qui ont aussi une signification pour l'analyseur syntaxique (parser) MediaWiki.
* Si le code source contient un trait vertical (le caractère |), remplacez-le par .
* Dans certains cas vous devez remplacer le signe d'égalité (le caractère =) par .

== Affichage des numéros de lignes ==
Pour afficher les numéros de lignes, ajoutez l’option lines=yes, par exemple

donne le résultat suivant