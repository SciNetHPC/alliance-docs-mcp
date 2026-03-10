---
title: "BLAS and LAPACK/fr"
url: "https://docs.alliancecan.ca/wiki/BLAS_and_LAPACK/fr"
category: "General"
last_modified: "2025-01-06T16:21:20Z"
page_id: 18717
display_title: "BLAS et LAPACK"
language: "fr"
---

BLAS (Basic Linear Algebra Subprogram et LAPACK (Linear Algebra PACK) sont deux des bibliothèques les plus utilisées en calcul de pointe pour la recherche. Elles servent aux opérations sur les vecteurs et les matrices qui sont fréquentes dans plusieurs algorithmes. Elles sont encore plus que des bibliothèques, car elles définissent une interface de programmation standard, soit un ensemble de définitions de fonctions qui peuvent être appelées pour faire certains calculs comme le produit scalaire de deux vecteurs de nombres double précision ou le produit de deux matrices hermitiennes de nombres complexes.

En plus de l'implémentation de référence de Netlib, il existe plusieurs autres implémentations de ces deux standards. La performance de ces implémentations peut varier de beaucoup selon le logiciel utilisé; par exemple, il est clairement établi que l'implémentation fournie par la bibliothèque Intel Math Kernel Library (Intel MKL) offre une meilleure performance avec les processeurs Intel dans la plupart des situations. Intel est cependant propriétaire de cette implémentation et dans certains cas il est préférable d'utiliser l'implémentation libre OpenBLAS ou encore BLIS, dont la performance est meilleure avec les processeurs AMD. Deux projets qui ne sont plus maintenus sont gotoblas et ATLAS BLAS.

Malheureusement, il faut habituellement recompiler un logiciel pour savoir quelle implémentation est la plus performante pour un code particulier et une configuration matérielle précise. Ceci est problématique quand on veut créer un environnement logiciel portable pouvant fonctionner sur plusieurs grappes. Pour y remédier, vous pouvez utiliser FlexiBLAS, une couche d'abstraction qui permet de changer l'implémentation de BLAS et de LAPACK au moment de l'exécution plutôt que pendant la compilation.

= Choisir une implémentation =
Depuis quelques années, nous recommandions d'utiliser Intel MKL comme implémentation de référence étant donné que nos grappes n'avaient que des processeurs Intel. Depuis la mise en service de Narval qui possède  des processeurs AMD, nous recommandons maintenant de compiler le code avec FlexiBLAS. La configuration de notre module FlexiBLAS fait en sorte que BLIS est utilisé en présence de processeurs AMD et Intel MKL en présence d'autres types de processeurs, ce qui offre habituellement la performance optimale.

= Compiler avec FlexiBLAS =
Puisque FlexiBLAS est relativement récent, ce ne sont pas tous les systèmes qui vont le reconnaître par défaut. Il est généralement possible de pallier à ceci en configurant les options d'édition pour utiliser -lflexiblas pour BLAS et pour LAPACK. Ces options sont habituellement dans votre Makefile, autrement vous pouvez les passer comme paramètres à   configure ou cmake. Les versions de CMake à partir de 3.19 peuvent trouver FlexiBLAS automatiquement; pour utiliser une de ces versions, chargez le module cmake/3.20.1 ou cmake/3.21.4.

= Changer l'implémentation de BLAS/LAPACK pour l'exécution =
L'avantage principal de FlexiBLAS est de pouvoir changer l'implémentation en arrière-plan pour l'exécution en configurant la variable d'environnement FLEXIBLAS. En date de janvier 2022, les implémentations disponibles sont netlib, blis, imkl et openblas, mais vous pouvez obtenir la liste complète avec la commande

Sur Narval, nous avons configuré FLEXIBLAS=blis pour utiliser BLIS par défaut. Sur les autres grappes, FLEXIBLAS n'est pas défini et Intel MKL est utilisé par défaut.

= Utiliser directement Intel MKL =
Même si nous recommandons d'utiliser FlexiBLAS, il est toujours possible d'utiliser directement Intel MKL. Avec un compilateur Intel (par exemple ifort, icc, icpc), la solution est de remplacer -lblas et -llapack dans les options du compilateur et de l'éditeur (linker) avec
* -mkl=sequential pour ne pas utiliser de fils internes, ou
* -mkl pour utiliser des fils internes.
Ceci fait en sorte que l'implémentation MKL de BLAS/LAPACK est utilisée. Voyez la documentation sur les options.

Avec les compilateurs autres que ceux d'Intel, par exemple la GCC (GNU Compiler Collection), vous devez lister explicitement les bibliothèques MKL requises au cours de l'étape d'édition. Intel fournit l'outil MKL Link Advisor pour vous aider à déterminer les options de compilation et d'édition.

MKL Link Advisor est aussi utile si vous obtenez des erreurs undefined reference avec les compilateurs Intel et -mkl.