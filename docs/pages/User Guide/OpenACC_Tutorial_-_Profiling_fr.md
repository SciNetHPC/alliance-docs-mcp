---
title: "OpenACC Tutorial - Profiling/fr"
url: "https://docs.alliancecan.ca/wiki/OpenACC_Tutorial_-_Profiling/fr"
category: "User Guide"
last_modified: "2023-05-09T18:16:31Z"
page_id: 3225
display_title: "Tutoriel OpenACC : Profileurs"
language: "fr"
---

== Profiler du code  ==
Pourquoi auriez-vous besoin de profiler du code? Parce que c'est la seule façon de comprendre
* comment le temps est employé aux points critiques (hotspots),
* comprendre la performance du code,
* savoir comment mieux employer votre temps de développement.

Pourquoi est-ce important de connaitre les points critiques dans le code?
D'après la loi d'Amdahl, paralléliser les routines qui exigent le plus de temps d'exécution (les points critiques) produit le plus d'impact.

== Préparer le code pour l'exercice ==
Pour l'exemple suivant, nous utilisons du code provenant de  dépôt de données Git.
Téléchargez et faites l'extraction du paquet et positionnez-vous dans le répertoire cpp ou f90. Le but de cet exemple est de compiler et lier le code pour obtenir un exécutable pour en profiler le code source avec un profileur.

Une fois l'exécutable cg.x créé, nous allons profiler son code source. Le profileur mesure les appels des fonctions en exécutant et en surveillant ce programme.
Important : Cet exécutable utilise environ 3Go de mémoire et un cœur CPU presque à 100 %. L'environnement de test devrait donc avoir 4Go de mémoire disponible et au moins deux (2) cœurs CPU.

=== Profileur en ligne de commande NVIDIA nvprof ===
Dans sa trousse de développement pour le calcul de haute performance, NVIDIA fournit habituellement nvprof, mais la version qu'il faut utiliser sur nos grappes est incluse dans un module CUDA.

Pour profiler un exécutable CPU pur, nous devons ajouter les arguments --cpu-profiling on à la ligne de commande.
 main
  7.94%  8.62146s  waxpby(double, vector const &, double, vector const &, vector const &)
  7.94%  8.62146s   main
  5.86%  6.36584s  dot(vector const &, vector const &)
  5.86%  6.36584s   main
  2.47%  2.67666s  allocate_3d_poisson_matrix(matrix&, int)
  2.47%  2.67666s   main
  0.13%  140.35ms  initialize_vector(vector&, double)
  0.13%  140.35ms   main
...
======== Data collected at 100Hz frequency
}}
Dans le résultat, la fonction matvec() utilise 83.5 % du temps d'exécution; son appel se trouve dans la fonction main().

==Renseignements sur le compilateur==
Avant de travailler sur la routine, nous devons comprendre ce que fait le compilateur; posons-nous les questions suivantes :
* Quelles sont les optimisations qui ont été automatiquement appliquées par le compilateur?
* Qu'est-ce qui a empêché d'optimiser davantage?
* La performance serait-elle affectée par les petites modifications?

Le compilateur NVIDIA offre l'indicateur -Minfo avec les options suivantes :
* all, pour imprimer presque tous les types d'information, incluant
** accel pour les opérations du compilateur en rapport avec l'accélérateur
** inline pour l'information sur les fonctions extraites et alignées
** loop,mp,par,stdpar,vect pour les renseignements sur l'optimisation et la vectorisation des boucles
* intensity, pour imprimer l'information sur l'intensité des boucles
* (aucune option) produit le même résultat que l'option  all, mais sans l'information fournie par inline.

== Obtenir les renseignements sur le compilateur  ==
* Modifiez le Makefile.
  CXX=nvc++
  CXXFLAGS=-fast -Minfo=all,intensity
  LDFLAGS=${CXXFLAGS}

* Effectuez un nouveau build.

== Interpréter le résultat  ==
L'intensité computationnelle d'une boucle représente la quantité de travail accompli par la boucle en fonction des opérations effectuées en mémoire, soit

\mbox{intensité computationnelle} = \frac{\mbox{opérations de calcul}}{\mbox{opérations en mémoire}}

Dans le résultat, une valeur supérieure à 1 pour Intensity indique que la boucle serait bien exécutée sur un processeur graphique (GPU).

== Comprendre le code  ==
Regardons attentivement la boucle principale  de
la fonction matvec() implémentée dans matrix_functions.h:

  for(int i=0;i