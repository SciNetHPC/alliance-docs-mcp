---
title: "Debugging and profiling/fr"
url: "https://docs.alliancecan.ca/wiki/Debugging_and_profiling/fr"
category: "General"
last_modified: "2023-06-05T17:43:55Z"
page_id: 11683
display_title: "Débogage et profilage"
language: "fr"
---

Une étape importante en développement logiciel, particulièrement en Fortran et C/C++, est l'utilisation d'un logiciel de débogage pour détecter et identifier l'origine des erreurs d'exécution (par exemple les fuites de mémoire, les exceptions de virgule flottante, etc.). Après avoir éliminé les erreurs, la prochaine étape est de profiler le programme avec un logiciel de profilage pour déterminer le pourcentage du temps d'exécution pour chacune des sections du code source avec un scénario de test représentatif. Un profileur peut fournir de l'information sur le nombre de fois qu'une fonction est appelée, quelles sont les fonctions qui l'appellent ou encore combien de millisecondes en moyenne coûte chaque appel.

=Outils=

Nos grappes offrent un choix de débogueurs et de profileurs pour effectuer le travail en mode graphique par connexion X11 ou en mode ligne de commande. Le débogage devrait être effectué dans une  tâche interactive et non dans un nœud de connexion.

== Débogueur GNU (gdb) ==

Voir GDB.

== Débogueur PGI (pgdb) ==
Voir PGDBG.

== Débogueur ARM (ddt) ==

Voir  ARM.

==Profileur GNU (gprof) ==

Voir  Gprof.

== Profileur Scalasca (scalasca, scorep, cube) ==

Scalasca est un ensemble d'outils open source avec une interface graphique pour le profilage parallèle avec GPU. Ces outils sont disponibles pour gcc 9.3.0 et OpenMPI 4.0.3, dans des architectures AVX2 et AVX512. Chargez l'environnement avec

module load StdEnv/2020 gcc/9.3.0 openmpi/4.0.3 scalasca

La version courante est 2.5. Vous trouverez plus d'information et des exemples de flux de travail dans Scalasca User Guide.

== Profileur PGI (pgprof) ==
Voir  Pgprof.

== Profileur Nvidia en ligne de commande (nvprof) ==
Voir  nvprof.

==Valgrind==

Voir  Valgrind.

= Autres références =

* Introduction to (Parallel) Performance, SciNet
* Code profiling on Graham (vidéo de 54 minutes), SHARCNET