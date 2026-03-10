---
title: "Windows Subsystem for Linux (WSL)/fr"
url: "https://docs.alliancecan.ca/wiki/Windows_Subsystem_for_Linux_(WSL)/fr"
category: "General"
last_modified: "2024-04-02T22:03:33Z"
page_id: 25222
display_title: "Travailler avec Windows Subsystem for Linux (WSL)"
language: "fr"
---

= Introduction =

Le sous-système Windows pour Linux (WSL) est une fonctionnalité du système d'exploitation Windows qui permet d'exécuter un environnement Linux sur un ordinateur Windows, sans nécessiter une application de machine virtuelle complète ou une autre méthode complexe telle que le double amorçage. L'utilisation de WSL vous permet d'accéder simultanément aux applications et fichiers Windows et Linux de manière intégrée et transparente.

Cette configuration est particulièrement intéressante si vous utilisez un ordinateur Windows et que vous avez besoin d'accéder aux ressources de l'Alliance qui sont basées sur Linux. Elle vous permet d'utiliser des outils basés sur Linux pour vous connecter et transférer des données vers ou à partir des ressources de l'Alliance, tout en ayant accès en même temps à votre environnement Windows habituel.

Cette page est une introduction rapide aux tâches de base pour lesquelles WSL peut être utile. Si une documentation plus détaillée est requise, consultez la documentation fournie par Microsoft à propos de WSL.

= Installation =

Voir Comment installer Linux sur Windows avec WSL.

Pour démarrer rapidement sur un ordinateur Windows 10/11 où WSL n'est pas encore installé, suivez les étapes ci-dessous qui installeront WSL et Ubuntu (une version populaire de Linux).
# Enregistrez votre travail, car ce processus nécessite un redémarrage.
# Cliquez sur le bouton Démarrer et commencez à écrire command prompt.
# Faites un clic droit sur l'application de l'invite de commande et sélectionnez Exécuter en tant qu'administrateur. Acceptez toute invite de sécurité qui apparaît.
# Dans la fenêtre de l'invite de commande, entrez la commande suivante et attendez qu'elle se termine :
# Redémarrez votre ordinateur.

= Premier lancement de Ubuntu =

Lorsque votre ordinateur aura terminé son redémarrage, une nouvelle application sera disponible dans le menu Démarrer, soit Ubuntu. Lors du premier lancement de cette application, WSL décompressera certains fichiers et préparera l'environnement Ubuntu Linux. Une fois ceci terminé, il vous sera demandé de configurer votre utilisateur Linux et de définir un mot de passe.

Prendre note :
* Ce nom d'utilisateur est unique au système Linux et ne doit pas nécessairement correspondre au nom de l'utilisateur Windows.
* Si vous installez ultérieurement plusieurs environnements Linux différents dans WSL, chacun d'entre eux aura ses propres utilisateurs et mots de passe (ils ne sont pas partagés).

# À l'invite Enter new UNIX username, entrez le nom d'utilisateur souhaité et appuyez sur Entrée.
# À l'invite Enter new UNIX password, entrez le mot de passe souhaité et appuyez sur Entrée. Il est normal de ne pas voir les caractères lorsque vous les entrez.

La configuration WSL/Ubuntu Linux est terminée et prête à être utilisée.

= Accès aux fichiers entre Windows et Linux =
Les environnements Linux fonctionnant sous WSL sont essentiellement équivalents à des machines virtuelles. Ils ne partagent pas en soi le même accès aux données stockées dans chaque environnement, mais le WSL s’est donné beaucoup de mal pour combler cet écart de deux manières :
# En montant (attachant) automatiquement vos lecteurs Windows dans la structure de dossiers Linux à /mnt/.
# En ajoutant une entrée Linux dans la barre latérale de l'explorateur Windows qui fournit un accès direct aux fichiers stockés sous Linux.

Ces intégrations vous permettent de transférer facilement des données entre les deux systèmes. À titre d'exemple, le lecteur Windows commun C:\ serait disponible sous Linux à l'adresse /mnt/c, et le dossier personnel de l'utilisateur Linux serait disponible dans l'explorateur Windows à Linux > Ubuntu > home > username.

Il y a des différences notables entre la manière dont Windows et Linux gèrent les chemins d'accès aux fichiers :

* Windows utilise la barre oblique inverse (\) entre les répertoires, tandis que Linux utilise la barre oblique (/).
* Linux utilise une approche sensible à la casse pour les noms de fichiers et de répertoires, ce qui signifie que les lettres majuscules et minuscules sont différentes; par exemple FILE.TXT, file.txt et FILE.txt sont tous des fichiers différents sous Linux. Windows n'est pas sensible à la casse, donc les trois exemples donnés précédemment pointeraient vers le même fichier dans Windows.

== Accès aux fichiers Windows à partir de Linux (ligne de commande) ==

# Recherchez le chemin complet du fichier ou du dossier sous Windows.
# Notez la lettre du lecteur (par exemple, C:\).
# Remplacez la lettre du lecteur par /mnt/{letter}/.
# Changez toutes les barres obliques inverses en barres obliques.

Exemples :

* C:\Users\user1\Documents\File1.txt se trouve à /mnt/c/Users/user1/Documents/File1.txt sous Linux.
* D:\Data\Project\Dataset\ se trouve à  /mnt/d/Data/Project/Dataset/ sous Linux.

== Accès aux fichiers Linux à partir de Windows (2 méthodes) ==

=== Méthode 1 ===

# Recherchez le chemin complet du fichier ou du dossier sous Linux.
# Utilisez la barre latérale de l'explorateur Windows pour rechercher l'entrée Linux (généralement en bas) et développez l'arborescence dessous.
# Sélectionnez l'environnement Linux qui contient le fichier (Ubuntu par défaut).
# Naviguez dans la même structure de dossiers qu'à l'étape 1 pour trouver le fichier ou le dossier.

Exemple :

* /home/username/file1.txt se trouve à Linux > Ubuntu > home > username > file1.txt dans Windows Explorer.

=== Méthode 2 ===

# Ouvrez une ligne de commande WSL et modifiez le répertoire où le fichier est stocké.
# Exécutez  pour ouvrir une fenêtre de l'explorateur Windows dans le répertoire prévu (le point final est important et demande à l'explorateur d'ouvrir le répertoire actuel).

= Transférer des données avec WSL =

Un cas d'utilisation courant de WSL consiste à l'utiliser pour transférer des données vers les ressources de l'Alliance à l'aide de programmes tels que . Souvent, la prise en charge de l'authentification multifacteur est plus forte sous Linux (et par extension WSL) en raison de divers facteurs techniques. Vous pouvez facilement installer de tels programmes dans l'environnement Ubuntu WSL. Dans le cas de FileZilla :

L'application est maintenant installée et vous pouvez la lancer soit par la ligne de commande Linux avec filezilla, ou via le menu de démarrage de Windows.

Lorsque vous parcourez le système de fichiers Linux à l'aide de tels outils, n'oubliez pas que vos fichiers Windows se trouvent par défaut sous /mnt/{lettre de lecteur} et que vous pouvez y accéder directement sans avoir à les copier d'abord dans l'environnement Linux.

Pour plus d'information, consultez la page Transfert de données.