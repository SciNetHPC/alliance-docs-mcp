---
title: "Tuning Lustre/fr"
url: "https://docs.alliancecan.ca/wiki/Tuning_Lustre/fr"
category: "General"
last_modified: "2022-04-28T13:29:45Z"
page_id: 4255
display_title: "Lustre"
language: "fr"
---

=Système de fichiers Lustre=

Lustre est un système de fichiers distribué de haute performance qui vous permet de réaliser des opérations d'entrée-sortie en parallèle avec un fort débit. Il y a cependant quelques précautions à prendre si on veut obtenir un rendement maximal. Les conseils présentés ici s'adressent aux utilisateurs expérimentés et doivent être suivis avec prudence. Assurez-vous d'effectuer des tests pour vérifier la validité scientifique des résultats obtenus et pour faire en sorte que les modifications entraînent une réelle amélioration de la performance.

== Paramètres stripe_count et stripe_size ==

Pour chaque fichier ou répertoire, il est possible de modifier ces paramètres.
* stripe_count est le nombre de disques sur lesquels les données sont réparties;
* stripe_size est la taille du plus petit bloc de données alloué dans le système de fichiers.

Il est possible de connaître la valeur de ces paramètres pour un fichier ou un répertoire donné avec la commande

De même, il est possible de modifier ces paramètres pour un répertoire donné avec la commande

Par exemple, si count=8, le fichier sera réparti sur huit disques RAID et chaque Mo sera écrit séquentiellement sur jusqu'à 8 serveurs.

Le fait de changer ces paramètres ne modifiera pas un fichier existant;  pour les changer, il faut migrer le fichier ou le copier (et non le déplacer) vers un répertoire ayant des paramètres différents. Pour créer un fichier vide avec des valeurs particulières pour stripe_count et stripe_size sans modifier les paramètres du répertoire, vous pouvez exécuter lfs setstripe sur le nom du fichier que vous voulez créer : le fichier sera créé vide et aura les paramètres spécifiés.

Exemple d'un répertoire non segmenté avec le fichier example_file (lmm_stripe_count est égal à 1 et il n'y a qu'un seul objet)
 $ lfs getstripe striping_example/
 striping_example/
 stripe_count:  1 stripe_size:   1048576 pattern:       raid0 stripe_offset: -1
 striping_example//example_file
 lmm_stripe_count:  1
 lmm_stripe_size:   1048576
 lmm_pattern:       raid0
 lmm_layout_gen:    0
 lmm_stripe_offset: 2
 	obdidx		 objid		 objid		 group
 	     2	       3714477	     0x38adad	   0x300000400

Nous pouvons modifier la segmentation de ce répertoire pour utiliser 2 disques et créer un nouveau fichier.

 $ lfs setstripe -c 2 striping_example
 $ dd if=/dev/urandom of=striping_example/new_file bs=1M count=10
 $ lfs getstripe striping_example/
 striping_example/
 stripe_count:  2 stripe_size:   1048576 pattern:       raid0 stripe_offset: -1
 striping_example//example_file
 lmm_stripe_count:  1
 lmm_stripe_size:   1048576
 lmm_pattern:       raid0
 lmm_layout_gen:    0
 lmm_stripe_offset: 2
 	obdidx		 objid		 objid		 group
 	     2	       3714477	     0x38adad	   0x300000400
 striping_example//new_file
 lmm_stripe_count:  2
 lmm_stripe_size:   1048576
 lmm_pattern:       raid0
 lmm_layout_gen:    0
 lmm_stripe_offset: 3
 	obdidx		 objid		 objid		 group
 	     3	       3714601	     0x38ae29	   0x400000400
 	     0	       3714618	     0x38ae3a	   0x2c0000400

Seulement le fichier new_file utilise par défaut count=2 (lmm_stripe_count) et 2 objets sont alloués.

Pour resegmenter l'ancien fichier, on utilise lfs migrate
 $ lfs migrate -c 2 striping_example/example_file
 $ lfs getstripe striping_example/example_file
 striping_example/example_file
 lmm_stripe_count:  2
 lmm_stripe_size:   1048576
 lmm_pattern:       raid0
 lmm_layout_gen:    2
 lmm_stripe_offset: 10
 	obdidx		 objid		 objid		 group
 	    10	       3685344	     0x383be0	   0x500000400
 	    11	       3685328	     0x383bd0	   0x540000400

lmm_stripe_count est maintenant de 2 et deux objets sont alloués.

Augmenter le nombre de disques peut augmenter les performances, mais rend aussi le fichier plus vulnérable aux défaillances matérielles.

Lorsqu'un programme parallèle a besoin de lire un petit fichier (< 1Mo), par exemple un fichier de configuration, il est plus efficace de placer ce fichier sur un seul disque (stripe count=1), de le lire avec le processus maître (master rank), et ensuite de l'envoyer aux autres processus à l'aide de MPI_Broadcast ou MPI_Scatter.

Lorsque l'on manipule de gros fichiers de données, il est préférable d'utiliser autant de disques que le nombre de processus MPI. La taille sera habituellement la même que celle du tampon de données qui est lu ou écrit par chaque processus; par exemple, si chaque processus lit 1Mo de données à la fois, alors 1Mo sera probablement l'idéal.  Si vous n'avez pas de bonne raison de modifier cette taille, nous vous recommandons de la laisser à sa valeur par défaut, qui a été optimisée pour des fichiers de grande taille.  Notez que la taille doit toujours être un multiple entier de 1Mo.

De manière générale, il faut réduire au maximum les ouvertures et fermetures de fichiers.  Il sera donc préférable d’agglomérer toutes les données dans un seul fichier plutôt que d'écrire une multitude de petits fichiers. Il sera aussi grandement préférable d'ouvrir le fichier une seule fois au début de l'exécution et de le fermer à la fin, plutôt que de l'ouvrir et de le fermer à l'intérieur d'une même exécution chaque fois que l'on veut y ajouter de nouvelles données.

== Pour plus d'information==

*Archivage et compression de fichiers