---
title: "Samtools/fr"
url: "https://docs.alliancecan.ca/wiki/Samtools/fr"
category: "General"
last_modified: "2025-05-03T09:12:56Z"
page_id: 28379
display_title: "Samtools"
language: "fr"
---

== Description ==
Samtools est un ensemble de programmes permettant d'interagir avec des données de séquençage à haut débit. Samtools est étroitement lié à BCFtools et HTSlib. Au besoin,  consultez la documentation.

*Samtools permettent la lecture, l'écriture, l'édition, l'indexation et l'affichage des formats SAM, BAM et CRAM;
*BCFtools permettent la lecture et l'écriture des fichiers BCF2, VCF et gVCF, en plus de l'appel, le filtrage et le résumé des variants de séquences courtes de SNP et indel;
*HTSlib est une bibliothèque en C pour lire et  écrire les données de séquençage haut débit avec Samtools et BCFtools.

Remarque : Nous n'abordons pas ici toutes les fonctionnalités. Pour la liste de tous les outils, consultez Samtools.

Pour charger la version par défaut, lancez la commande

Pour plus d'information sur la commande module et sur comment trouver d'autres versions de Samtools, voir  Utiliser des modules.

== Utilisation ==

Samtools propose divers outils pour manipuler les alignements dans les formats SAM et BAM. La tâche la plus courante consiste à convertir vos fichiers SAM (Sequence Alignment/Map) en fichiers BAM (version binaire de SAM). Les fichiers BAM sont des versions compressées des fichiers SAM et sont beaucoup plus compacts. Ils sont faciles à manipuler et un excellent choix pour le stockage de grands alignements de séquences nucléotidiques.

CRAM est un format plus récent pour le même type de données et offre encore plus de compression.

=== Conversion de SAM à BAM ===

Avant la conversion, vérifiez si votre fichier BAM a un en-tête avec le caractère « @ ». Vous pouvez vérifier ceci avec la commande view.

Si le fichier SAM a un en-tête, vous pouvez utiliser l'une des options suivantes pour le convertir en BAM.

Si les en-têtes sont absents, vous pouvez utiliser le fichier de référence FASTA pour mapper les lectures.

=== Tri et indexation des fichiers BAM ===

Vous devrez peut-être trier et indexer les fichiers BAM pour plusieurs applications que vous utiliserez par la suite.

Les fichiers SAM peuvent être directement convertis en fichiers BAM triés avec la fonction | (barre verticale) de l'interpréteur.

Un fichier BAM trié accompagné de son fichier index (extension .bai) est souvent un prérequis à d'autres processus tels que les appels de variantes, le décompte des fonctionnalités, etc.

=== Multifil et/ou GNU parallel ===

Plusieurs fichiers SAM sont souvent traités simultanément.
Un script comportant une boucle est une bonne solution, comme suit

Samtools fonctionne généralement sur un seul cœur par défaut, mais dans certains cas il est possible d'améliorer la performance en  travaillant sur plusieurs cœurs ou avec GNU Parallel.

Samtools peut travailler sur plusieurs cœurs (multithreading) avec l'indicateur -@.

Un autre moyen de travailler sur plusieurs cœurs est d'utiliser  GNU Parallel pour traiter plusieurs fichiers simultanément.

 parallel -j ${SLURM_CPUS_PER_TASK} "time samtools view -bS {}  samtools sort -o {.}_mt_sorted.bam"
}}

Le script ci-dessus exécutera view et sort sur quatre fichiers SAM simultanément. Si vous avez plusieurs fichiers d'entrée, modifiez la requête --cpous-per-task.