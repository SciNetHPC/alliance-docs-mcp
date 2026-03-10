---
title: "RDKit/fr"
url: "https://docs.alliancecan.ca/wiki/RDKit/fr"
category: "General"
last_modified: "2025-01-27T21:14:37Z"
page_id: 27755
display_title: "RDKit"
language: "fr"
---

RDKit est un ensemble d'applications pour la chimie computationnelle et l'apprentissage machine qui sont écrites en C++ et en Python.

__FORCETOC__

= Versions disponibles =
Les bibliothèques C++ et les interfaces Python sont disponibles via un module.

Pour connaître les versions disponibles, utilisez

Pour l'information sur une version particulière, utilisez

où X.Y.Z est la version recherchée, par exemple 2024.03.5.

= Interfaces (bindings) Python  =
Le module contient des interfaces pour plusieurs versions de Python. Pour connaître les versions disponibles, utilisez

où X.Y.Z est la version que vous voulez.

== Dépendance ==
Quand un autre wheel dépend de rdkit, la dépendance doit être satisfaite.

1. Désactivez tout environnement virtuel Python.

Remarque : Si un environnement virtuel est actif, il est important de le désactiver avant de charger le module. Une fois le module chargé, activez à nouveau votre environnement virtuel.

2. Chargez le module.

3. Vérifiez qu'il est visible par pip avec
 grep rdkit
|result=
rdkit            2024.3.5
}}
et que le module Python que vous avez chargé lui a accès, avec

Si aucune erreur ne survient, le problème devrait être réglé.

Si aucune erreur ne survient, tout va bien.

4. Créez un environnement virtuel et installez les paquets.

= Dépannage =

== Message ModuleNotFoundError: No module named 'rdkit' ==
Ce message peut survenir si rdkit n’est pas disponible.

ModuleNotFoundError: No module named 'rdkit'

Solutions possibles
* Vérifiez quelles versions de Python sont compatibles avec le module RDKit chargé avec module spider rdkit/X.Y.Z. Une fois qu'un module Python compatible est chargé, vérifiez que python -c 'import rdkit' fonctionne.
* Chargez le module avant d'activer votre environnement virtuel; voir  Dépendance ci-dessus.

Voir aussi ModuleNotFoundError: No module named 'X'.