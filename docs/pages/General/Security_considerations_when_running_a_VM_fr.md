---
title: "Security considerations when running a VM/fr"
url: "https://docs.alliancecan.ca/wiki/Security_considerations_when_running_a_VM/fr"
category: "General"
last_modified: "2024-09-27T18:55:58Z"
page_id: 3486
display_title: "Sécurité des instances virtuelles"
language: "fr"
---

Page enfant de Service infonuagique de Calcul Canada

Vous êtes responsable de la sécurité pour vos instances virtuelles dans l'environnement infonuagique de Calcul Canada.

Sans être un guide complet pour la sécurité de vos instances, vous trouverez ici les règles de base pour créer une instance virtuelle.

==Information de base==
La vidéo Safety First! (environ 90 minutes) aborde l'information de base; elle est disponible en anglais seulement.

Vous pouvez aller directement aux sujets suivants :
* Talk overview
* Cloud service levels
* General security principles
* Key topics
* Creating a first VM (with some comments about security)
* OpenStack security groups
* SSH security
* Logs
* Creating backups of VMs

==Sécurité du système d'exploitation==
* Effectuez régulièrement les mises à jour de sécurité (voir Mise à jour d'une instance virtuelle ci-dessous).
* Évitez d'utiliser des paquets de sources non réputées.
* Utilisez l'image la plus récente; par exemple, évitez d'utiliser Ubuntu 14.04 si Ubuntu 18.04 est disponible.
* Utilisez l’authentification qui se fait par défaut avec des  clés SSH; elle est beaucoup plus sûre que par mots de passe.
* Installez fail2ban pour parer les attaques par force brute.

==Sécurité du réseau==
* Limitez l'accès à votre service. Évitez d’utiliser 0.0.0.0 dans le champ CIDR du formulaire pour le groupe de sécurité et, en particulier, ne créez pas des règles pour 0.0.0.0 pour le groupe de sécurité par défaut, ce qui permettrait l’accès à toutes les instances du projet.
** Portez attention aux adresses IP rendues disponibles par la configuration du netmask.
* Ne regroupez pas les ports d'accès.
* Portez attention aux règles de sécurité, en particulier pour :
** les services qui ne devraient pas être accédés publiquement
*** ssh (22); ce service permet une connexion interactive avec votre instance et NE DOIT PAS être publiquement accessible.
*** RDP (3389); ce service permet une connexion interactive avec votre instance et NE DOIT PAS être publiquement accessible.
*** mysql (3306)
*** VNC (5900-5906); ce service permet une connexion interactive avec votre instance et NE DOIT PAS être publiquement accessible.
*** postgresql (5432)
*** nosql
*** RDP (3389)
*** tomcat
*** et plusieurs autres
** les services qui devraient être accédés publiquement
*** Apache (80, 443)
*** Nginx (80, 443)
*** et autres
* Configurez le serveur Web pour HTTPS plutôt que HTTP.
* Dans plusieurs cas, HTTP ne devrait être utilisé que pour rediriger vers HTTPS.
* N'installez pas de serveur de courriel.
* N'installez pas un serveur BitTorrent.

==Mise à jour d'une instance virtuelle==
Effectuez régulièrement des mises à jour du système d’exploitation de vos instances, idéalement chaque semaine ou chaque fois que de nouveaux paquets sont disponibles. Utilisez les commandes suivantes, selon la distribution Linux. Vous devrez redémarrer votre instance et vous connecter à nouveau.
===Ubuntu/Debian===

$ sudo apt-get update
$ sudo apt-get dist-upgrade
$ sudo reboot

===CentOS===

$ sudo yum update
$ sudo reboot

===Fedora===

$ sudo dnf update
$ sudo reboot

==Références==
Tips for Securing Your EC2 Instance (article Amazon).