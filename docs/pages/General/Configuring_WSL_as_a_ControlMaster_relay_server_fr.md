---
title: "Configuring WSL as a ControlMaster relay server/fr"
url: "https://docs.alliancecan.ca/wiki/Configuring_WSL_as_a_ControlMaster_relay_server/fr"
category: "General"
last_modified: "2024-04-03T17:33:23Z"
page_id: 25218
display_title: "Utiliser WSL comme un serveur relais pour ControlMaster"
language: "fr"
---

Cette procédure permet d'utiliser ControlMaster de WSL pour vous connecter aux grappes avec plusieurs applications Windows natives pour une certaine durée, sans avoir à vous authentifier pour chaque session.

=== Installer Linux sur Windows avec WSL ===
Voir Travailler avec Windows Subsystem for Linux (WSL)

Dans les fichiers de configuration :
* la distribution est Ubuntu
* le nom de l'hôte pour l'instance WSL est ubuntu; /etc/hostname contient ubuntu et /etc/hosts contient 127.0.0.1 localhost ubuntu
* le  nom du système Windows est smart et la connexion est faite par l'utilisateur nommé jaime
* le nom de l'utilisateur pour la VM Ubuntu est aussi jaime
* le nom de l'utilisateur pour l'Alliance est pinto et nous voulons nous connecter à Cedar

=== Installer d'autres logiciels ===

 sudo apt update && sudo apt upgrade -y
 sudo apt install openssh-server -y

Vous pouvez vous connecter à Ubuntu à partir de Windows avec ssh localhost.

=== La configuration ressemble à ===

[ssh client] ----> [ssh relay server] ----> [ssh target server]
your Windows     modified authorized_keys     using cedar for
  machine          in your Ubuntu VM           this exercise
 smart        ubuntu                 Cedar

=== Se connecter à la VM Ubuntu et créer le répertoire custom_ssh===

jaime@ubuntu:~$ cat custom_ssh/sshd_config
Port 2222
HostKey /home/jaime/custom_ssh/ssh_host_ed25519_key
HostKey /home/jaime/custom_ssh/ssh_host_rsa_key
AuthorizedKeysFile /home/jaime/custom_ssh/authorized_keys
ChallengeResponseAuthentication no
UsePAM no
Subsystem sftp /usr/lib/openssh/sftp-server
PidFile /home/jaime/custom_ssh/sshd.pid

Pour copier les clés ssh_host de  /etc/ssh, utilisez
sudo cp /etc/ssh/ssh_host_ed25519_key /home/jaime/custom_ssh/

=== Modifier .ssh/config sur Ubuntu ===

jaime@ubuntu:~$ cat ~/.ssh/config
Host cedar
    ControlPath ~/.ssh/cm-%r@%h:%p
    ControlMaster auto
    ControlPersist 10m
    HostName cedar.alliancecan.ca
    User pinto

=== Modifier les clés permises ===

jaime@ubuntu:~/custom_ssh$ cat /home/jaime/custom_ssh/authorized_keys
ssh-ed25519 AAAZDINzaC1lZDI1NTE5AAC1lZDIvqzlffkzcjRAaMQoTBrPe5FxlSAjRAaMQyVzN+A+

Utilisez la clé publique SSH que vous avez téléchargée dans CCDB.

=== Lancer le serveur sshd server sur Ubuntu ===

jaime@ubuntu:~/custom_ssh$ /usr/sbin/sshd -f ${HOME}/custom_ssh/sshd_config

Assurez-vous que le serveur est lancé avec votre profil et non avec le profil racine (root). Vous devrez lancer le serveur sshd à chaque fois que vous redémarrez votre ordinateur ou que WSL est fermé ou lancé de nouveau.

=== Modifier .ssh/config sur smart avec RemoteCommand ===

jaime@smart ~/.ssh cat config
Host ubuntu
        Hostname localhost
        RemoteCommand ssh cedar

=== Se connecter à Cedar ===

jaime@smart ~
$ ssh -t ubuntu -p 2222
Enter passphrase for key '/home/jaime/.ssh/id_ed25519':
Last login: Fri Mar 22 10:50:12 2024 from 99.239.174.157
================================================================================
Welcome to Cedar! / Bienvenue sur Cedar!
...
...
...
[pinto@cedar1 ~]$

=== Autre option de configuration ===
Vous pouvez aussi personnaliser les clés permises pour Ubuntu et le fichier ~/.ssh/config de Windows pour que certaines applications graphiques fonctionnent sans avoir à indiquer RemoteCommand (par exemple, WinSCP). Dans ce cas,  RemoteCommand est indiqué pour la clé publique.

jaime@ubuntu:~/custom_ssh$ cat /home/jaime/custom_ssh/authorized_keys
command="ssh cedar" ssh-ed25519 AAAZDINzaC1lZDI1NTE5AAC1lZDIvqzlffkzcjRAaMQoTBrPe5FxlSAjRAaMQyVzN+A+

jaime@smart ~/.ssh cat config
Host ubuntu
        Hostname localhost
        #RemoteCommand ssh cedar

Par la suite, vous pouvez encore utiliser ssh ubuntu -p 2222 à partir d'un interpréteur (shell) Windows.

=== Configuration avec MobaXterm ===