---
title: "PennyLane/fr"
url: "https://docs.alliancecan.ca/wiki/PennyLane/fr"
category: "General"
last_modified: "2025-07-01T12:47:23Z"
page_id: 26335
display_title: "PennyLane"
language: "fr"
---

PennyLane est une plateforme logicielle à code source ouvert pour le calcul quantique différentiable dont la première version a été publiée sur Github en 2018. Développée à Toronto par Xanadu, PennyLane permet de concevoir des circuits quantiques et de les exécuter sur divers simulateurs et matériels quantiques. La plateforme est conçue pour faciliter la simulation, l'optimisation et l’apprentissage d’algorithmes quantiques hybrides qui combinent des traitements classiques et quantiques.

== Fonctionnalités ==
PennyLane offre plusieurs fonctionnalités pour faciliter la recherche et le développement dans le domaine de l'informatique quantique différentiable.

=== Interface quantique unifiée ===
PennyLane fournit une interface unifiée qui permet de concevoir des circuits quantiques et de les exécuter sur différents simulateurs et matériels quantiques. La plateforme prend en charge plusieurs simulateurs quantiques populaires, tels que Qiskit, CirQ, Strawberry Field ou encore QuTip. PennyLane prend également en charge plusieurs matériels quantiques, notamment les dispositifs quantiques de Xanadu, IBM, Rigetti et IonQ.

Calcul Québec a développé le plugiciel PennyLane-CalculQuebec qui utilise l’interface PennyLane pour concevoir et exécuter des circuits quantiques sur MonarQ.

=== Intégration avec des bibliothèques d'apprentissage automatique ===
PennyLane s'intègre de manière transparente avec des librairies d'apprentissage automatique populaires telles que TensorFlow et PyTorch, et vous permet d'utiliser les outils d'apprentissage automatique pour construire des modèles d'apprentissage automatique quantiques hybrides et optimiser les circuits quantiques.

=== Optimisation de circuits quantiques ===
En utilisant des techniques d'optimisation différentiables et en combinant les méthodes de différenciation classiques et quantiques, PennyLane optimise les paramètres des circuits quantiques afin de résoudre des problèmes variés.

=== Outils de visualisation ===
PennyLane fournit des outils de visualisation pour faciliter la compréhension du fonctionnement des circuits quantiques.

=== Communauté et développement ===
PennyLane est un projet à code source ouvert avec une communauté active de développeurs et d'utilisateurs. Le projet est constamment mis à jour avec de nouvelles fonctionnalités et améliorations, et tous peuvent contribuer au développement de la plateforme.

== Utiliser PennyLane avec MonarQ ==
MonarQ est conçu pour être programmé avec Snowflurry, une bibliothèque logicielle programmée en Julia et développée par Anyon Systems. Par contre, grâce au plugiciel PennyLane-CalculQuebec, les circuits PennyLane peuvent être créés en utilisant Snowflurry en arrière-plan. Cela permet d’exécuter des circuits sur MonarQ tout en bénéficiant des fonctionnalités et de l'environnement de développement offerts par PennyLane. Voir la documentation PennyLane-CalculQuebec pour les guides d’installation et d’usage.

Un transpileur quantique est également disponible à partir de PennyLane afin d'optimiser ses circuits pour MonarQ.

== Création de l'environnement virtuel  ==
Créons un environnement virtuel Python pour utiliser PennyLane.
X.Y.Z
|python -c "import pennylane"
}}
où X.Y.Z est la version désirée.

Vous pouvez également inscrire les trois dernières commandes ci-dessus dans un fichier pennylane-reqs.txt et appeler le fichier à l'intérieur d'une session avec  les commandes:

==Exécuter PennyLane sur une grappe==

Vous pouvez ensuite soumettre votre tâche à l'ordonnanceur.

== Exemple d’utilisation : États de Bell ==
Commençons par créer l'environnement virtuel, tel que décrit ci-dessus.

Nous allons ensuite générer le premier état de Bell en utilisant PennyLane.
    import pennylane as qml

   # Définir le circuit quantique pour générer le premier état de Bell
    def bell_circuit():
     qml.Hadamard(wires=0)
     qml.CNOT(wires=[0, 1])

   # Définir le simulateur de circuit quantique
    dev = qml.device('default.qubit', wires=2)

   # Définir le circuit quantique comme fonction QNode
    @qml.qnode(dev)
    def generate_bell_state():
     bell_circuit()
     return qml.state()

   # Générer et afficher le premier état de Bell
    bell_state_0 = generate_bell_state()
    print("Premier état de Bell :", bell_state_0)
    Premier état de Bell :[0.70710678+0.j 0.        +0.j 0.        +0.j 0.70710678+0.j]

== Références ==
* Site officiel de PennyLane
* Documentation de PennyLane sur GitHub
* PennyLane-CalculQuebec