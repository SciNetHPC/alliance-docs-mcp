---
title: "Dedalus/fr"
url: "https://docs.alliancecan.ca/wiki/Dedalus/fr"
category: "General"
last_modified: "2024-09-30T17:26:42Z"
page_id: 25489
display_title: "Dedalus"
language: "fr"
---

__FORCETOC__

Dedalus est un environnement de développement flexible pour résoudre des équations aux dérivées partielles à l'aide de méthodes spectrales modernes.

= Versions disponibles =
Sur nos grappes, les versions de Dedalus sont des wheels Python. Pour connaître les versions disponibles, exécutez avail_wheels.

= Installation dans un environnement virtuel Python =
1. Chargez les modules requis pour exécuter Dedalus.

2. Créez et activez un environnement virtuel Python.

3. Installez une version de Dedalus et ses dépendances Python.
X.Y.Z
}}
où X.Y.Z est la version choisie (par exemple 3.0.2).
Si aucun numéro n'est indiqué, la plus récente version sera installée.

4. Validez.

5. Gelez l'environnement et les dépendances requises.

6. Supprimez l'environnement virtuel local.

= Exécution =
Dedalus peut être exécuté en mode distribué sur plusieurs nœuds ou cœurs.
Pour plus d'information, voir
* Tâche MPI
* Contrôle de l'ordonnancement avec MPI

1. Préparez le script.

2. Soumettez la tâche à l'ordonnanceur.

Avant de soumettre la tâche, il est important de tester le script pour des erreurs possibles. Faites un test rapide avec une tâche interactive.