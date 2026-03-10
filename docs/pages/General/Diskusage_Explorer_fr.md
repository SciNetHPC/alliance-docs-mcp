---
title: "Diskusage Explorer/fr"
url: "https://docs.alliancecan.ca/wiki/Diskusage_Explorer/fr"
category: "General"
last_modified: "2025-12-19T15:08:53Z"
page_id: 16495
display_title: "Diskusage Explorer"
language: "fr"
---

==Contenu des répertoires==

Important : Pour l'instant, cet outil est seulement disponible sur Narval.

L'outil Diskusage Explorer vous permet d'obtenir le détail de l'utilisation de l'espace dans vos répertoires /home, /scratch et /project. Cette information est mise à jour quotidiennement et est triée selon un format SQLite pour un accès rapide.

Dans notre exemple, nous verrons la consommation de l'espace disque du répertoire def-professor dans /project.

=== Interface ncurse ===
Sélectionnez un espace /project auquel vous avez accès et que vous voulez analyser; dans notre exemple, nous analysons def-professor.

Cette commande charge un navigateur qui montre les ressources consommées par tous les fichiers dans l'arborescence d'un répertoire.

Entrez c pour alterner entre l'espace disque consommé et le nombre de fichiers, q ou  pour quitter et h pour l'aide.

Pour ne consulter qu'un sous-répertoire de cet espace /project sans avoir à naviguer dans toute l'arborescence, utilisez

La commande man duc affiche une page du manuel.

=== Interface graphique ===

Si le nœud de connexion est particulièrement occupé ou si vous avez un trop grand nombre de fichiers dans votre espace /project, l'affichage peut être lent et irrégulier. Pour de meilleurs résultats, voyez comment utiliser diskusage_explorer sur votre propre ordinateur.

Nous recommandons d'utiliser le mode texte ncurse standard sur nos nœuds de connexion, mais diskusage_explorer inclut aussi une belle interface graphique.

Assurez-vous d'abord que votre connexion SSH fait en sorte que l'affichage des applications d'interfaces se fait correctement. Vous pouvez alors utiliser une interface graphique avec la commande

Vous pouvez naviguer avec la souris et aussi utiliser c pour alterner entre la taille des fichiers et le nombre de fichiers.

=== Naviguer plus rapidement sur votre ordinateur ===

Installez d'abord le logiciel diskusage_explorer sur votre ordinateur local puis, toujours sur votre ordinateur local, téléchargez le fichier SQLite de votre grappe et lancez duc.

rsync -v --progress username@beluga.calculcanada.ca:/project/.duc_databases/def-professor.sqlite  .
duc gui -d ./def-professor.sqlite  /project/def-professor

Vous pourrez ainsi naviguer de manière plus agréable.

== Espace utilisé et nombre de fichiers par utilisateur sur Cedar ==

Sur Cedar, chaque membre d'un groupe peut obtenir les données sur l'utilisation de l'espace et la quantité de fichiers par utilisateur avec la commande  diskusage_report et les options --per_user et --all_users. Avec la première option, la commande fait afficher seulement les membres du groupe qui ont le plus de fichiers et/ou qui occupent le plus d'espace. En ajoutant la deuxième option, on obtient l'utilisation  par tous les membres. Cette commande permet d'identifier les utilisateurs qui pourraient mieux gérer leurs données.

Dans le prochain exemple, l'utilisateur user01 lance la commande et obtient le résultat suivant :

[user01@cedar1 ~]$ diskusage_report --per_user --all_users
                             Description                Space           # de fichiers
                     /home (user user01)             109k/50G              12/500k
                  /scratch (user user01)             4000/20T              1/1000k
                 /project (group user01)              0/2048k               0/1025
          /project (group def-professor)            9434G/10T            497k/500k

Breakdown for project def-professor (Last update: 2023-05-02 01:03:10)
           User      File count                 Size             Location
-------------------------------------------------------------------------
         user01           28313             4.00 GiB              On disk
         user02           11926             3.74 GiB              On disk
         user03           14507          6121.03 GiB              On disk
         user04            4010           377.86 GiB              On disk
         user05          125929           262.75 GiB              On disk
         user06          201099            60.51 GiB              On disk
         user07           84806          1721.33 GiB              On disk
         user08           26516           947.23 GiB              On disk
          Total          497106          9510.43 GiB              On disk

Breakdown for nearline def-professor (Last update: 2023-05-02 01:01:30)
           User      File count                 Size             Location
-------------------------------------------------------------------------
         user03               5          1197.90 GiB     On disk and tape
          Total               5          1197.90 GiB     On disk and tape

Ce groupe est composé de 8 utilisateurs et le résultat obtenu montre clairement que 4 d'entre eux ont une grande quantité de fichiers qui ne contiennent que peu de données.

           User      File count                 Size             Location
-------------------------------------------------------------------------
         user01           28313             4.00 GiB              On disk
         user02           11926             3.74 GiB              On disk
         user05          125929           262.75 GiB              On disk
         user06          201099            60.51 GiB              On disk