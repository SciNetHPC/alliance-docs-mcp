---
title: "LAMMPS/fr"
url: "https://docs.alliancecan.ca/wiki/LAMMPS/fr"
category: "General"
last_modified: "2025-03-06T14:11:18Z"
page_id: 9162
display_title: "LAMMPS"
language: "fr"
---

Page enfant de  Simulation biomoléculaire

= Généralités =

LAMMPS (pour large-scale atomic/molecular massively parallel simulator) est un logiciel classique de dynamique moléculaire distribué par Sandia National Laboratories du ministère de l’Énergie des États-Unis.

* Site web du projet : http://lammps.sandia.gov/
* Documentation
* Liste de messagerie

La parallélisation se fait avec  MPI et  OpenMP et LAMMPS peut être exécuté sur GPU.

= Champs de force =

Les champs de force disponibles sont listés à la section Interatomic potentials du site web, classés selon leur forme fonctionnelle (soit par paire, N corps, etc.). LAMMPS pouvant traiter un grand nombre de champs de force, il peut être utilisé pour la modélisation dans plusieurs domaines d’application, par exemple :

* Biomolécules : CHARMM, AMBER, OPLS, COMPASS (classe 2), coulombiques longue portée via PPPM, dipôles de moment, etc.
* Polymères : liaison d’atomes, union d’atomes, gros grains (chaînes globulaires FENE), bond-breaking, etc.
* Matériaux : EAM et MEAM pour les métaux, Buckingham, Morse, Yukawa, Stillinger-Weber, Tersoff, EDIP, COMB, SNAP, etc.
* Réactions : AI-REBO, REBO, ReaxFF, eFF
* Échelle mésoscopique : granulaire, DPD, Gay-Berne, colloïdal, péridynamiques, DSMC, etc.

Les potentiels peuvent aussi être combinés dans des systèmes hybrides, par exemple eau sur métal, interfaces polymère/semi-conducteur, colloïdes en solution, etc.

= Versions et paquets =

Pour connaître les versions disponibles, lancez module spider lammps (voir Utiliser des modules).

Les numéros de version de LAMMPS comprennent la date de sortie au format AAAAMMJJ. Exécutez

 module avail lammps

pour connaître les versions installées et sélectionner celle que vous voulez utiliser.

Il peut y avoir plusieurs modules pour une même version. Par exemple, la version du 31 mars 2017 a les trois modules suivants :

* lammps/20170331 développé sous MPI
* lammps-omp/20170331 USER-OMP (compatible OpenMP)
* lammps-user-intel/20170331 USER-INTEL

Ces versions fonctionnent aussi avec GPU; le module  CUDA doit être chargé avant le module LAMMPS.

 $ module load cuda
 $ module load lammps-omp/20170331

Le nom de l’exécutable peut être différent selon la version. Toutes les versions installées sur nos grappes ont le lien symbolique lmp; vous pouvez donc exécuter LAMMPS en faisant appel à lmp peu importe le module que vous utilisez.

Pour connaître le nom original de l’exécutable d’un module en particulier, faites lister les fichiers dans le répertoire ${EBROOTLAMMPS}/bin avec, par exemple

 $ module load lammps-omp/20170331
 $ ls ${EBROOTLAMMPS}/bin/
 lmp lmp_icc_openmpi

où l’exécutable est lmp_icc_openmpi et lmp est le lien symbolique associé.

Il existe différents modules pour la même version, dépendant des paquets qui sont inclus. Les versions de LAMMPS les plus récentes comprennent environ 60 paquets différents qui peuvent être activés ou désactivés à la compilation du programme. Ce ne sont pas tous les paquets qui peuvent être activés dans un même exécutable. Consultez la documentation sur les paquets. Si votre simulaiton ne fonctionne pas avec un module, il est possible qu'un des paquets nécessaire n'a pas été activé.

Pour certains modules LAMMPS, nous fournissons le fichier list-packages.txt qui liste les paquets activés (Supported) et non activés (Not Supported). Une fois que vous avez chargé un module, lancez cat ${EBROOTLAMMPS}/list-packages.txt pour en connaître le contenu.

Si list-packages.txt est introuvable, vous pourriez être capable de déterminer quels sont les paquets disponibles en ouvrant le fichier de recette EasyBuild avec $EBROOTLAMMPS/easybuild/LAMMPS*.eb.  Les paquets disponibles se trouvent dans le bloc general_packages.

= Exemples de fichiers d'entrée =

Le fichier ci-dessous peut être utilisé avec l’un ou l’autre des scripts de tâche donnés en exemple.

= Performance =

Dans le cas de simulations en dynamique moléculaire, le calcul des interactions de paires entre particules occupe la plus grande part du temps CPU. LAMMPS utilise la méthode de décomposition des domaines pour répartir le travail aux processeurs disponibles en assignant à chacun une partie de la boîte de simulation. Il est nécessaire que les processeurs communiquent entre eux pendant le calcul des interactions entre particules. Pour un nombre déterminé de particules, plus le nombre de processeurs est élevé, plus il y a de parties à la boîte de simulation qui s’échangent de l’information. Ainsi, plus il y a de processeurs, plus longue est la durée du temps de communication, ce qui cause éventuellement la faible efficacité du CPU.

Avant d’exécuter des simulations pour des problèmes d’une certaine taille ou avec des boîtes à plusieurs parties, faites des tests pour voir l’impact du nombre de cœurs sur la performance du programme. Effectuez des tests courts avec un nombre différent de cœurs pour identifier le nombre de cœurs susceptible d’offrir la meilleure efficacité; les résultats demeurent cependant approximatifs.

Le tableau suivant montre la durée pour la simulation d’un système de 4000 particules avec 12 tâches MPI. En utilisant 12 cœurs, le système de 4000 atomes est réparti sur 12 petites boîtes et l’efficacité est très basse. Le calcul des interactions de paires occupe 46.45% du temps et la communication entre processeurs 44.5%. La proportion importante du temps de communication est due au fait qu’un si petit système utilise un grand nombre de petites boîtes.

Durée de boucle 15.4965 pour 12 processus de 25000 étapes avec 4000 atomes.
Performance : 696931.853 tau/jour, 1613.268 timesteps/s.
CPU utilisé à 90.2% avec 12 tâches MPI x 1 fil OpenMP.	Durée de boucle 15.4965 pour 12 processus de 25000 étapes avec 4000 atomes.
Performance : 696931.853 tau/jour, 1613.268 timesteps/s.
CPU utilisé à 90.2% avec 12 tâches MPI x 1 fil OpenMP.	Durée de boucle 15.4965 pour 12 processus de 25000 étapes avec 4000 atomes.
Performance : 696931.853 tau/jour, 1613.268 timesteps/s.
CPU utilisé à 90.2% avec 12 tâches MPI x 1 fil OpenMP.	Durée de boucle 15.4965 pour 12 processus de 25000 étapes avec 4000 atomes.
Performance : 696931.853 tau/jour, 1613.268 timesteps/s.
CPU utilisé à 90.2% avec 12 tâches MPI x 1 fil OpenMP.	Durée de boucle 15.4965 pour 12 processus de 25000 étapes avec 4000 atomes.
Performance : 696931.853 tau/jour, 1613.268 timesteps/s.
CPU utilisé à 90.2% avec 12 tâches MPI x 1 fil OpenMP.	Durée de boucle 15.4965 pour 12 processus de 25000 étapes avec 4000 atomes.
Performance : 696931.853 tau/jour, 1613.268 timesteps/s.
CPU utilisé à 90.2% avec 12 tâches MPI x 1 fil OpenMP.
SECTION      	durée minimale	durée moyenne	durée maximale	variation moyenne (%)	total (%)
paires       	6.6964        	7.1974       	7.9599        	14.8                 	46.45
voisins      	0.94857       	1.0047       	1.0788        	4.3                  	6.48
communication	6.0595        	6.8957       	7.4611        	17.1                 	44.50
sortie       	0.01517       	0.01589      	0.019863      	1.0                  	0.10
modification 	0.14023       	0.14968      	0.16127       	1.7                  	0.97
autre        	--            	0.2332       	--            	--                   	1.50

Dans le dernier tableau, le temps de communication est comparé au temps de calcul des paires pour différents nombres de cœurs.

     	2048 atomes	2048 atomes	4000 atomes	4000 atomes	6912 atomes	6912 atomes	13500 atomes	13500 atomes
cœurs	paires     	comm.      	paires     	comm.      	paires     	comm.      	paires      	comm.
1    	73.68      	1.36       	73.70      	1.28       	73.66      	1.27       	73.72       	1.29
2    	70.35      	5.19       	70.77      	4.68       	70.51      	5.11       	67.80       	8.77
4    	62.77      	13.98      	64.93      	12.19      	67.52      	8.99       	67.74       	8.71
8    	58.36      	20.14      	61.78      	15.58      	64.10      	12.86      	62.06       	8.71
16   	56.69      	20.18      	56.70      	20.18      	56.97      	19.80      	56.41       	20.38