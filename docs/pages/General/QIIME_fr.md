---
title: "QIIME/fr"
url: "https://docs.alliancecan.ca/wiki/QIIME/fr"
category: "General"
last_modified: "2024-12-09T23:00:34Z"
page_id: 12861
display_title: "QIIME"
language: "fr"
---

QIIME (pour Quantitative Insights Into Microbial Ecology) est un pipeline bioinformatique open source pour l’analyse de microbiomes. À partir de données brutes de séquençage d’ADN générées par des plateformes comme Illumina, QIIME produit des graphiques et statistiques de haute qualité pour, entre autres, le démultiplexage, le filtrage de qualité, la sélection d’OTU, l’attribution taxonomique, la reconstruction phylogénétique et l’analyse de la diversité.

Remarque : QIIME 2 a remplacé QIIME 1 en janvier 2018; la version 1 n'est plus supportée.

Remarque : Depuis février 2020, il n'est pas possible d'installer QIIME avec Anaconda ou Miniconda sur nos grappes en raison de plusieurs problèmes dus aux environnements Conda.

== Module pour QIIME2 ==
QIIME2 est disponible en chargeant un module qui enveloppe un conteneur. Pour connaître les versions disponibles, lancez

Une fois le module chargé, vous pouvez lancer

=== Exemple ===
Voici un exemple simple d'un script pour soumettre une tâche :

==Installation ==
L’installation peut se faire en utilisant Apptainer ou EasyBuild. Il est préférable d'utiliser Apptainer pour éviter que plusieurs milliers de fichiers soient générés dans votre répertoire /home, ce qui risquerait de dépasser le quota sur le nombre de fichiers.

=== Utilisation avec Apptainer ===

Les développeurs de QIIME2 publient des images sur Quay.io.
Pour utiliser ces images avec nos ressources, il faut d'abord  créer une image Apptainer comme suit :

Cette étape du build pourrait prendre plus d'une heure, mais il ne faut l'effectuer qu'une seule fois. Sauvegardez le fichier image (dans notre exemple qiime2-2021.11.sif) pour pouvoir le réutiliser plus tard.

Exécutez ensuite votre programme comme décrit dans la page Apptainer. De façon générale, chaque commande QIIME est exécutée dans un énoncé apptainer exec comme suit%nbsp;:

Votre script SBATCH ressemblerait à

#!/bin/bash
#SBATCH --time=15:00:00
#SBATCH --account=def-someuser

apptainer exec -B $PWD:/home -B /scratch/someuser:/outputs \
  -B /project/def-somePI/someuser/path/to/inputs:/inputs qiime2-2021.11.sif \
  qiime tools import --type 'FeatureData[Sequence]' \
  --input-path /inputs/some_fastafile.fa \
  --output-path /outputs/some_output_feature.qza

apptainer exec -B $PWD:/home -B /scratch/someuser:/outputs \
  -B /project/def-somePI/someuser/path/to/inputs:/inputs qiime2-2021.11.sif \
  qiime tools import \
  --type 'FeatureData[Taxonomy]' \
  --input-format HeaderlessTSVTaxonomyFormat \
  --input-path /inputs/some_taxonomy_file.tax \
  --output-path /outputs/some_output_ref-taxonomy.qza

apptainer exec -B $PWD:/home -B /scratch/someuser:/outputs \
  -B /project/def-somePI/someuser/path/to/inputs:/inputs qiime2-2021.11.sif \
  qiime feature-classifier fit-classifier-naive-bayes \
  --i-reference-reads  /outputs/some_output_feature.qza \
  --i-reference-taxonomy /outputs/some_output_ref-taxonomy.qza \
  --o-classifier /outputs/some_output_classifier.qza

Notez qu'il est important d'utiliser l'option bind (-B) avec chacun des répertoires avec lesquels vous voulez travailler quand des programmes sont exécutés dans votre conteneur. Pour plus d'information, voyez ce webinaire Apptainer.

La première fois que des données sont importées en format QIIME, vous pourriez recevoir un message semblable à

Timezone offset does not match system offset: 0 != -18000. Please, check your config files.

Vous pouvez contourner ceci en définissant un fuseau horaire avant d'invoquer Apptainer, comme suit :

'UTC'
|apptainer exec qiime2-2021.11.sif qiime tools import ...
}}

=Références =

Site web QIIME