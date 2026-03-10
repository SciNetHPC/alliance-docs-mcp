---
title: "Mii/fr"
url: "https://docs.alliancecan.ca/wiki/Mii/fr"
category: "General"
last_modified: "2021-10-18T13:53:38Z"
page_id: 17786
display_title: "Mii"
language: "fr"
---

Un moteur de recherche intelligent pour les modules.

Mii recherche et charge des modules sur demande dans une installation existante.

Une fois Mii chargé, les modules sont chargés automatiquement dans le cas de commandes connues.
Pour les commandes inconnues, Mii demande interactivement quels modules doivent être chargés.

Caractéristiques :
* prend en charge les installations de Lmod et Environment Modules
* intégration des interpréteurs bash et zsh
* liste des modules / renseignements particuliers (via mii list, mii show)
* recherche de commande exactes (via mii exact)
* recherche de commandes similaires (via mii search)
* format d'exportation JSON optionnel

= Activation =
Chargez et activez avec la commande

Une fois chargé, Mii émet des suggestions. Par exemple, lorsqu'une commande est introuvable, Mii suggère

= Commandes connues =
Une commande ou un binaire qui est connu sera chargé automatiquement.

= Commandes non connues =
Lorsqu'une commande ou un binaire n'est pas connu, Mii suggère des candidats potentiels en se basant sur leur pertinence.

Dans cet exemple, nous avons entré le chiffre 1 à l'invite Make a selection et la commande s'est exécutée.

= Recherche =
Vous pouvez chercher des binaires pour obtenir la liste des modules qui les offrent. Les résultats sont présentés dans  l'ordre de leur pertinence.

= Désactiver Mii =
Lancez la commande

== Réactiver Mii ==
Lancez la commande