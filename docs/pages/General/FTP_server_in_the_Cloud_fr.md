---
title: "FTP server in the Cloud/fr"
url: "https://docs.alliancecan.ca/wiki/FTP_server_in_the_Cloud/fr"
category: "General"
last_modified: "2023-07-06T21:46:38Z"
page_id: 3140
display_title: "Serveurs FTP dans notre environnement infonuagique"
language: "fr"
---

Page enfant de Services infonuagiques

=Meilleures options en remplacement de FTP=
Si vous pouvez utiliser un autre protocole que FTP, il existe d'autres possibilités.

* Dans un contexte de FTP acceptant les connexions anonymes
** pour un accès en lecture seulement (read only), utilisez HTTP (voir Création d'un serveur web sur un nuage);
** pour un accès en lecture et écriture (read/write), comme il est extrêmement risqué d'accepter des fichiers transférés de façon anonyme, contactez le soutien technique; en connaissant votre cas particulier, nous pourrons vous aider à trouver une solution sécuritaire.
* Si vous voulez que les utilisateurs FTP soient authentifiés par des identifiants et des mots de passe
** SFTP est une option plus sécuritaire et plus facile;
** FTPS, une extension de FTP, utilise TLS (Transport Layer Security) pour crypter les données en entrée et en sortie.
Quand l'authentification se fait par mot de passe, les données transmises devraient être cryptées pour éviter qu'une personne habile puisse décoder le mot de passe. Nous recommandons fortement de ne pas permettre l'accès par mot de passe à votre instance (ou VM pour virtual machine) puisque toute machine connectée à l'internet est à risque de recevoir des attaques par force brute (brute-force attacks). L'authentification par clés SSH est préférable  et fonctionne avec SFTP.

=Configurer un serveur FTP=
Si vous devez utiliser FTP, consultez un des guides suivants, selon le système d'exploitation :
*Ubuntu
*CentOS 6
Les ports d'une instance utilisés par FTP doivent être ouverts; voyez Groupes de sécurité pour savoir comment ouvrir les ports. FTP utilise le port 21 pour lancer la requête de transfert de fichiers, mais le transfert comme tel peut s'effectuer sur un port aléatoire au-delà du port 1025; les détails varient toutefois selon le mode d'opération de FTP (par exemple, le port 20 pourrait être utilisé). Ceci signifie que, pour permettre un accès FTP à votre instance, vous devez ouvrir le port 21, possiblement le port 20 et probablement les ports au-delà de 1025. Chaque port ouvert représente un risque de sécurité et les protocoles autres que FTP sont à privilégier.
Pour plus d'information sur les ports utilisés par FTP, lisez cet article.