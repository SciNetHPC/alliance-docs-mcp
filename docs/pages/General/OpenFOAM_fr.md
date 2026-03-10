---
title: "OpenFOAM/fr"
url: "https://docs.alliancecan.ca/wiki/OpenFOAM/fr"
category: "General"
last_modified: "2026-01-11T11:46:01Z"
page_id: 6839
display_title: "OpenFOAM"
language: "fr"
---

OpenFOAM (pour Open Field Operation and Manipulation) est un paquet logiciel open source gratuit pour la modélisation numérique de la dynamique des fluides. Ses nombreuses fonctions touchent autant l'électromagnétisme et la dynamique des solides que les flux liquides complexes avec réaction chimique, turbulence et transfert thermique.

===Modules===
Pour une version récente, utilisez

La communauté OpenFOAM comprend :
* La OpenFOAM Foundation avec ses sites web openfoam.org et cfd.direct,
* OpenCFD avec son site web openfoam.com.
Les versions semblent identiques jusqu'à 2.3.1 (décembre 2014). Pour les versions après 2.3.1,
*les modules avec des noms commençant par la lettre v sont dérivés de la branche .com (par exemple openfoam/v1706);
*les modules avec des noms commençant par un chiffre sont dérivés de la branche .org (par exemple, openfoam/4.1).

Pour plus d'information sur les commandes, consultez  Utiliser des modules.

===Documentation===
*documentation OpenFOAM.com
*CFD Direct, Guide de l'utilisateur.

===Utilisation===
Votre environnement nécessite beaucoup de préparation. Pour pouvoir exécuter les commandes OpenFOAM  (paraFoam, blockMesh, etc.), vous devez charger un module.

Le script suivant est pour une tâche séquentielle avec OpenFOAM 5.0 ː

Le script suivant est pour une tâche parallèle ː

La préparation du maillage (blockMesh) peut être assez rapide pour se faire en ligne de commande (voir Exécuter des tâches). L'étape la plus exigeante est habituellement celle du solveur (entre autres icoFoam); ces tâches devraient toujours être soumises à l'ordonnanceur, sauf pour de très petits cas ou des tutoriels.

=== Solveur petscFoam ===
OpenFOAM peut être compilé avec le solveur externe petscFoam.
Nos modules OpenFOAM n'incluent pas ce solveur, mais la compilation peut se faire sur toutes nos grappes.

Les versions de OpenFOAM et de PETSc doivent être compatibles. Par exemple, les combinaisons suivantes sont valides :

* openfoam/v2412 et petsc/3.21.6
* openfoam/v2312 et petsc/3.20.0

==== Compatibilité des versions d'OpenFOAM et de PETSc ====
Pour vérifier quelle version mineure de PETSc est compatible avec une version particulière d'OpenFOAM, chargez le module OpenFOAM souhaité et exécutez la commande grep.

Nous savons ainsi que quand  openfoam/v2412 est sortie, les tests ont été faits avec PETSc 3.21.2.

Nous avons un module pour petsc/3.21.6 issue de la même branche 3.21 et qui ne devrait contenir que des corrections de bogues par rapport à la version 3.21.2.

==== Compiler le solveur petscFoam ====
Il faut maintenant télécharger et extraire le paquet external-solver-main.tar.gz.

L'exécution de ./Allmake compilera le solveur petscFoam.

==== Confirmer que le solveur petscFoam est fonctionnel ====
Quelques tests rapides peuvent confirmer ceci.

Vérifiez si OpenFOAM peut charger petscFoam.

Vérifiez si la bibliothèque dynamique se trouve dans $FOAM_USER_LIBBIN.

Vérifiez si libpetscFoam.so peut trouver ses dépendances.
 grep petsc
|result=
 libpetsc.so.3.21 > /cvmfs/soft.computecanada.ca/easybuild/software/2023/x86-64-v4/MPI/gcc12/openmpi4/petsc/3.21.6/lib/libpetsc.so.3.21 (0x00007f96fa800000)
 libstrumpack.so.7.2 > /cvmfs/soft.computecanada.ca/easybuild/software/2023/x86-64-v4/MPI/gcc12/openmpi4/petsc/3.21.6/lib/libstrumpack.so.7.2 (0x00007f96f8200000)
 libml.so.13 > /cvmfs/soft.computecanada.ca/easybuild/software/2023/x86-64-v4/MPI/gcc12/openmpi4/petsc/3.21.6/lib/libml.so.13 (0x00007f96fa281000)
}}

===Performance===
La fonction de débogage produit fréquemment des centaines d'opérations d'écriture par seconde, ce qui peut causer une baisse de performance des systèmes de fichiers partagés. Si vous êtes en production et que vous n'avez pas besoin de cette information, diminuez ou désactivez la fonction de débogage avec

Plusieurs autres paramètres peuvent diminuer la quantité et la fréquence des écritures sur disque; voir la documentation pour la version 6 et la version 7.

Par exemple, le dictionnaire debugSwitches dans $HOME/.OpenFOAM/$WM_PROJECT_VERSION/controlDict peut être modifié pour que les valeurs des indicateurs qui sont plus grandes que zéro soient égales à zéro. Une autre solution serait d'utiliser l'espace scratch local ($SLURM_TMPDIR) qui est un disque attaché directement au nœud de calcul; voir la section Disque local dans la page Travailler avec un grand nombre de fichiers.

==== Espace /scratch local sur un nœud de calcul ====

Si votre flux de travail crée plusieurs petits fichiers, il serait préférable d'utiliser $SLURM_TMPDIR comme répertoire de travail. Pour plus d'information, voir Stockage local sur les nœuds de calcul.