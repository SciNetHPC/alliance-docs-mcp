---
title: "AMBER/fr"
url: "https://docs.alliancecan.ca/wiki/AMBER/fr"
category: "General"
last_modified: "2025-09-09T21:49:01Z"
page_id: 4218
display_title: "AMBER"
language: "fr"
---

==Introduction==
Amber désigne un ensemble d'applications pour effectuer des simulations de dynamique moléculaire,  particulièrement avec les biomolécules. Chacune des applications porte un nom différent, mais l'ensemble fonctionne plutôt bien et constitue un outil puissant pour effectuer plusieurs calculs usuels.

== Modules Amber ==
Nous fournissons les modules pour Amber, AmberTools et Amber-PMEMD dans notre pile logicielle.

* Amber (module amber) : comprend tout ce qui se trouve dans AmberTools, plus le programme avancé pmemd pour les simulations de dynamique moléculaire haute performance (QUICK pour les calculs de DFT avec GPU et sander pour la dynamique moléculaire).
* Amber-PMEMD (module amber-pmemd, Amber 24+) : Moteur pmemd haute performance optimisé pour CPU et GPU.  Le moteur pmemd (optimized for CPU/GPU) est un module distinct depuis Amber24 parce que pmemd n'est plus compilé avec AmberTools.  Remarque : Le module amber-pmemd n'inclut pas AmberTools. Pour utiliser les deux applications, chargez les deux modules.
* Le module ambertools pour AmberTools offre des outils pour préparer et analyser les simulations. L'application sander est utilisée pour les simulations de dynamique moléculaire. Tous ces outils sont gratuits et open source.

Pour la liste des versions installées et de leurs modules dépendants, lancez la sous-commande module spider ou consultez la page Logiciels disponibles.

== Utiliser AMBER sur les grappes de GPU H100 ==

ATTENTION : Les anciens modules AMBER ne prennent pas en charge les GPU H100 de NVIDIA. Utilisez plutôt les modules listés dans le tableau ci-dessous.

=== Modules requis ===

ambertools/25.0 ou amber-pmemd/24.3

Ces modules offrent des noyaux (kernels) CUDA pour les H-100 (compilés avec CUDA 12+ pour l'architecture Hopper).

Important : Pour les tâches avec GPU, n'utilisez pas les anciens modules AMBER; ils ne fonctionnent pas sur les nœuds H100.

== Charger des modules ==

Version         	avec CPU                                                     	avec GPU (CUDA)                                              	Notes
amber-pmemd/24.3	StdEnv/2023 gcc/12.3 openmpi/4.1.5 cuda/12.6 amber-pmemd/24.3	StdEnv/2023 gcc/12.3 openmpi/4.1.5 cuda/12.6 amber-pmemd/24.3	H100 pris en charge
amber/22.5-23.5 	StdEnv/2023 gcc/12.3 openmpi/4.1.5 amber/22.5-23.5           	StdEnv/2023 gcc/12.3 openmpi/4.1.5 cuda/12.2 amber/22.5-23.5
ambertools/25.0 	StdEnv/2023 gcc/12.3 openmpi/4.1.5 ambertools/25.0           	StdEnv/2023 gcc/12.3 openmpi/4.1.5 cuda/12.6 ambertools/25.0 	H100 pris en charge, avec PLUMED/2.9.0

Version          	avec CPU                                                     	avec GPU (CUDA)                                                         	Notes
ambertools/21    	StdEnv/2020 gcc/9.3.0 openmpi/4.0.3 scipy-stack ambertools/21	StdEnv/2020  gcc/9.3.0 cuda/11.4 openmpi/4.0.3 scipy-stack ambertools/21	GCC, FlexiBLAS & FFTW
amber/20.12-20.15	StdEnv/2020 gcc/9.3.0 openmpi/4.0.3 amber/20.12-20.15        	StdEnv/2020  gcc/9.3.0 cuda/11.4 openmpi/4.0.3 amber/20.12-20.15        	GCC, FlexiBLAS & FFTW
amber/20.9-20.15 	StdEnv/2020 gcc/9.3.0 openmpi/4.0.3 amber/20.9-20.15         	StdEnv/2020  gcc/9.3.0 cuda/11.0 openmpi/4.0.3 amber/20.9-20.15         	GCC, MKL & FFTW
amber/18.14-18.17	StdEnv/2020 gcc/9.3.0 openmpi/4.0.3 amber/18.14-18.17        	StdEnv/2020  gcc/8.4.0  cuda/10.2  openmpi/4.0.3                        	GCC, MKL

Version          	avec CPU                                                               	avec GPU (CUDA)                                                                      	Notes
amber/18         	StdEnv/2016 gcc/5.4.0 openmpi/2.1.1 scipy-stack/2019a amber/18         	StdEnv/2016 gcc/5.4.0 openmpi/2.1.1 cuda/9.0.176 scipy-stack/2019a amber/18          	GCC, MKL
amber/18.10-18.11	StdEnv/2016 gcc/5.4.0 openmpi/2.1.1 scipy-stack/2019a amber/18.10-18.11	StdEnv/2016 gcc/5.4.0 openmpi/2.1.1 cuda/9.0.176 scipy-stack/2019a amber/18.10-18.11 	GCC, MKL
amber/18.10-18.11	StdEnv/2016 gcc/7.3.0 openmpi/3.1.2 scipy-stack/2019a amber/18.10-18.11	StdEnv/2016 gcc/7.3.0  cuda/9.2.148 openmpi/3.1.2 scipy-stack/2019a amber/18.10-18.11	GCC, MKL
amber/16         	StdEnv/2016.4 amber/16                                                 	                                                                                     	Disponible uniquement sur Graham. Certaines fonctionnalités Python ne sont pas prises en charge.

==Utilisation==
===AmberTools 21===
Le module AmberTools 21 est présentement disponible sur toutes les grappes et offre sander, sander.LES, sander.LES.MPI, sander.MPI, sander.OMP, sander.quick.cuda, et sander.quick.cuda.MPI. Après avoir chargé le module, configurez les variables d'environnement avec

 source $EBROOTAMBERTOOLS/amber.sh

===Amber 20===
Amber20 est présentement disponible sur toutes les grappes. Il y a deux modules, soit 20.9-20.15 et 20.12-20.15.
* 20.9-20.15 utilise MKL et cuda/11.0; notez que les bibliothèques MKL ne fonctionnent pas bien avec des AMD et des CPU.
* 20.12-20.15 utilise FlexiBLAS et cuda/11.4; FlexiBLAS détecte le type de CPU et utilise des bibliothèques optimisées pour le matériel. De plus, CUDA/11.4 est requis pour effectuer des simulations sur les GPU A100 (installés sur Narval).

Les modules pour utilisation avec CPU offrent les applications disponibles avec AmberTools/20 plus pmemd (séquentiel) et pmemd.MPI (parallèle). Les modules pour utilisation avec ajoutent pmemd.cuda (un seul GPU) et pmemd.cuda.MPI (plusieurs GPU).

=== Problèmes connus ===
1. Le module amber/20.12-20.15 n'offre pas l'exécutable MMPBSA.py.MPI.

2. MMPBSA.py des modules amber/18-10-18.11 et amber/18.14-18.17 ne peut pas effectuer les calculs PB; utilisez plutôt les modules amber/20 plus récents.

==Exemples de soumission de tâches==
=== Avec un seul GPU ===
Pour les simulations avec un GPU sur Narval, utilisez amber/20.12-20.15. Les modules compilés avec une version CUDA < 11.4 ne fonctionnent pas sur un GPU A100. Voici un exemple de script pour une tâche de calcul utilisant un seul GPU.

=== Tâche MPI parallèle avec CPU ===

=== Tâche QM/MM distribuée avec plusieurs GPU ===
Dans l'exemple suivant, huit GPU sont demandés.

=== Tâche MMPBSA parallèle ===
Dans l'exemple suivant, 32 processus MPI sont utilisés. La scalabilité de MMPBSA se fait de façon linéaire parce que chaque séquence de la trajectoire est traitée indépendamment.

Pour les détails sur comment modifier vos scripts pour faire des simulations sur des ressources de calcul, voir Exécuter des tâches.

==Performance et étalonnage benchmarking==

Le guide Molecular Dynamics Performance Guide a été créé par une équipe d'ACENET. Le guide décrit les conditions optimales pour exécuter aussi des tâches sur nos grappes avec GROMACS, NAMD et OpenMM.

Étalonnage de simulations avec PMEMD

Étalonnage de simulations QM/MM avec SANDER.QUICK .