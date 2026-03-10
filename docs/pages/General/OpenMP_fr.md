---
title: "OpenMP/fr"
url: "https://docs.alliancecan.ca/wiki/OpenMP/fr"
category: "General"
last_modified: "2024-07-15T20:20:02Z"
page_id: 1833
display_title: "OpenMP"
language: "fr"
---

== Description ==
OpenMP (pour Open Multi-Processing) est une interface de programmation (API) pour le calcul parallèle sur architecture à mémoire partagée. L'interface OpenMP est supportée sur de nombreuses plateformes dont Unix et Windows, pour les langages de programmation C/C++ et Fortran. Elle est composée d'un ensemble de directives, d'une bibliothèque logicielle et de variables d'environnement.

OpenMP permet de développer rapidement des applications parallèles à fine granularité en restant proche du code séquentiel. Avec une seule instance du programme, plusieurs sous-tâches peuvent être exécutées en parallèle. Les directives insérées dans le programme déterminent si une section du programme s'exécute en parallèle; dans ce cas, les directives prennent aussi en charge la distribution du travail sur plusieurs fils. Ainsi, un compilateur qui ne comprend pas les directives peut tout de même compiler le programme, qui peut ensuite être exécuté en série.

L'interface OpenMP est basée sur le concept de fils d'exécution (threads), bien connu en programmation orientée objet. Un fil d'exécution est un peu comme un processeur virtuel opérant en séquentiel; il s'agit de la plus petite unité de travail/calcul que peut programmer un système d'exploitation. Du point de vue du programmeur, cinq fils équivalent virtuellement à cinq processeurs qui peuvent effectuer du calcul en parallèle. Il est important de comprendre que le nombre de fils n'est pas associé au nombre de processeurs physiques disponibles : par exemple, deux processeurs peuvent exécuter un programme possédant 10 fils. C'est le système d'exploitation qui se charge de partager le temps des processeurs disponibles entre les fils.

Il n'est cependant pas possible d'exécuter le même fil sur plusieurs processeurs;  si vous disposez par exemple de quatre processeurs, vous devrez utiliser au minimum quatre fils pour profiter de toute la puissance de calcul. Dans certains cas, il pourrait être avantageux d'utiliser plus de fils que de processeurs; cependant, le nombre de fils est habituellement égal au nombre de processeurs.

Un autre point important concernant les fils est la synchronisation. Lorsque plusieurs fils d'un même programme effectuent des calculs en même temps, on ne peut absolument pas présumer de l'ordre dans lequel ils vont s'effectuer. Si un ordre déterminé est nécessaire pour assurer l'intégrité du code, le programmeur utilisera les directives de synchronisation d'OpenMP. La méthode exacte de distribution sur les fils demeure inconnue du programmeur, mais il existe toutefois des fonctionnalités de contrôle (voir processor affinity).

Pour paralléliser un programme avec OpenMP ou toute autre technique, il importe de considérer la capacité du programme à s'exécuter en parallèle, ce que nous appellerons sa scalabilité. Après avoir parallélisé votre locigiel et que sa qualité vous satisfait, nous vous recommandons d'effectuer une analyse de sa scalabilité pour en comprendre la performance.

Pour des renseignements sur l'utilisation d'OpenMP sous Linux, consultez ce tutoriel.

== Compilation ==
Pour la plupart des compilateurs, la compilation d'un code OpenMP s'effectue simplement en ajoutant une option de compilation. Pour les compilateurs GNU (GCC), il s'agit de l'option -fopenmp; pour ceux d'Intel, dépendant de la version, ce peut être -qopenmp, -fopenmp ou -openmp. Pour les autres compilateurs, vérifiez leur documentation respective.

== Directives ==

Les directives OpenMP sont insérées dans les programmes Fortran en utilisant des sentinelles. Une sentinelle est un mot clé placé immédiatement après le symbole indiquant un commentaire. Par exemple :

!$OMP directive
c$OMP directive
C$OMP directive
*$OMP directive

En C, les directives sont insérées en utilisant un pragma :

#pragma omp directive

===Directives OpenMP===

Fortran                                   	C, C++
!$OMP PARALLEL [clause, clause,…]
block  	$OMP END PARALLEL                 	#pragma omp parallel [clause, clause,…]
structured-block
!$OMP DO [ clause, clause,… ]
do_loop     	$OMP END DO                       	#pragma omp for [ clause, clause,… ]
for-loop
!$OMP SECTIONS [clause, clause,…]         	$OMP SECTION
block                	$OMP SECTION
block	$OMP END SECTIONS [NOWAIT]	#pragma omp sections [clause, clause,…] {
[ #pragma omp section ]
structured-block
[ #pragma omp section ]
structured-block
}
!$OMP SINGLE [clause, clause,…]
block     	$OMP END SINGLE [NOWAIT]          	#pragma omp single [clause, clause,…]
structured-block
!$OMP PARALLEL DO [clause, clause,…]
DO_LOOP
[ !$OMP END PARALLEL DO ]	#pragma omp parallel for [clause, clause,…]
for-loop
!$OMP PARALLEL SECTIONS [clause, clause,…]	$OMP SECTION
block                	$OMP SECTION
block	$OMP END PARALLEL SECTIONS	#pragma omp parallel sections [clause, clause,…] {
[ #pragma omp section ]
structured-block
[ #pragma omp section ]
structured-block
}
!$OMP MASTER
block                        	$OMP END MASTER                   	#pragma omp master
structured-block
!$OMP CRITICAL [(name)]
block             	$OMP END CRITICAL [(name)]        	#pragma omp critical [(name)]
structured-block
!$OMP BARRIER                             	#pragma omp barrier
!$OMP ATOMIC
expresion_statement          	#pragma omp atomic
expression-statement
!$OMP FLUSH [(list)]                      	#pragma omp flush [(list)]
!$OMP ORDERED
block                       	$OMP END ORDERED                  	#pragma omp ordered
structured-block
!$OMP THREADPRIVATE( /cb/[, /cb/]…)       	#pragma omp threadprivate ( list )
Clauses                                   	Clauses
PRIVATE ( list )                          	private ( list )
SHARED ( list )                           	shared ( list )
SHARED | NONE )                           	none )
FIRSTPRIVATE ( list )                     	firstprivate ( list )
LASTPRIVATE ( list )                      	lastprivate ( list )
intrinsic } : list )                      	reduction ( op : list )
IF ( scalar_logical_expression )          	if ( scalar-expression )
COPYIN ( list )                           	copyin ( list )
NOWAIT                                    	nowait

== Environnement ==
Certaines variables d'environnement ont un effet sur l'exécution d'un programme OpenMP :

OMP_NUM_THREADS
OMP_SCHEDULE
OMP_DYNAMIC
OMP_STACKSIZE
OMP_NESTED

Les variables sont définies ou modifiées avec une commande Unix telle que
12}}

Dans la plupart des cas, vous voudrez spécifier avec OMP_NUM_THREADS le nombre de cœurs réservés par machine. Ceci pourrait cependant être différent pour une application hybride OpenMP/MPI.

Une autre variable importante est OMP_SCHEDULE. Celle-ci contrôle comment sont distribuées les boucles (et plus généralement les sections parallèles).  La valeur par défaut dépend du compilateur et peut être définie dans le code source. Les valeurs possibles sont static,n, dynamic,n, guided,n et auto où n indique le nombre d'itérations gérées par chaque fil d'exécution.
*Dans le cas de static, le nombre d'itérations est fixe et les itérations sont distribuées au début de la section parallèle.
*Dans le cas de dynamic, le nombre d'itérations est fixe, mais les itérations sont distribuées pendant l'exécution en fonction du temps requis par chaque fil pour exécuter ses itérations.
*Dans le cas de guided, n indique le nombre minimal d'itérations. Le nombre d'itérations est d'abord choisi « grand », puis diminue dynamiquement au fur et à mesure que le nombre restant d'itérations diminue.
*Pour le mode auto, le compilateur et la bibliothèque sont libres de faire des choix.

L'avantage des cas dynamic, guided et auto est qu'ils permettent en théorie de mieux balancer les fils d'exécution, puisqu'ils s'ajustent dynamiquement selon le temps requis par chaque fil. Par contre, l'inconvénient est que vous ne savez pas à l'avance quel processeur exécutera quel fil, et à quelle mémoire il doit accéder. Il est ainsi impossible avec ces types de planification de prévoir l'affinité entre la mémoire et le processeur exécutant le calcul. Ceci peut être particulièrement problématique dans une architecture NUMA.

La variable d'environnement OMP_STACKSIZE définit la taille de la pile pour chacun des fils créés à l'exécution de OpenMP.  Remarquez que le fil principal OpenMP (celui qui exécute la partie séquentielle du programme) obtient la taille de sa pile de l'interpréteur (shell) alors que OMP_STACKSIZE affecte chacun des fils additionnels créés à l'exécution. Si cette variable n'est pas définie, la valeur sera de 4Mo. Si votre code ne possède pas assez de mémoire pour la pile, il pourrait se terminer de façon anormale en raison d'une erreur de segmentation.

D'autres variables d'environnement sont aussi disponibles : certaines sont spécifiques à un compilateur alors que d'autres sont plus génériques. Consultez la liste des variables pour les  compilateurs Intel et pour les  compilateurs GNU.

Les variables d'environnement spécifiques au compilateur Intel débutent par KMP_ alors que celles spécifiques à Gnu débutent par GOMP_. Pour des performances optimales en accès mémoire, fixez les variables  OMP_PROC_BIND et les variables d'affinité KMP_AFFINITY pour Intel et GOMP_CPU_AFFINITY pour GNU. Ceci empêche les fils d'exécution OpenMP de se déplacer d'un processeur à l'autre, ce qui est particulièrement important dans une architecture NUMA.

== Exemple ==
Voici un exemple "hello world" qui montre l'usage d'OpenMP.

Le code C est compilé et exécuté comme suit :
 litai10:~$ gcc -O3 -fopenmp ompHello.c -o ompHello
 litai10:~$ export OMP_NUM_THREADS=4
 litai10:~$ ./ompHello
 Hello world from thread 0 out of 4
 Hello world from thread 2 out of 4
 Hello world from thread 1 out of 4
 Hello world from thread 3 out of 4

Le code Fortran 90 est compilé et exécuté comme suit :
 litai10:~$ gfortran -O3 -fopenmp ompHello.f90 -o fomphello
 litai10:~$ export OMP_NUM_THREADS=4
 litai10:~$ ./fomphello
 Hello world from thread           0 out of           4
 Hello world from thread           2 out of           4
 Hello world from thread           1 out of           4
 Hello world from thread           3 out of           4

Pour savoir comment soumettre une tâche OpenMP, consultez la section Tâche multifil ou tâche OpenMP de la page Exécuter des tâches.

== Références ==
*Lawrence Livermore National Laboratory : documentation OpenMP.

* OpenMP.org : spécifications, aide-mémoire pour les interfaces C/C++ et Fortran, exemples.