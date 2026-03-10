---
title: "Setting up GUI Desktop on a VM/fr"
url: "https://docs.alliancecan.ca/wiki/Setting_up_GUI_Desktop_on_a_VM/fr"
category: "General"
last_modified: "2023-02-27T21:02:02Z"
page_id: 15506
display_title: "Interface graphique pour les instances OpenStack"
language: "fr"
---

Certains logiciels que vous pouvez installer sur votre machine virtuelle (VM ou instance) sont accessibles uniquement, ou préférablement, via leur interface utilisateur graphique (GUI). Vous pouvez utiliser une interface graphique avec la redirection X11, mais vous pourriez obtenir une meilleure performance en utilisant VNC pour vous connecter à une session graphique qui se trouve sur votre instance.

Nous décrivons ici les étapes pour configurer une interface de bureau   avec VNC sur une instance qui utilise le système d’exploitation Ubuntu.

 Sur votre instance, installez un bureau ayant une interface graphique. Plusieurs paquets sont disponibles pour Ubuntu :
* ubuntu-unity-desktop
* ubuntu-mate-desktop
* lubuntu-desktop
* xubuntu-desktop
* xfce4
* ubuntu-desktop
* kde-plasma-desktop
* ubuntu-desktop-minimal
* cinnamon
* icewm

Cet article montre quelques-uns de ces bureaux. Les commandes suivantes installent un bureau MATE.

Au cours de l'installation du paquet  ubuntu-mate-desktop vous devez sélectionner le gestionnaire de session par défaut; le meilleur choix serait  lightdm. Cette installation peut souvent prendre de 15 à 30 minutes.

Installez le serveur TigerVNC.
Ce logiciel installé sur votre instance permet d’utiliser l’interface de bureau que vous avez installée à l’étape 1.

Cette commande installe le serveur TigerVNC et les logiciels nécessaires. Pour plus d’information sur les serveurs VNC, voyez notre page wiki VNC.

Démarrez le serveur VNC.
 Au premier démarrage du serveur VNC, vous devez entrer un mot de passe qui vous servira à vous connecter au bureau VNC. Il n’est pas nécessaire que le mot de passe soit pour lecture seulement. Pour modifier votre mot de passe, utilisez la commande vncpasswd.

Testez la connexion en ouvrant le port 5901 (pour savoir comment ouvrir un port vers votre instance OpenStack, voir  Groupes de sécurité) et connectez-vous avec un client VNC, par exemple TigerVNC. Cette option n’est pas sécuritaire parce que les données entrant et sortant de l’instance ne seront pas chiffrées. Par contre, cette étape vous permet de tester la connexion client-serveur avant de vous connecter de façon sécuritaire via un tunnel SSH; vous pouvez ignorer cette étape si vous savez comment configurer un tunnel SSH correctement.

Connectez-vous via un tunnel SSH. Vous pouvez consulter cet exemple qui utilise un noeud de calcul sur nos grappes.  Pour vous connecter sous Linux ou Mac :

*Ouvrez votre terminal.
*Dans votre terminal local, entrez SSH -i filepathtoyoursshkey/sshprivatekeyfile.key -L5901:localhost:5901 ubuntu@ipaddressofyourVM
*Lancez votre client VNC.
*Dans le champ pour le serveur VNC, entrez localhost:5901.
*Le bureau graphique pour votre session à distance devrait s’ouvrir.

 Fermez le port 5901; ce port ne sert plus après que la connexion avec le serveur VNC est établie via un tunnel SSH et il est recommandé de supprimer cette règle dans vos groupes de sécurité.

 Quand vous n’avez plus besoin du bureau, arrêtez le serveur VNC avec