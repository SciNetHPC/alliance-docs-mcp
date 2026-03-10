---
title: "CirQ/fr"
url: "https://docs.alliancecan.ca/wiki/CirQ/fr"
category: "General"
last_modified: "2024-10-02T17:50:02Z"
page_id: 26313
display_title: "CirQ"
language: "fr"
---

CirQ est une bibliothèque d'informatique quantique à code source ouvert développée en Python par Google, qui permet de construire, optimiser, simuler et exécuter des circuits quantiques. Plus particulièrement, CirQ permet de simuler des circuits sur des configurations spécifiques de qubits, ce qui peut optimiser un circuit pour une certaine architecture de qubits. L'information sur les fonctionnalités de la bibliothèque est disponible dans la documentation et sur le GitHub de CirQ. Tout comme Snowflurry, CirQ peut être utilisée pour exécuter des circuits quantiques sur l'ordinateur quantique MonarQ.

== Installation ==
Le simulateur d'ordinateur quantique CirQ est disponible sur toutes nos grappes. Le langage de programmation Python doit être chargé avant d'y avoir accès. Il est préférable de travailler dans un environnement virtuel Python.
1.4.1
|python -c "import cirq"
|pip freeze > cirq-1.4.1-reqs.txt
}}
La dernière commande crée un fichier nommé cirq-1.4.1-reqs.txt, que vous pouvez réutiliser dans un script de tâche, tel que décrit ci-dessous.
==Exécution sur une grappe==

Vous pouvez ensuite soumettre votre tâche à l'ordonnanceur.

== Exemple d'utilisation : États de Bell ==
Les états de Bell sont les états les plus simples qui permettent d'expliquer à la fois la superposition et l'intrication sur des qubits.
La bibliothèque CirQ permet de construire un état de Bell comme ceci :

Ce code construit et affiche un circuit qui prépare un état de Bell. La porte H (porte de Hadamard) crée une superposition égale de |0⟩ et |1⟩ sur le premier qubit tandis que la porte CNOT (porte X contrôlée) crée une intrication entre les deux qubits. Cet état de Bell est donc une superposition égale des états |00⟩ et |11⟩. La simulation de ce circuit à l'aide de CirQ permet de visualiser les résultats. Dans ce diagramme, le nombre entier 3 représente l'état |11⟩ puisque 3 s'écrit 11 en binaire.