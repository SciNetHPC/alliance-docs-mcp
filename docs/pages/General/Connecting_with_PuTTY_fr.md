---
title: "Connecting with PuTTY/fr"
url: "https://docs.alliancecan.ca/wiki/Connecting_with_PuTTY/fr"
category: "General"
last_modified: "2023-04-24T18:56:59Z"
page_id: 1667
display_title: "Connexion à un serveur avec PuTTY"
language: "fr"
---

Démarrez PuTTY et entrez  le nom ou l'adresse du serveur auquel vous voulez vous connecter.
Les paramètres peuvent être sauvegardés pour usage futur : entrez le nom dans le champ Save Session et cliquez sur le bouton Save à droite de la liste des noms.
Vous pouvez aussi sauvegarder le nom d'utilisateur pour une connexion à un serveur en particulier : sous Category->Connection->Data, entrez le nom d'utilisateur dans le champ Auto-login username.  Il ne sera plus nécessaire d'entrer le nom d'utilisateur pour vous connecter.

=Redirection X11=
Pour utiliser des applications graphiques, activez la redirection X11 : sous Connection->SSH->X11, cochez Enable X11 forwarding.
La fonction de redirection X11 nécessite un serveur X window tel que  Xming ou, pour les versions récentes de Windows, VcXsrv. Le serveur X window devrait être en marche avant d'établir la connexion SSH. Pour tester la redirection, ouvrez une session PuTTY et lancez une commande simple, par exemple xclock. L'affichage d'une fenêtre contextuelle montrant une horloge indique que la redirection X11 est probablement fonctionnelle.

=Paire de clés SSH=
Pour localiser la clé privée :  sous Category->Connection->SSH->Auth, cliquez sur le bouton Browse.
PuTTY utilise les fichiers avec le suffixe .ppk; ces suffixes sont générés via PuTTYGen (voir Generating SSH keys in Windows pour savoir comment créer ces clés).
Dans les versions plus récentes de Putty, vous devez cliquer sur le signe + près de Auth, puis sélectionner Credentials pour pouvoir chercher le Private key file for authentication. Dans cette plus récente interface, les champs Certificate to use et Plugin to provide authentication response doivent être vides.