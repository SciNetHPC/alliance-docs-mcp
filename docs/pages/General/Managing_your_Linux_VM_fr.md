---
title: "Managing your Linux VM/fr"
url: "https://docs.alliancecan.ca/wiki/Managing_your_Linux_VM/fr"
category: "General"
last_modified: "2023-05-01T20:13:32Z"
page_id: 21682
display_title: "Gestion des machines virtuelles sous Linux"
language: "fr"
---

Linux est très utilisé pour les machines virtuelles. Les distributions souvent employées sont AlmaLunix, CentOS, Debian, Fedora et Ubuntu. Vous trouverez ici de l'assistance pour les tâches communes. Il est aussi possible d'utiliser le système d'exploitation Windows.

=Gestion des utilisateurs sous Linux=

Il existe quelques méthodes pour permettre à plusieurs personnes d'avoir accès à une machine virtuelle. Notre recommandation est de créer de nouveaux comptes d'utilisateur et de leur associer des clés SSH.

==Créer un compte d'utilisateur et ses clés==
Pour créer un compte d'utilisateur sur Ubuntu, utilisez la commande

Pour pouvoir se connecter, le nouvel utilisateur devra avoir une paire de clés; selon le système d'exploitation, voyez Générer des clés SSH sous Windows  ou Créer une paire de clés sous Linux et Mac. Ajoutez ensuite la clé publique à /home/USERNAME/.ssh/authorized_keys pour la machine virtuelle et vérifiez que les permissions et le propriétaire sont corrects, comme indiqué aux étapes 2 et 3 de Se connecter avec une paire de clés.

==Privilèges admin==
Pour accorder les privilèges admin (root) à un utilisateur, utilisez la commande

Ceci démarre un éditeur où vous pouvez ajouter une ligne comme
 USERNAME ALL=(ALL) NOPASSWD:ALL
Pour plus d'information sur la commande visudo et sur comment éditer le fichier, consultez le tutoriel de DigitalOcean.

==Problèmes de système et de sécurité==
Référez-vous à
*  Récupération des données d'une machine virtuelle compromise
*  Récupération d'une machine virtuelle via la console