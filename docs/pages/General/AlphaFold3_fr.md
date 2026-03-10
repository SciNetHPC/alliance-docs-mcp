---
title: "AlphaFold3/fr"
url: "https://docs.alliancecan.ca/wiki/AlphaFold3/fr"
category: "General"
last_modified: "2025-07-23T15:17:00Z"
page_id: 27989
display_title: "AlphaFold3"
language: "fr"
---

Nous décrivons ici l'utilisation de AlphaFold v3.0.

Le code source et la documentation se trouvent sur leur page GitHub.
Toute publication qui divulgue des résultats découlant de l'utilisation de ce code source ou des paramètres du modèle doit citer le document AlphaFold3.

== Versions disponibles ==
Sur nos grappes, AlphaFold3 est disponible sous forme de paquets préconstruits (wheels) Pour les lister, utilisez avail_wheels.

AlphaFold2 est encore disponible (voir la documentation).

== Créer un fichier des dépendances requises ==

1. Chargez les dépendances de AlphaFold3.

2. Téléchargez le script d'exécution.

3. Créez et activez un environnement virtuel Python.

4. Installez une version de AlphaFold3 ainsi que ses dépendances Python
X.Y.Z
}}
où X.Y.Z est la version spécifique, par exemple 3.0.0.
N'entrez pas le numéro de la version si vous voulez installer la plus récente.

5. Compilez les données nécessaires.

Ceci crée des fichiers de données dans l'environnement virtuel.

6. Validez.

7. Gelez l'environnement et l'ensemble des requis.

8. Désactivez l'environnement.

9. Nettoyez et supprimez l'environnement virtuel.

L'environnement virtuel sera plutôt créé dans votre tâche.

== Modèle ==
Vous pouvez obtenir le modèle de Google, qui répond habituellement dans les 2 ou 3 jours ouvrables; voir Obtaining Model Parameters.

== Bases de données ==
AlphaFold3 nécessite un ensemble de bases de données.

Important : Les bases de données doivent résider dans le répertoire $SCRATCH.

1. Téléchargez le script de téléchargement.

2. Téléchargez les bases de données.

== Exécution par étapes ==
Alphafold3 doit être exécuté par étapes, c'est-à-dire
# séparer le pipeline de données pour CPU seulement et le modèle d'inférence (qui demande un GPU) pour optimiser les coûts et l'utilisation des ressources;
# cacher les résultats de la recherche de MSA/modèle, pour ensuite réutiliser le JSON augmenté pour plusieurs différentes inférences ou pour des variations d'autres fonctionnalités (par exemple un ligand).

Pour des références, voir
* inputs
* outputs
* performance

=== 1. Pipeline de données (CPU) ===
Modifiez le script suivant selon vos besoins.

=== 2. Inférence de modèle ===
Modifiez le script suivant selon vos besoins.

=== 3. Soumettre une tâche ===

Soumettez la tâche à l'ordonnanceur.

==== Tâches indépendantes ====

Attendez la fin et soumettez ensuite la deuxième étape.

==== Tâches dépendantes ====
$(sbatch alphafold3-data.sh)
|jid2$(sbatch --dependencyafterok:$jid1 alphafold3-inference.sh)
|sq
}}
Si la première étape échoue, vous devez annuler manuellement la deuxième étape.

== Dépannage ==
=== Mémoire insuffisante (GPU) ===
Si vous voulez exécuter AlphaFold3 avec plus de 5120 jetons ou sur un GPU de mémoire moindre,  (par exemple sur un A100 avec 40Go de mémoire), vous pouvez activer la fonctionnalité de mémoire unifiée.

Dans le script de soumission à l'étape d'inférence, ajoutez les variables d'environnement suivantes :

export XLA_PYTHON_CLIENT_PREALLOCATE=false
export TF_FORCE_UNIFIED_MEMORY=true
export XLA_CLIENT_MEM_FRACTION=2.0  # 2 x 40GB = 80 GB

et ajuster en conséquence la quantité de mémoire allouée à la tâche, par exemple #SBATCH --mem=80G.