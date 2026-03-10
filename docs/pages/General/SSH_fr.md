---
title: "SSH/fr"
url: "https://docs.alliancecan.ca/wiki/SSH/fr"
category: "General"
last_modified: "2026-03-06T19:45:31Z"
page_id: 524
display_title: "SSH"
language: "fr"
---

Le protocole SSH (Secure Shell) est fréquemment utilisé pour obtenir une connexion sécurisée à une machine à distance. Une connexion SSH est entièrement chiffrée, ce qui comprend les informations d'identification entrées pour se connecter (nom d'utilisateur et mot de passe). Le protocole SSH est employé pour vous connecter à nos grappes afin d'exécuter des commandes, vérifier la progression des tâches ou, dans certains cas, transférer des fichiers.

Il existe des implémentations logicielles du protocole SSH pour la majorité des systèmes d'exploitation importants.
*Sous macOS et Linux, le plus utilisé est OpenSSH, une application en ligne de commande installée par défaut.
*Avec les récentes versions de Windows,  SSH est disponible via le terminal PowerShell, dans l'invite cmd ou par WSL (Windows Subsystem for Linux). D'autres clients SSH sont aussi offerts par des tiers comme PuTTY, MobaXTerm, WinSCP et Bitvise.

Pour utiliser correctement ces implémentations de SSH, vous devez :
* Connaître le nom de la machine à laquelle vous voulez vous connecter; le format ressemble à fir.alliancecan.ca ou trillium.alliancecan.ca.
* Connaître votre nom d'utilisateur (username); le format ressemble à ansmith. Votre nom d'utilisateur n'est pas votre adresse de courriel, ni votre CCI (par exemple code>abc-123), ni un CCRI (par exemple abc-123-01).
* Connaître votre mot de passe ou utiliser une clé SSH. Votre mot de passe est le même que celui que vous utilisez pour vous connecter au portail CCDB. Nous vous recommandons de créer et utiliser une clé SSH, ce qui est plus sécuritaire qu'un mot de passe.
*  Vous enregistrer à l'authentification multifacteur et vous souvenir de votre deuxième facteur.
* Vous avez demandé l'accès au système pour votre compte.

Dans un client ligne de commande (par exemple /Applications/Utilities/Terminal.app sous macOS; cmd ou PowerShell sous Windows), utilisez la commande ssh ainsi

Pour plus d'information sur les clients graphiques comme MobaXterm ou PuTTY, consultez
*Connexion à un serveur avec MobaXTerm
*Connexion à un serveur avec PuTTY

À votre première connexion à une machine, on vous demandera d'enregistrer une copie de sa clé hôte (host key); cette clé est un identifiant unique avec lequel le client SSH vérifie s'il s'agit de la même machine quand vous vous connectez par la suite.

Pour plus d'information sur comment générer des paires de clés, consultez
*Clés SSH

= X11 pour les applications graphiques =

SSH prend en charge les applications graphiques via le protocole X, connu sous le nom de X11. Pour utiliser X11, un serveur X11 doit être installé sur votre ordinateur. Sous Linux, un serveur X11 sera habituellement déjà installé, mais sous macOS vous devrez généralement installer un paquet externe tel que XQuartz. Sous Windows, MobaXterm est fourni avec un serveur X11; avec PuTTY, le serveur est  VcXsrv.

En ligne de commande SSH, ajoutez l'option -Y pour permettre les communications X11.

= Erreurs de connexion =
Il est possible que vous receviez un message d'erreur lors de votre connexion à une grappe  :
* no matching cipher found
* no matching MAC found
* unable to negotiate a key exchange method
* couldn't agree a key exchange algorithm
* remote host identification has changed

Ce dernier message peut indiquer une attaque de l'homme du milieu (man-in-the-middle attack) ou une mise à jour de la sécurité pour la grappe à laquelle vous voulez vous connecter. Si ce message est affiché, vérifiez si l'empreinte (fingerprint) de la clé hôte mentionnée correspond à une des clés hôtes valides; si c'est le cas, vous pouvez poursuivre la connexion.
Si la clé hôte n'est pas dans la liste, fermez la connexion et contactez le soutien technique.

Les utilisateurs de Niagara ont eu  des mesures à prendre suite à la mise à jour de sécurité du 31 mai 2019. Des mises à jour semblables ont été effectuées sur les autres grappes vers la fin de septembre 2019; pour plus d'information, consultez la page wiki sur l'amélioration de la sécurité.

Dans le cas des autres messages d'erreur, vous devrez effectuer la mise à jour de votre système d'exploitation et/ou de votre client SSH pour permettre un chiffrement plus robuste, des protocoles d'échange de clés et des algorithmes MAC (message authentication code).

Ces erreurs sont connues pour les versions suivantes qui devront être mises à jour :
* OpenSSH sous CentOS/RHEL 5
*  PuTTY v0.64 et moins, sous Windows