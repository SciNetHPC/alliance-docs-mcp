---
title: "Open Babel/fr"
url: "https://docs.alliancecan.ca/wiki/Open_Babel/fr"
category: "General"
last_modified: "2024-03-04T22:15:31Z"
page_id: 24884
display_title: "Open Babel"
language: "fr"
---

==  Description ==
Open Babel est une boîte à outils conçue pour parler les nombreux langages des données chimiques. Il s'agit d'un projet ouvert et collaboratif permettant à quiconque de rechercher, convertir, analyser ou stocker les données provenant de la modélisation moléculaire, de la chimie, des matériaux solides, de la biochimie ou de domaines connexes.

Consultez le Open Babel User Guide.

Deux types de modules sont installés sur nos grappes :

== openbabel ==
Cette version séquentielle peut être utilisée en toute sécurité même sur les nœuds de connexion pour convertir les formats des fichiers de structure chimique. Dans la plupart des cas, c'est le bon module.

==== Exemple ====
str&3dyes&id171" -O acetic_acid.mol
| obabel  -i mol  acetic_acid.mol  -o pdb  -O acetic_acid.pdb
}}
Remarques :
* La commande wget télécharge le fichier acetic_acid.mol.
* La commande obabel convertit la molécule décrite dans acetic_acid.mol du format .mol au format .pdb.

== openbabel-omp ==
Cette version offre la parallélisation avec OpenMP.

La version parallèle est utile pour convertir un très grand nombre de structures moléculaires ou calculer un grand nombre de descripteurs chimio-informatiques pour plusieurs molécules.

Assurez-vous de définir la variable d'environnement OMP_NUM_THREADS afin d'indiquer à Open Babel combien de CPU il peut utiliser.

==== Exemple ====
La prochaine tâche utilise le fichier SDF   many_molecules.sdf qui devrait contenir une base de données de plusieurs  molécules et génère des représentations canoniques SMILES pour chacune d'elles, en utilisant deux cœurs CPU.

== Python ==
Les fonctionnalités d'Open Babel peuvent être utilisées à partir d'autres langages tels que Python. L'interface Python pour Open Babel est ajoutée aux modules openbabel eg openbabel-omp  en tant qu'extensions. Par conséquent, les paquets openbabel et pybel peuvent être utilisés après avoir chargé openbabel et un module Python compatible.

==== Exemple ====

 $ module load python/3.11 openbabel/3.1.1
 $ python
 Python 3.11.5 (main, Sep 19 2023, 19:49:15) [GCC 11.3.0] on linux
 >>> import openbabel
 >>> print(openbabel.__version__)
 3.1.1.1
 >>> from openbabel import pybel
 >>>