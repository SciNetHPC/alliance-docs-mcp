---
title: "Dask/fr"
url: "https://docs.alliancecan.ca/wiki/Dask/fr"
category: "General"
last_modified: "2026-01-28T14:35:21Z"
page_id: 27545
display_title: "Dask"
language: "fr"
---

Dask est une bibliothèque polyvalente pour Python. Elle fournit des tableaux NumPy et des DataFrame Pandas permettant le calcul distribué en Python pur avec accès à la pile PyData.

=Installer le wheel =

La meilleure option est d'installer avec Python wheels comme suit :

::1. Chargez un module  Python avec module load python.
::2. Créez et démarrez un  environnement virtuel.
::3. Dans l'environnement virtuel, utilisez pip install pour installer dask et en option dask-distributed.

:

=Soumettre une tâche=

== Nœud simple ==
L’exemple suivant démarre une grappe Dask avec un nœud simple de 6 CPU et calcule la moyenne d’une colonne pour l'ensemble des données.

Ce script démarre une grappe Dask ayant autant de processus de travail que de coeurs dans la tâche. Chacun des processus crée au moins un fil d’exécution. Pour déterminer le nombre de processus et de fils, consultez la documentation officielle de Dask. Ici, le dataframe Pandas est divisé en 6 parts et chaque processus en traitera une avec un CPU

== Plusieurs nœuds ==
Dans le prochain exemple, nous reprenons l'exemple du nœud simple, mais cette fois avec une grappe Dask de deux nœuds comportant 6 CPU chacun. Nous créons aussi deux processus par nœud comportant trois cœurs chacun.

où le script config_virtualenv.sh est

et le script launch_dask_workers.sh est

Enfin, le script test_dask.py est