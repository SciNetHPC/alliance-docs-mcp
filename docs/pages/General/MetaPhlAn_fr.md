---
title: "MetaPhlAn/fr"
url: "https://docs.alliancecan.ca/wiki/MetaPhlAn/fr"
category: "General"
last_modified: "2022-11-28T20:16:22Z"
page_id: 21768
display_title: "MetaPhlAn"
language: "fr"
---

MetaPhlAn est un outil informatique permettant de profiler la composition des communautés microbiennes (bactéries, archées et eucaryotes) à partir de données de séquençage métagénomique (c'est-à-dire non 16S) au niveau de l'espèce. Avec StrainPhlAn, il est possible d'effectuer un profilage microbien précis au niveau de la souche. Bien que la pile logicielle de nos grappes contienne des modules pour quelques versions plus anciennes (2.2.0 et 2.8), nous attendons désormais des utilisateurs qu'ils installent les versions récentes à l'aide d'un  environnement virtuel Python.

Pour plus d'information, voir le site wiki de MetaPhlan.

= Wheels disponibles=
Pour connaître les wheels disponibles, utilisez la commande avail_wheels.

= Télécharger les bases de données =
MetaPhlAn exige qu'un ensemble de bases de données soit téléchargé dans  $SCRATCH.

Important : La base de données doit se trouver dans  $SCRATCH.

Téléchargez les bases de données à partir deSegatalab FTP.

1. À partir d'un nœud de connexion, créez le répertoire pour les données.
$SCRATCH/metaphlan_databases
|mkdir -p $DB_DIR
|cd $DB_DIR
}}

2. Téléchargez les données.

Remarque ː Cette étape doit se faire à partir d'un nœud de connexion et non à partir d'un nœud de calcul.

3. Faites l'extraction des données téléchargées en utilisant par exemple une tâche interactive.
 --cpus-per-task2 --mem10G
}}
Décompressez les bases de données.

= Utiliser MetaPhlAn =
Une fois que les fichiers des bases de données ont été téléchargés et extraits, vous pouvez soumettre une tâche.  Le script suivant peut servir d'exemple ː
$SCRATCH/metaphlan_databases

# Générez votre enironnement virtuel dans $SLURM_TMPDIR
virtualenv --no-download ${SLURM_TMPDIR}/env
source ${SLURM_TMPDIR}/env/bin/activate

# Installez metaphlan et ses dépendances.
pip install --no-index --upgrade pip
pip install --no-index metaphlan==X.Y.Z  # EDIT: the required version here, e.g. 4.0.3

# Réutilisez le nombre de cœurs (--cpus-per-task=4) alloués à votre tâche.
# Il est important d'utiliser --index et --bowtie2db pour que MetaPhlAn soit exécuté à l'intérieur de la tâche.
metaphlan metagenome.fastq --input_type fastq -o profiled_metagenome.txt --nproc $SLURM_CPUS_PER_TASK --index mpa_vJan21_CHOCOPhlAnSGB_202103 --bowtie2db $DB_DIR --bowtie2out metagenome.bowtie2.bz2
}}

Soumettez ensuite la tâche à l'ordonnanceur.