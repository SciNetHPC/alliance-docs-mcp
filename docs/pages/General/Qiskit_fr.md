---
title: "Qiskit/fr"
url: "https://docs.alliancecan.ca/wiki/Qiskit/fr"
category: "General"
last_modified: "2025-03-05T21:02:51Z"
page_id: 26345
display_title: "Qiskit"
language: "fr"
---

Qiskit est une bibliothèque de programmation quantique à code source ouvert développée en Python par IBM. Comme PennyLane et Snowflurry, elle permet de construire, simuler et exécuter des circuits quantiques.

== Installation ==
1. Chargez les dépendances de Qiskit.

2. Créez et activez un environnement virtuel Python.

3. Installez une version spécifique de Qiskit.
X.Y.Z  qiskit_aerX.Y.Z}}
où X.Y.Z représente le numéro de la version, par exemple 1.4.0. Pour installer la plus récente version disponible sur nos grappes, n'indiquez pas de version. Ici, nous n'avons importé que qiskit et qiskit_aer. Vous pouvez ajouter d'autres logiciels Qiskit en fonction de vos besoins en suivant la structure qiskit_package==X.Y.Z où qiskit_package représente le logiciel voulu, par exemple qiskit-finance. Les wheels présentement disponibles sont listés sur la page Wheels Python.

4. Validez l’installation de Qiskit.

5. Gelez l'environnement et les dépendances.

==Exécuter Qiskit sur une grappe==

Vous pouvez ensuite soumettre votre tâche à l'ordonnanceur.
== Utiliser Qiskit avec MonarQ (à venir)==

== Exemple d'utilisation : États de Bell ==
Nous allons créer le premier état de Bell sur Narval en simulation. Il faut d'abord importer les modules nécessaires.
    from qiskit_aer import AerSimulator
    from qiskit import QuantumCircuit
    from qiskit.visualization import plot_histogram

Ensuite, nous définissons le circuit. Nous appliquons une porte Hadamard afin de créer un état de superposition sur le premier qubit et nous appliquons ensuite une porte CNOT pour intriquer le premier et le deuxième qubit.
    circuit = QuantumCircuit(2)
    circuit.h(0)
    circuit.cx(0,1)
    circuit.measure_all()

Nous précisons le simulateur que nous voulons utiliser. AerSimulator étant le simulateur par défaut. Nous obtenons le dénombrement des états finaux des qubits après 1000 mesures.
    simulator = AerSimulator()
    result = simulator.run(circuit, shots=1000).result()
    counts = result.get_counts()
    print(counts)
    {'00': 489, '11': 535}
Nous affichons un histogramme des résultats avec la commande
    plot_histogram(counts)