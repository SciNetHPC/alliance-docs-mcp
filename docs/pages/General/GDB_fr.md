---
title: "GDB/fr"
url: "https://docs.alliancecan.ca/wiki/GDB/fr"
category: "General"
last_modified: "2024-07-15T19:40:39Z"
page_id: 9589
display_title: "GDB"
language: "fr"
---

GDB (pour GNU Project Debugger) est un débogueur pour investiguer des problèmes présents dans les logiciels.

== Description ==
Avec un débogueur, il est possible de trouver rapidement la cause d'un problème dans un logiciel.
Le cas type d'utilisation est pour trouver les erreurs de segmentation.
Si vous désirez trouver un problème de mémoire (par exemple, une
fuite de mémoire), il est conseillé d'utiliser Valgrind.

== Cas d'utilisation ==
=== Trouver la cause d'une erreur de segmentation directement avec le débogueur ===
Dans cette section, le programme suivant est utilisé :

Ce programme génère une erreur de segmentation lorsqu'on l'exécute.

On peut alors l'exécuter à l'intérieur du débogueur. Notez qu'on a compilé avec l'option -g pour permettre au débogueur de décoder les symboles et fournir davantage d'information sur la source du bogue. On exécute l'application à l'intérieur du débogueur comme suit :
2, argv0x7fffffffda88) at program.cpp:15
15		cout << numbers[1000000] << endl;
Missing separate debuginfos, use: debuginfo-install glibc-2.16-31.fc18.x86_64 libgcc-4.7.2-8.fc18.x86_64 libstdc++-4.7.2-8.fc18.x86_64

(gdb) bt
#0  0x0000000000400c17 in main (argc2, argv0x7fffffffda88) at program.cpp:15
|
}}
Ici, l'erreur est causée par la ligne 15. Le code accède à l'indice 1000000, mais le tableau contient seulement 1000 éléments.

=== Trouver la cause d'une erreur de segmentation avec un fichier core ===
Dans cet exemple, on utilise le programme de la section précédente, mais sans utiliser le débogueur directement. Ceci est utile pour un bogue qui se produit longtemps après le début de l'exécution du programme.

Afin de trouver la cause de l'erreur de segmentation, il faut générer un fichier core. Pour ce faire, il faut activer la création de tels fichiers.

En exécutant à nouveau le même programme, un fichier core sera écrit.

En utilisant l'exécutable programme et le fichier core, il est possible de tracer l'exécution jusqu'à l'erreur.
1, argv0x7fff2315c848) at
program.cpp:15
15              cout << numbers[1000000] << endl;
Missing separate debuginfos, use: debuginfo-install
glibc-2.16-31.fc18.x86_64 libgcc-4.7.2-8.fc18.x86_64
libstdc++-4.7.2-8.fc18.x86_64

(gdb) bt
#0  0x0000000000400c17 in main (argc1, argv0x7fff2315c848) at
program.cpp:15
|
}}
On obtient alors le même résultat qu'en ayant exécuté le code à l'intérieur du débogueur.

=== Attacher le débogueur à un processus existant ===
Il est possible de déboguer un processus existant, par exemple une tâche qui s'exécute sur un nœud de calcul. Pour ce faire, il faut d'abord trouver le numéro du processus.

 grep firefox  grep -v grep
seb      12691  6.4  7.5 1539672 282656 ?      Sl   08:53   6:48 /usr/lib64/firefox/firefox http://www.google.ca/
}}

Ensuite, il est possible d'attacher le débogueur directement.

Après avoir fait cette commande, beaucoup d'information sera imprimée.

Plusieurs commandes de débogage sont disponibles. L'une des commandes utiles est backtrace, ou bt.
Cette commande permet d'afficher la pile d'appels en cours.

== Utilisation plus avancée ==
Dans les sections précédentes, nous avons utilisé les commandes run et backtrace. Plusieurs autres commandes sont disponibles pour déboguer en mode interactif, soit en contrôlant l'exécution du programme. Il est par exemple possible de fixer des points d'arrêt sur des fonctions ou des lignes de code ou encore lors d'une modification d'une variable. Lorsque l'exécution est interrompue, il est possible d'analyser l'état du programme en affichant la valeur de certaines variables. Le tableau ci-dessous contient une liste des principales commandes.

Principales commandes de GDB

Commande         	Raccourci	Argument                         	Description
run/kill         	r/k      	-                                	débute/arrête l'exécution du programme
where / backtrace	bt       	-                                	affiche la pile d'appel
break            	b        	src.c:numero_de_ligne ou fonction	crée un point d'arrêt à la ligne de code ou à la fonction spécifiée
watch            	-        	nom de variable                  	arrête l'exécution lorsque la variable est modifiée
continue         	c        	-                                	continue l'exécution après un point d'arrêt
step             	s        	-                                	exécute l'opération suivante
print            	p        	nom de variable                  	affiche le contenu d'une variable
list             	l        	src.c:numéro                     	affiche la ligne de code spécifiée

=== Afficher les structures de la STL ===
Par défaut, GDB n'affiche pas très bien le contenu des structures de la librairie standard du C++ (STL). Plusieurs solutions sont documentées ici. La solution la plus simple est probablement celle-ci, qui consiste à copier ce fichier dans votre répertoire d'accueil, sous le nom ~/.gdbinit.

== Autres ressources ==
* Site web GDB
* Tutoriel GDB
* Document du TACC sur le débogage et le profilage