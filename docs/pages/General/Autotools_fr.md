---
title: "Autotools/fr"
url: "https://docs.alliancecan.ca/wiki/Autotools/fr"
category: "General"
last_modified: "2019-02-12T18:17:12Z"
page_id: 2238
display_title: "Autotools"
language: "fr"
---

== Description ==
La suite Autotools (aussi appelée GNU build system) comprend l'outil autoconf. Cet outil automatise la création des fichiers Makefile de l'utilitaire Make pour différents systèmes et (peut-être) différents compilateurs.

Lorsqu'un programme est bâti à l'aide de autoconf, la première étape est d'appeler le script configure ainsi :

Autoconf vérifie que les versions des compilateurs et logiciels nécessaires sont installés sur l'ordinateur et génère le Makefile approprié.

On appelle ensuite Make de manière habituelle.

Les fichiers sont installés aux bons endroits par make install.
Pour vous réserver l'accès exclusif au logiciel, vous devez normalement spécifier où votre logiciel sera installé, ce qui se fait (habituellement) ainsi :

Dans certains cas, il faut utiliser l'option  --prefix plutôt que make; référez-vous à la documentation du logiciel que vous voulez installer.
Pour indiquer au système les chemins vers notre nouveau logiciel, il faut créer un module.

Une compilation de base d'un programme utilisant Autoconf peut être aussi simple que
$HOME/LOGICIEL}}

==Options pour les scripts configure==
Il existe de nombreuses options dont l'utilisation varie selon le projet. Pour obtenir la liste détaillée, lancez

Nous présentons ici les options les plus communes.

===Répertoire d'installation ===
Une option toujours disponible est --prefix. Celle-ci permet de définir dans quel répertoire make install installera l'application ou la bibliothèque. Par exemple, pour installer une application dans le sous-répertoire programmes de votre répertoire personnel, vous pouvez utiliser $HOME/programmes/}}

===Options de fonctionnalités===
La plupart des scripts de configuration permettent d'activer ou de désactiver certaines fonctionnalités du programme ou de la bibliothèque à compiler; elle sont généralement de type --enable-feature ou --disable-feature. En calcul informatique de pointe, il est fréquent que ces options incluent la parallélisation via fils d'exécution ou via MPI, par exemple

ou encore

Il est aussi fréquent d'avoir des options --with-...  pour paramétrer spécifiquement les fonctionnalités.
On recommande généralement de ne pas utiliser ces options et de laisser Autoconf trouver les paramètres automatiquement. Cependant, il est parfois nécessaire de spécifier des paramètres via les options --with-..., par exemple
$MPIDIR}}

===Options définies par des variables===
Vous pouvez généralement spécifier le compilateur à utiliser et les options qui doivent lui être passées en déclarant des variables après la commande ./configure. Par exemple, pour définir le compilateur C et les options à lui passer, vous pourriez lancer
icc CFLAGS"-O3 -xHost"}}
Voici une description des options les plus communes :

Option  	Description
CFLAGS  	pour le compilateur C
CPPFLAGS	pour le préprocesseur et les compilateurs C, C++, Objective C et Objective C++
CXXFLAGS	pour le compilateur C++
DEFS    	pour définir une macro pour préprocesseur
FCFLAGS 	pour le compilateur Fortran
FFLAGS  	pour le compilateur Fortran 77
LDFLAGS 	pour l'éditeur de liens
LIBS    	pour les bibliothèques à lier

La liste exhaustive des variables et options types est disponible dans la  documentation Autoconf.