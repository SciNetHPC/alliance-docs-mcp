---
title: "Configuring Apache to use SSL/fr"
url: "https://docs.alliancecan.ca/wiki/Configuring_Apache_to_use_SSL/fr"
category: "General"
last_modified: "2024-07-05T17:12:14Z"
page_id: 1788
display_title: "Configuration du serveur Apache pour utiliser SSL"
language: "fr"
---

Page enfant de Création d'un serveur web sur un nuage

Le terme SSL réfère à la fois au protocole  Transport Layer Security (TLS) et à son prédécesseur Secure Sockets Layer (SSL). Ils servent au chiffrement des données communiquées sur les réseaux. Le chiffrement protège les données critiques qui transitent sur l'Internet, par exemple les mots de passe. Cependant, même si le serveur web envoie au client de l'information qui n'est pas sensible, le chiffrement empêchera une tierce partie d'intercepter et de modifier les données avant qu'elles se rendent à destination. Dans la plupart des cas, les certificats SSL sont utiles pour chiffrer les données à provenance ou à destination d'un serveur web via l'internet.

Il y a deux types de certificats : les certificats signés par une tierce partie et les certificats autosignés. Dans la plupart des cas, vous voudrez un certificat signé par une tierce partie, ce qui se fait très facilement avec Let's Encrypt, comme expliqué ci-dessous. Par contre, d'autres cas (comme les tests) se prêtent mieux aux certificats autosignés. De cette manière, les données en provenance et à destination de votre serveur sont cryptées; par contre, aucune tierce partie ne confirme la validité de votre serveur web et un avertissement sera affiché quand on voudra s'y connecter. Vous ne voudrez probablement pas utiliser un certificat autosigné si votre site est ouvert au public.

Une fois que vous avez le certificat et que le serveur web est configuré, il est recommandé d'utiliser l'outil ssltest de ssllabs qui peut vous suggérer des modifications à votre configuration pour renforcer la sécurité.

==Certificat signé==
Un certificat signé par une Autorité de certification (CA, pour Certificate Authority) permet aux utilisateurs d'un site web de s'assurer qu'un tiers (le CA) confirme l'identité du site et prévient ce qu'on appelle une attaque de l'homme du milieu.

Plusieurs CA exigent des frais annuels, contrairement à  Let's Encrypt. Un certificat SSL signé par ce CA peut être créé et renouvelé automatiquement par l'outil Certbot qui configure aussi votre serveur web pour l'utilisation de ce même certificat. Pour un démarrage rapide, consultez la page principale de Certbot. Les détails se trouvent dans la documentation Certbot.

Si vous configurez Certbot via Apache, ouvrez le port 443 (TCP ingress) pour que Certbot puisse se connecter au site (ceci n'est pas couvert dans la documentation de Certbot).

==Certificat autosigné==
Cette section décrit la procédure de création d'un certificat SSL autosigné et la configuration d'Apache pour le chiffrement. Il n'est pas recommandé d'utiliser un certificat autosigné sur un site de production d'importance; ces certificats conviendront cependant à la production sur des sites restreints à usage local, ou encore dans un environnement de test.

Les étapes suivantes décrivent la procédure sous Ubuntu. On trouvera certaines différences sous d'autres systèmes d'exploitation, notamment en ce qui a trait aux commandes, aux localisations ou aux noms des fichiers de configuration.

Activer le module SSL
Installez Apache (voir  Installer Apache), puis activez le module SSL  ainsi :

Créer un certificat SSL autosigné
Si on vous demande une phrase de passe, assurez-vous de relancer la commande avec la syntaxe correcte, y compris l'option -node. Vous devrez ensuite répondre aux questions qui suivent, pour lesquelles nous donnons des exemples de réponse.

  Country Name (2 letter code) [AU]:CA
  State or Province Name (full name) [Some-State]:Nova Scotia
  Locality Name (eg, city) []:Halifax
  Organization Name (eg, company) [Internet Widgits Pty Ltd]:Alliance
  Organizational Unit Name (eg, section) []:ACENET
  Common Name (e.g. server FQDN or YOUR name) []:XXX-XXX-XXX-XXX.cloud.computecanada.ca
  Email Address []:

La réponse à Common Name est la plus importante; il s'agit du nom de domaine de votre serveur. Pour une machine virtuelle sur un de nos nuages, remplacez les X dans l'exemple par l'adresse IP flottante associée à la machine virtuelle.

Définir le propriétaire et les autorisations
Pour définir le propriétaire et les autorisations associés à la clé privés, entrez les commandes

Configurer Apache pour utiliser le certificat
Modifiez le fichier de configuration SSL avec la commande

et remplacez les deux lignes suivantes
 SSLCertificateFile      /etc/ssl/certs/ssl-cert-snakeoil.pem
 SSLCertificateKeyFile /etc/ssl/private/ssl-cert-snakeoil.key
par les trois lignes
 SSLCertificateFile      /etc/ssl/certs/server.crt
 SSLCertificateKeyFile /etc/ssl/private/server.key
 SSLCertificateChainFile /etc/ssl/certs/server.crt

Vérifiez que le chemin pour  DocumentRoot correspond au chemin défini dans /etc/apache2/sites-available/000-default.conf, en autant que SSL s'applique à ce site.
Renforcer la sécurité
Redirigez les requêtes http vers https; exigez des versions plus à jour de SSL; utilisez de meilleures options de chiffrement, d'abord en modifiant le fichier ainsi  et ensuite en ajoutant

 ServerName XXX-XXX-XXX-XXX.cloud.computecanada.ca
SSLProtocol all -SSLv2 -SSLv3
SSLCipherSuite HIGH:MEDIUM:!aNULL:!MD5:!SEED:!IDEA:!RC4
SSLHonorCipherOrder on

dans le bas, à l'intérieur de la balise ; remplacez les X aux deux endroits par l'IP de la machine virtuelle (notez qu'il faut utiliser des tirets dans l'IP plutôt que des points). Entrez une commande de redirection sur le serveur virtuel en modifiant le fichier de configuration du site web par défaut avec
et en ajoutant la ligne

 Redirect permanent / https://XXX-XXX-XXX-XXX.cloud.computecanada.ca

à l'intérieur de la balise .

Activer le site sécurisé