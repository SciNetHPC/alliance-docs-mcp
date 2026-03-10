---
title: "Structure/fr"
url: "https://docs.alliancecan.ca/wiki/Structure/fr"
category: "General"
last_modified: "2022-01-24T20:49:09Z"
page_id: 9910
display_title: "Structure"
language: "fr"
---

== Description ==

structureSite web :  http://web.stanford.edu/group/pritchardlab/structure.html est un logiciel gratuit qui utilise les données de génotypage de locus multiples dans la recherche en structure génétique des populations. Il est utilisé pour inférer la présence de populations distinctes, assigner des individus à des populations, étudier les zones hybrides, identifier les individus migrants et estimer la fréquence des allèles dans les populations avec individus migrants. structure peut être employé avec la plupart des marqueurs génétiques couramment utilisés dont  SNPS, RFLP,  AFLP et les microsatellites.
J.K. Pritchard, M. Stephens, and P. Donnelly. Inference of population structure using multilocus genotype data. Genetics, 155:945–959, 2000. https://www.ncbi.nlm.nih.gov/pmc/articles/PMC1461096/
M.J. Hubisz, D. Falush, M. Stephens, and J.K. Pritchard. Inferring weak population structure with the assistance of sample group information. Molecular Ecology Resources, 9(5):1322–1332, 2009. doi: https://doi.org/10.1111/j.1755-0998.2009.02591.x

== Versions installées ==

Consultez la liste des logiciels disponibles  pour connaître la version à jour du module structure (structure/2.3.4 en date de janvier 2019).

== Utilisation ==

Au démarrage en ligne de commande sans options, structure s’attend à trouver les trois fichiers suivants dans le répertoire courant :
* mainparams (exemples mainparams),
* extraparams(exemplesextraparams),
* fichier de données; le fichier peut être identifié par le paramètre INFILE de mainparams ou en ligne de commande avec -i.

Pour la description des paramètres et des formats de fichiers, consultez la documentationVersion 2.3.4 (PDF) au chapitre 7, Running structure from the command line. La section 7.4 décrit plusieurs options pour modifier les paramètres.

Voici un exemple d’un script de soumission :

== Exécution parallèle : StrAuto et StructureHarvester ==
structure n’est pas conçu pour travailler en parallèle; cependant, pour travailler avec la méthode Evanno
G. Evanno, S. Regnaut, and J. Goudet. Detecting the number of clusters of individuals using the software structure: a simulation study. Molecular Ecology, 14:2611–2620, 2005. DOI: https://doi.org/10.1111/j.1365-294X.2005.02553.x
les outils StrAuto
Site web StrAuto : https://vc.popgen.org/software/strauto/
Guide de l'utilisateur StrAuto : https://vc.popgen.org/software/strauto/strauto_doc.pdf
Chhatre, VE & Emerson KJ. StrAuto: Automation and parallelization of STRUCTURE analysis. BMC Bioinformatics (2017) 18:192. doi: http://dx.doi.org/10.1186/s12859-017-1593-0 et StructureHarvester
Site web StructureHarvester : http://alumni.soe.ucsc.edu/~dearl/software/structureHarvester/ ; GitHub: https://github.com/dentearl/structureHarvester
Earl, Dent A. and vonHoldt, Bridgett M. STRUCTURE HARVESTER: a website and program for visualizing STRUCTURE output and implementing the Evanno method. Conservation Genetics Resources (2011) DOI: 10.1007/s12686-011-9548-7
sont utilisés pour automatiser la préparation de processus multiples et l’agrégation des résultats.

=== Considérations pratiques ===
Le chapitre 8 du manuel de l’utilisateur pour StrAuto montre un exemple de tâches exécutées sur des grappes de CHP avec l’ordonnanceur Slurm. Cet exemple fonctionne mieux lorsque le nombre total de processus est un multiple du nombre de cœurs demandés. Si ce n’est pas le cas, certains des CPU qui ont été alloués ne seront pas utilisés pendant que les derniers processus structure sont exécutés, ce qui n’est pas une utilisation maximale des ressources de calcul.

De plus, le temps d’exécution demandé doit être suffisant pour tenir compte des multiples processus structure qui vont suivre. Ceci est un choix raisonnable dans le cas de processus qui exigent un temps d’exécution relativement court, mais les tâches plus longues resteront plus longtemps en attente en raison de la politique d'ordonnancement.

=== Exécution de processus de longue durée ===

Le script create_strauto_slurm_scripts.py au bas de cette section est conçu pour vous aider à exécuter des tâches Structure ordonnancées par Slurm sur les grappes de Calcul Canada. Les outils requis sont Structure, StrAuto et StructureHarvester.

Utilisation :

* Placez le script create_strauto_slurm_scripts.py dans un répertoire avec
:* strauto_1.py et input.py de StrAuto;
:* structureHarvester.py et harvesterCore.py de StructureHarvester;
:* le fichier de données. my_dataset.str.
* Rendez create_strauto_slurm_scripts.py exécutable avec

: * Vous devriez maintenant avoir les fichiers suivants :

* Modifiez les paramètres de input.py tel que décrit dans le manuel StrAutol.
:* Assurez-vous de configurer l'option parallel = True (question 23).
* Ajustez les paramètres (lignes 65-70) :
:* Définissez une valeur appropriée pour max_jobtime, soit une durée minimale pour l’exécution d’un processus structure unique.
:* Utilisez slurm_account pour indiquer dans quel compte Slurm soumettre la tâche.
:* Définissez une valeur appropriée pour submit_delay afin d’éviter de surcharger l’ordonnanceur.
* Exécutez les commandes suivantes :

* Soumettez les tâches avec

* Lorsque toutes les tâches sont terminées, lancez bash post_strauto.sh pour effectuer l’agrégation des résultats et exécuter StructureHarvester.

==== Script 'create_strauto_slurm_scripts.py' ====

== Références ==