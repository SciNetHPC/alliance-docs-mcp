---
title: "MPI4py/fr"
url: "https://docs.alliancecan.ca/wiki/MPI4py/fr"
category: "General"
last_modified: "2024-10-17T19:53:00Z"
page_id: 26655
display_title: "MPI4py"
language: "fr"
---

MPI for Python fournit une interface Python pour la norme de communication MPI (Message Passing Interface), permettant aux applications Python d'exploiter plusieurs processeurs sur des postes de travail, des grappes et des supercalculateurs.

__FORCETOC__

= Versions disponibles =

Dans notre environnement, mpi4py est un module et non un paquet précompilé (wheel) comme la plupart des paquets Python. Pour trouver les versions disponibles, utilisez

Pour plus d’information sur une version particulière, utilisez

où X.Y.Z est le numéro de la version, par exemple 4.0.0.

= Exemple avec Hello World =
1.Démarrez une courte tâche interactive.
 --ntasks5}}

2. Chargez le module.

3. Faites un test de type Hello World.

Dans cet exemple, deux nœuds (node1 et node3) ont été alloués et les tâches ont été distribuées sur les ressources disponibles.

= mpi4py comme dépendance d'un autre paquet =
Quand un autre paquet dépend de mpi4py,

1. Désactivez tout environnement virtuel Python.

Remarque : Si un environnement virtuel est actif, il est important de le désactiver avant de charger le module. Une fois le module chargé, activez à nouveau votre environnement virtuel.

2. Chargez le module.

3.  Vérifiez que le module est visible par pip
 grep mpi4py
|result=
mpi4py            4.0.0
}}
et que le module Python que vous avez chargé lui a accès.

Si aucune erreur ne survient, tout va bien.

4. Créer un environnement virtuel et installez les paquets.

= Exécuter des tâches =
Les tâches MPI peuvent être distribuées sur plusieurs cœurs ou plusieurs nœuds. Pour plus d’information, voir
* MPI job
* Advanced MPI scheduling

== Sur CPU ==
1. Préparez le code Python ci-dessous pour distribuer un tableau NumPy.

Cet exemple est basé sur le tutopriel mpi4py.

2. Préparez le script de tâche

3. Testez votre script.

Avant de soumettre la tâche, il est important de tester le script pour des erreurs possibles. Faites un test rapide avec une tâche interactive.

4. Soumettez votre tâche.

== GPU ==
1. Sur un nœud de connexion, téléchargez l’exemple tiré des démos.

2. Préparez votre script de soumission.

3. Testez votre script.

Avant de soumettre la tâche, il est important de tester le script pour des erreurs possibles. Faites un test rapide avec une tâche interactive.

4. Soumettez votre tâche.

= Dépannage =

== Message ModuleNotFoundError: No module named 'mpi4py' ==
Ce message peut survenir à l’importation quand mpi4py n’est pas accessible.

ModuleNotFoundError: No module named 'mpi4py'

Solutions suggérées :
* avec module spider mpi4py/X.Y.Z, vérifiez quelles versions de Python sont compatibles avec le module mpi4py que vous avez chargé. Quand une version compatible est chargée, vérifiez si python -c 'import mpi4py' fonctionne;
* chargez le module avant d'activer votre environnement virtuel (voir mpi4py comme dépendance d'un autre paquet ci=dessus).

Voir aussi Message ModuleNotFoundError: No module named 'X'.