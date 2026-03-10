---
title: "BUSCO/fr"
url: "https://docs.alliancecan.ca/wiki/BUSCO/fr"
category: "General"
last_modified: "2026-01-13T13:26:02Z"
page_id: 9866
display_title: "BUSCO"
language: "fr"
---

BUSCO (pour Benchmarking Universal Single-Copy Orthologs) est une application qui permet d'évaluer la complétude de l'assemblage et de l'annotation de génomes.

Pour plus d'information, consultez le manuel de l'utilisateur.

== Versions disponibles ==
Les versions récentes sont disponibles dans des wheels et les plus anciennes versions sont dans un module (voir la section Modules ci-dessous).

Pour connaître la dernière version disponible, lancez

== Wheel Python ==
=== Installation ===
1. Chargez les modules requis.

2. Créez l'environnement virtuel.

3. Installez le wheel et ses dépendances.
6.0.0
}}

4. Validez l'installation.

5.  Gelez l’environnement et le fichier requirements.txt pour utiliser ce fichier, voir le script bash montré au point 8.

=== Utilisation ===
==== Ensembles de données ====
6. Avant de soumettre une tâche, les ensembles de données doivent être téléchargés de
BUSCO data.

Pour connaître les ensembles de données disponibles, entrez busco --list-datasets dans votre terminal.

Vous pouvez utiliser l'une des deux commandes suivantes :
*busco
*wget

===== 6.1  Téléchargement avec la commande busco =====
Cette option est recommandée. Entrez la commande suivante dans votre répertoire de travail pour télécharger l’ensemble de données, par exemple

Il est aussi possible de télécharger plusieurs ensembles de données en une opération en ajoutant les arguments all, prokaryota, eukaryota ou virus, par exemple

Ceci permet de
::1. créer une hiérarchie pour les ensembles de données,
::2. télécharger les ensembles de données appropriés,
::3. décompresser le ou les fichiers,
::4. si plusieurs fichiers sont téléchargés, ils seront automatiquement ajoutés au répertoire des lignées.

La hiérarchie sera semblable à

* busco_downloads/

::* information/

::::lineages_list.2021-12-14.txt

::* lineages/

::::bacteria_odb10

::::actinobacteria_class_odb10

::::actinobacteria_phylum_odb10

::* placement_files/

::::list_of_reference_markers.archaea_odb10.2019-12-16.txt

Tous les fichiers de lignées se trouveront alors dans busco_downloads/lineages/. La présence de --download_path busco_downloads/ dans la ligne de commande BUSCO indiquera où trouver l’argument --lineage_dataset bacteria_odb10 pour l'ensemble de données. Si busco_download n’est pas votre répertoire de travail, il faudra fournir le chemin complet.

=====6.2 Téléchargement avec la commande wget  =====

Tous les fichiers doivent être décompressés avec tar -xvf file.tar.gz.

==== Test ====
7. Téléchargement des fichiers de génome.

8. Exécution.

Pour un seul génome :

Pour plusieurs génomes, le répertoire genome/ doit se trouver dans le répertoire courant, autrement il faut donner le chemin complet :

La commande pour un seul génome devrait être exécutée sous les 60 secondes. Les tâches de production qui nécessitent plus de temps doivent être soumises à l'ordonnanceur.

===== Conseils pour BUSCO =====

Utilisez --in genome.fna pour analyser un seul fichier.

Utilisez --in genome/ pour analyser plusieurs fichiers.

===== Conseils pour Slurm  =====
Utilisez --offline pour éviter l'utilisation de l’internet.

Utilisez --cpu avec $SLURM_CPUS_PER_TASK dans le script de la tâche pour utiliser le nombre alloué de CPU.

Utilisez --restart pour reprendre une tâche interrompue.

====Soumettre une tâche====

Vous pouvez soumettre le script suivant avec sbatch run_busco.sh.

====Paramètres Augustus ====
9. Si vous avez plus d'expérience, vous pouvez utiliser les paramètres Argutus :  --augustus_parameters="--yourAugustusParameter".

*Copiez le répertoire config d'Augustus à un endroit où la lecture est possible.

*Assurez-vous de définir la variable d'environnement  AUGUSTUS_CONFIG_PATH.
$HOME/augustus_config}}

====Paramètres SEPP ====
10. Pour utiliser ces paramètres, SEPP doit être installé localement dans votre environnement virtuel, ce que vous devez faire à partir du nœud de connexion.

10.1. Activez votre environnement virtuel BUSCO.

10.2. Installez DendroPy.

10.3. Installez SEPP.

10.4. Validez l'installation.

10.5. Puisque SEPP est installé localement, vous ne pouvez pas utiliser le script ci-dessus pour créer votre environnement virtuel. Pour activer votre environnement virtuel, ajoutez la commande suivante sur la ligne qui suit la commande de chargement du module.

== Modules ==

1. Chargez les modules nécessaires.

Ceci charge aussi les modules pour Augustus, BLAST+, HMMER et d'autres paquets requis par BUSCO.

2. Copiez le fichier de configuration

ou

3. Modifiez le fichier de configuration. Les endroits où se trouvent les outils externes sont définis dans la dernière section.

4.  Copiez le répertoire config d’Augustus à un endroit où la lecture est possible.

5. Vérifiez si tout fonctionne bien.

$HOME/busco_config.ini
|export AUGUSTUS_CONFIG_PATH$HOME/augustus_config
|run_BUSCO.py --in $EBROOTBUSCO/sample_data/target.fa --out TEST --lineage_path $EBROOTBUSCO/sample_data/example --mode genome
}}

La commande run_BUSCO.py devrait être exécutée sous les 60 secondes. Les tâches de production qui nécessitent plus de temps doivent être soumises à l'ordonnanceur.

= Dépannage =
== Erreur : Cannot write to Augustus config path ==
Assurez-vous d’avoir copié le répertoire config d’Augustus à un endroit où la lecture est possible et d’avoir exporté la variable AUGUSTUS_CONFIG_PATH.