---
title: "CPMD/fr"
url: "https://docs.alliancecan.ca/wiki/CPMD/fr"
category: "General"
last_modified: "2024-05-05T15:38:56Z"
page_id: 11484
display_title: "CPMD"
language: "fr"
---

CPMD est un programme de simulation ab initio en dynamique moléculaire basé sur la théorie de la fonctionnelle de la densité (DFT) pour les ondes planes/pseudo-potentiels.

= Limites de la licence  =

Par le passé, vous deviez d'abord vous enregistrer et attendre la confirmation de l'équipe de développement, mais maintenant l'enregistrement n'est plus nécessaire. Cependant, les modules qui sont installés sur nos grappes sont protégés par un groupe POSIX.

Pour pouvoir utiliser CPMD sur  nos grappes, écrivez au,  soutien technique pour que nous vous ajoutions au groupe POSIX.

= Module =

Pour charger le module, lancez

module load StdEnv/2020
module load intel/2020.1.217 openmpi/4.0.3 cpmd/4.3

= Installer CPMD localement =

La réponse des administrateurs de CPMD peut prendre quelques semaines et même quelques mois. Comme utilisateur enregistré, vous avez accès aux fichiers sources de CPMD; vous pouvez donc construire l'application dans votre répertoire /home avec notre environnement EasyBuild en utilisant la même recette que nous utilisons pour une installation centrale.

Pour CPMD 4.3 dans votre compte sur une de nos grappes, suivez les directives suivantes :

Créez d'abord un répertoire local.
 $ mkdir -p ~/.local/easybuild/sources/c/CPMD

Placez  les tarballs et les rustines (patches) dans ce répertoire.

$ ls -al ~/.local/easybuild/sources/c/CPMD
cpmd2cube.tar.gz
cpmd2xyz-scripts.tar.gz
cpmd-v4.3.tar.gz
fourier.tar.gz
patch.to.4612
patch.to.4615
patch.to.4616
patch.to.4621
patch.to.4624
patch.to.4627

Lancez ensuite la commande EasyBuild.
 $ eb CPMD-4.3-iomkl-2020a.eb --rebuild

L'option --rebuild fait en sorte que  EasyBuild utilise l'installation située dans votre répertoire /home plutôt que celle de l'endroit central.

Une fois l'application installée, déconnectez-vous de la grappe et reconnectez-vous à nouveau

La commande module load cpmd trouvera l'application dans votre répertoire /home.

$ module load StdEnv/2020
$ module load intel/2020.1.217 openmpi/4.0.3 cpmd/4.3
$ which cpmd.x
~/.local/easybuild/software/2020/avx2/MPI/intel2020/openmpi4/cpmd/4.3/bin/cpmd.x

Vous pouvez maintenant l'utiliser dans un script de soumission de tâche.

=Exemples de script =

Pour faire exécuter une tâche, vous devez configurer un fichier d'entrée et l'accès aux pseudo-potentiels.

Si le fichier d'entrée et les pseudo-potentiels sont dans le même répertoire, la commande suivante fait exécuter le programme en parallèle :

srun cpmd.x  >  (comme dans le script 1)

Si les pseudo-potentiels sont dans un répertoire différent, la commande est

srun cpmd.x   >  (comme dans le script 2)

=Référence=

* site web