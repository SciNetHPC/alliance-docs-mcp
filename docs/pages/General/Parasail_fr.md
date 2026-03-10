---
title: "Parasail/fr"
url: "https://docs.alliancecan.ca/wiki/Parasail/fr"
category: "General"
last_modified: "2024-06-28T14:11:11Z"
page_id: 25963
display_title: "Parasail"
language: "fr"
---

parasail est une bibliothèque  SIMD C (C99) qui contient des implémentations d'algorithmes d'alignement de séquences par paires Smith-Waterman (alignement local), Needleman-Wunsch (alignement global) et autres alignements semi-globaux.

= Utilisation =

Pour connaître la version disponible, utilisez

Chargez la bibliothèque avec

== Avec le binaire parasail_aligner  ==
Il est important de définir le nombre de fils selon le nombre de cœurs alloués à votre tâche, par exemple

parasail_aligner -t ${SLURM_CPUS_PER_TASK:-1} ...}}

== Extension Python ==
Le module contient des liaisons pour plusieurs versions de Python.
Pour connaître les versions compatibles de Python, lancez

=== Utiliser l'extension ===
1. Chargez les modules requis.

2. Importez parasail 1.3.4.

L'importation est réussie quand la commande ne retourne rien.

=== Exemple ===
Comparez les résultats d'un alignement local avec BioPython et parasail.

1. Préparez le script Python.

2. Préparez le script de soumission selon votre environnement.

2.1. Identify available wheels first :

Installez maintenant la version choisie dans votre environnement virtuel.

3. Soumettez la tâche avec

4. Une fois la tâche terminée, vérifiez le résultat dans le fichier de sortie de l'ordonnanceur Slurm.

==== Paquets Python disponibles ====
Les exigences des paquets Python qui dépendent de parasail seront satisfaites en chargeant le module parasail.
 grep parasail
|result=
parasail                           1.3.4
}}