---
title: "CMake/fr"
url: "https://docs.alliancecan.ca/wiki/CMake/fr"
category: "General"
last_modified: "2023-06-19T19:15:23Z"
page_id: 2327
display_title: "CMake"
language: "fr"
---

== Description ==
CMake (pour cross-platform make) est un outil de compilation libre multiplateforme et multilangage. Alors que Autotools est l'outil traditionnel sous Linux (utilisé entre autres pour tous les projets GNU), plusieurs projets sont passés à CMake au cours des dernières années, et ce pour différentes raisons, entre autres KDE et MySQL. Ceux qui ont éprouvé certaines difficultés à construire leur propre projet avec Autotools trouveront probablement CMake beaucoup plus facile d'utilisation. Selon KDE, les principales raisons pour lesquelles ils sont passés de Autotools à CMake sont que la compilation est beaucoup plus rapide et que les fichiers de construction sont beaucoup plus faciles à écrire.

== Principe de base ==
CMake fonctionne de la même manière que Autotools et requiert l'exécution d'un script configure, suivi d'un build avec make. Cependant, plutôt qu'appeler ./configure, on appelle cmake directory. Par exemple, si on est dans le répertoire où l'on veut construire l'application, on exécute

Ainsi, pour configurer, construire et installer une application ou une bibliothèque, la façon la plus simple est avec

== Options utiles pour travailler avec les grappes ==
Nos grappes sont configurées de telle sorte qu'à la compilation d'un nouveau paquet logiciel, l'information est automatiquement ajoutée au binaire résultant afin qu'il puisse trouver les bibliothèques desquelles il dépend; le mécanisme utilisé est RUNPATH (ou RPATH). Certains paquets qui utilisent CMake font de même avec une fonctionnalité offerte par CMake. Des conflits sont parfois créés quand les deux sont utilisés en même temps; pour éviter ceci, ajouter l'option

* -DCMAKE_SKIP_INSTALL_RPATH=ON

en ligne de commande. Aussi, les bibliothèques de nos grappes sont installées dans des endroits non standards et il est difficile pour CMake de les trouver; il peut être utile d'ajouter sur la ligne de commande l'option

* -DCMAKE_SYSTEM_PREFIX_PATH=$EBROOTGENTOO

Parfois, même cela n'est pas suffisant et vous pourriez devoir ajouter des options plus spécifiques aux bibliothèques utilisées par votre paquet logiciel, par exemple :
* -DCURL_LIBRARY=$EBROOTGENTOO/lib/libcurl.so -DCURL_INCLUDE_DIR=$EBROOTGENTOO/include
* -DPYTHON_EXECUTABLE=$EBROOTPYTHON/bin/python
* -DPNG_PNG_INCLUDE_DIR=$EBROOTGENTOO/include -DPNG_LIBRARY=$EBROOTGENTOO/lib/libpng.so
* -DJPEG_INCLUDE_DIR=$EBROOTGENTOO/include -DJPEG_LIBRARY=$EBROOTGENTOO/lib/libjpeg.so
* -DOPENGL_INCLUDE_DIR=$EBROOTGENTOO/include -DOPENGL_gl_LIBRARY=$EBROOTGENTOO/lib/libGL.so -DOPENGL_glu_LIBRARY=$EBROOTGENTOO/lib/libGLU.so
* -DZLIB_ROOT=$EBROOTGENTOO

== Personnalisation de la configuration ==
Tout comme avec Autotools, il est possible de personnaliser la configuration de l'application ou de la bibliothèque. Cela peut se faire par différentes options de la ligne de commande, mais aussi via une interface texte avec la commande ccmake.

=== Commande ccmake ===
La commande ccmake est appelée de la même façon que la commande cmake, en indiquant le répertoire à construire. S'il s'agit du répertoire courant, la commande est

Il faut appeler ccmake après avoir appelé cmake : en général, la commande est

ccmake affiche d'abord la liste des options définies par le projet. Le résultat est une liste relativement courte semblable à ceci :

Comme indiqué au bas de cette liste, vous pouvez éditer une valeur en appuyant sur la touche Enter. Si vous modifiez une valeur, appuyez sur la touche c pour tester la configuration avec cette nouvelle valeur. Si la configuration réussit, vous aurez alors l'option g, pour générer le Makefile avec la nouvelle configuration, ou vous pouvez quitter avec la touche q. Le mode avancé est activé avec la touche t, ce qui produit une liste beaucoup plus longue de variables qui permettra de configurer l'application avec précision. Voici un exemple de liste d'options :

Remarquez que ccmake en mode avancé affiche aussi bien les bibliothèques trouvées que celles qui n'ont pas été trouvées. Si vous voulez utiliser une certaine version de BLAS par exemple, vous saurez immédiatement si c'est celle que CMake a trouvée et, le cas échéant, pourrez la modifier. ccmake affiche aussi la liste des options passées aux compilateurs et à l’éditeur de liens, et ce, en fonction du type de construction.

===Options en ligne de commande===
Les options affichées par ccmake peuvent toutes être modifiées en ligne de commande, avec la syntaxe
VALEUR}}

Par exemple, pour spécifier l'emplacement d'installation :
/home/user/mon_repertoire}}

Pour configurer la compilation, vous voudrez possiblement changer les valeurs suivantes :

Option                   	Description
CMAKE_C_COMPILER         	change le compilateur C
CMAKE_CXX_COMPILER       	change le compilateur C++
CMAKE_LINKER             	change l'éditeur de liens
CMAKE_C_FLAGS            	change les options passées au compilateur C
CMAKE_CXX_FLAGS          	change les options passées au compilateur C++
CMAKE_SHARED_LINKER_FLAGS	change les options passées à l'éditeur de liens

La liste complète des options est disponible sur la page officielle de CMake.

Si vous ne voulez pas vous aventurer dans ces options spécifiques, CMake propose une option plus simple avec CMAKE_BUILD_TYPE, qui définit le type de compilation à utiliser. Les valeurs possibles sont

Option        	Description
-             	aucune valeur
Debug         	active les options de débogage, désactive les options d'optimisation
Release       	désactive les options de débogage, active les optimisations typiques
MinSizeRel    	désactive les options de débogage, active les options d'optimisation en minimisant la taille du binaire
RelWithDebInfo	active les options de débogage et les optimisations typiques

Ces différents types de compilation définissent des options de compilateurs qui varient selon le compilateur utilisé; vous n'avez donc pas à vérifier quelles options doivent être utilisées.

== Références ==

* Guide d'initiation en français qui couvre aussi bien la création de fichiers CMake que la compilation d'un projet déjà fait.
* Exemple simple (en anglais) sur le site officiel.
* Tutoriel (en anglais) plutôt complet sur le site officiel.
* Tutoriel assez complet en français.