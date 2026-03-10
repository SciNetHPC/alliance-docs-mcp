---
title: "Dalton/fr"
url: "https://docs.alliancecan.ca/wiki/Dalton/fr"
category: "General"
last_modified: "2018-09-17T17:26:09Z"
page_id: 8928
display_title: "Dalton"
language: "fr"
---

= Introduction =

Le noyau de la suite logicielle Dalton2016 est constitué de deux puissantes applications pour l'étude des structures électroniques de molécules : Dalton et LSDalton. Ensemble, ces applications offrent des fonctionnalités étendues pour le calcul des propriétés moléculaires aux niveaux théoriques HF, DFT, MCSCF et CC. Plusieurs de ses propriétés sont uniques à la suite Dalton2016.

* site web du projet : http://daltonprogram.org/
* documentation : http://daltonprogram.org/documentation/
* forum : http://forum.daltonprogram.org/

= Modules =

$ module load nixpkgs/16.09 intel/2016.4 openmpi/2.0.2 dalton/2017-alpha

Remarquez que dalton/2017-alpha dépend d’une version OpenMPI autre que la version par défaut. Pour de l’information sur la commande module voyez Utiliser des modules.

= Utilisation =

Voici un exemple ː

* fichier d'entrée : dft_rspexci_nosym.dal (voir les exemples ci-dessous)
* spécification de la molécule : H2O_cc-pVDZ_nosym.mol (voir les exemples ci-dessous)
* pour utiliser les bases atomiques, ajouter l'option -b ${BASLIB} en ligne de commande (voir les exemples ci-dessous)
* pour définir le nombre de processus avec une option en ligne de commande ou une variable d’environnement :
** ajoutez l’option -N ${SLURM_NTASKS} en ligne de commande pour le lanceur (voir Script 1 dans les exemples ci-dessous)
** ou  export DALTON_NUM_MPI_PROCS=${SLURM_NTASKS} (voir Script 2 dans les exemples ci-dessous).

Pour exécuter Dalton, chargez le module et utilisez le lanceur dalton.

dalton -b ${BASLIB} -N ${SLURM_NTASKS}  -dal dft_rspexci_nosym.dal  -mol H2O_cc-pVDZ_nosym.mol

ou

export DALTON_NUM_MPI_PROCS=${SLURM_NTASKS}
dalton -b ${BASLIB}  -dal dft_rspexci_nosym.dal  -mol H2O_cc-pVDZ_nosym.mol

= Exemples ː scripts et fichiers d’entrée =

== Exemple 1 : dft_rspexci_nosym ==

== Exemple 2 : dft_rspexci_sym.dal ==