---
title: "Linux introduction/fr"
url: "https://docs.alliancecan.ca/wiki/Linux_introduction/fr"
category: "General"
last_modified: "2024-10-24T14:49:52Z"
page_id: 178
display_title: "Introduction à Linux"
language: "fr"
---

Cet article s'adresse aux utilisateurs de systèmes Windows ou Mac ayant peu ou pas d'expérience dans un environnement UNIX. Il devrait vous donner les bases nécessaires pour accéder aux serveurs de calcul et être rapidement capable de les utiliser.

La connexion aux serveurs utilise le protocole SSH en mode texte. Vous ne disposez pas d'une interface graphique, mais d'une console.  Notez que les exécutables Windows ne fonctionnent pas sur nos serveurs sans l'utilisation d'un émulateur.

SHARCNET offre un tutoriel de formation autonome; cliquez sur Introduction to the Shell.

== Obtenir de l'aide ==
En général, les commandes sont documentées dans les manuels de référence disponibles sur les serveurs. Vous pouvez y accéder à partir du terminal avec

man utilise less (voir section Visualiser et éditer des fichiers); il faut appuyer sur q pour quitter le programme.

Par convention, les exécutables contiennent eux-mêmes de l'aide sur la façon dont ils doivent être utilisés.
De façon générale, vous obtenez l'information avec les arguments en ligne de commande -h, --help ou dans certains cas -help, par exemple

== S'orienter sur le système ==
Au moment de la connexion, vous serez dirigé dans le répertoire $HOME (terme UNIX pour dossier ou répertoire) de votre compte utilisateur.
Lors de la création d'un compte, le $HOME ne contient rien d'autre que des fichiers de configuration qui sont cachés, soit ceux qui sont préfixés par un point (.).

Dans un système Linux, il est fortement déconseillé de créer des fichiers ou répertoires dont les noms contiennent des espaces ou des caractères spéciaux; ces caractères spéciaux incluent les accents.

=== Lister le contenu d'un répertoire ===
Pour lister les fichiers d'un répertoire dans un terminal, on utilise la commande ls (list)

Pour inclure les fichiers cachés

Pour trier les résultats par date (du plus récent au plus ancien) plutôt qu'en ordre alphabétique

Pour obtenir des informations détaillées sur les fichiers (permissions d'accès, propriétaire, groupe, taille et date de dernière modification)

L'option -h donne la taille des fichiers dans un format aisément lisible par les humains.

Les options peuvent être combinées, par exemple

=== Naviguer dans le système de fichiers ===
Pour se déplacer dans le système de fichiers, on utilise la commande cd (change directory).

Ainsi, pour se déplacer dans mon_répertoire, entrez

Pour revenir au dossier parent, entrez

Enfin, pour revenir à la racine de votre compte utilisateur ($HOME) entrez

=== Créer et supprimer des répertoires ===
Pour créer un répertoire, on utilise la commande mkdir (make directory)

Pour supprimer un répertoire, on utilise la commande rmdir (remove directory)

La suppression d'un répertoire avec cette méthode ne fonctionnera que si celui-ci est vide.

=== Supprimer des fichiers ===
On efface des fichiers avec la commande rm (remove)

On peut effacer un répertoire récursivement

L'option (potentiellement dangereuse!) -f peut être utile pour passer outre les demandes de confirmation de suppression et poursuivre l'opération après une erreur.

=== Copier et renommer des fichiers ou répertoires ===
Pour copier un fichier on utilise la commande cp (copy)

Pour copier récursivement un répertoire

Pour renommer un fichier ou un dossier, on utilise la commande mv (move)

Cette commande permet aussi de déplacer un dossier. Remplacez alors fichier_source par dossier_source et fichier_destination par dossier_destination.

== Permissions associées aux fichiers ==
Un système UNIX comporte trois niveaux de permissions : lecture (r), écriture (w) et exécution (x). Pour un fichier, il faut que le fichier soit accessible en lecture pour être lu, en écriture pour qu'on puisse le modifier et en exécution pour l'exécuter (si c'est un exécutable ou un script). Pour un répertoire, il faut la permission de lecture pour en lister le contenu, la permission d'écriture pour en modifier le contenu (ajouter ou supprimer un fichier) et la permission d'exécution pour modifier le répertoire.

Les permissions s'appliquent à trois types d'utilisateurs : le propriétaire (u), le groupe (g) et toutes les autres personnes (o). Pour connaitre les permissions associées aux fichiers et sous-répertoires du répertoire courant, on utilise la commande

Les 10 caractères au début de chaque ligne indiquent les permissions.
Le premier caractère indique le type de fichier :
* - : un fichier normal
* d : un répertoire
* l : un lien symbolique

Ensuite, de gauche à droite, on trouve les permissions en lecture, en écriture et en exécution du propriétaire, du groupe et des autres utilisateurs. Voici quelques exemples :
* drwxrwxrwx : un répertoire accessible en lecture et en écriture par tous
* drwxr-xr-x : un répertoire qui peut être listé par tous, mais où seul le propriétaire peut ajouter ou supprimer des fichiers
* -rwxr-xr-x : un fichier exécutable par tous, mais qui ne peut être modifié que par son propriétaire
* -rw-r--r-- : un fichier lisible par tous, mais qui ne peut être modifié que par son propriétaire
* -rw-rw---- : un fichier qui peut être lu et modifié par son propriétaire ou par son groupe
* -rw------- : un fichier qui ne peut être lu ou modifié que par son propriétaire
* drwx--x--x : un répertoire qui ne peut être listé ou modifié que par son propriétaire, mais par lequel tous peuvent passer pour se rentre à un sous-répertoire plus profond
* drwx-wx-wx : un répertoire dans lequel tous peuvent écrire, mais dont seul le propriétaire peut lister le contenu

Il est important de noter que pour pouvoir lire ou écrire dans un répertoire, il est nécessaire d'avoir l'accès en exécution (x) dans tous les répertoires parents, jusqu'à la racine / du système de fichiers. Ainsi, si votre répertoire personnel a les permissions drwx------ et qu'il contient un sous-répertoire avec les permissions drwxr-xr-x, les autres utilisateurs ne pourront pas lire le contenu de ce sous-répertoire, car ils n'ont pas l'accès en exécution au répertoire parent.

La commande ls -la donne ensuite un nombre, suivi du nom du propriétaire du fichier, du nom du groupe du fichier, de la taille du fichier, de la date de sa dernière modification et de son nom.

La commande chmod permet de modifier les permissions associées à un fichier. La manière simple de l'utiliser est de spécifier quelles permissions on veut ajouter ou enlever à quel type d'utilisateur. Ainsi, on spécifie la liste des utilisateurs (u pour le propriétaire, g pour le groupe, o pour les autres utilisateurs, a pour les trois options), suivie d'un + pour ajouter une permission ou d'un - pour retirer une permission, et suivie de la liste des permissions à modifier (r pour lecture, w pour écriture, x pour exécution). Les permissions non spécifiées ne sont pas affectées.  Voici quelques exemples :

* Empêcher les membres du groupe et les autres utilisateurs de lire ou de modifier le fichier secret.txt
* Permettre à tous de lire le fichier public.txt
* Rendre le fichier script.sh exécutable
* Permettre aux membres du groupe de lire et d'écrire dans le répertoire partage
* Empêcher d'autres utilisateurs de lire le contenu de son répertoire personnel

== Visualiser et éditer des fichiers ==
=== Visualiser un fichier ===
Pour visualiser un fichier en lecture seule, on utilise la commande less

On utilise alors les flèches du clavier ou la molette de la souris pour se déplacer dans le document.
On peut rechercher un terme dans le document en entrant /terme_a_rechercher.
On quitte en appuyant sur q.

=== Comparer deux fichiers ===
La commande diff permet de voir les différences entre deux fichiers

L'option -y permet d'afficher les fichiers côte à côte.

=== Rechercher dans un fichier ===
La commande grep permet de recherche une expression donnée dans un fichier

ou plusieurs fichiers
.
Notez que, sous Linux, le caractère * permet de remplacer aucun, un, ou une série de caractères.
Le caractère ? remplace (exactement) un caractère.

Le texte recherché peut également contenir des variables. Par exemple, pour rechercher le texte No. 10 ou No. 11, etc. avec n'importe quel nombre entre 10 et 29, on peut utiliser la commande

Le texte recherché doit être sous la forme d'une expression régulière. Pour en savoir plus sur les expressions régulières, consultez .