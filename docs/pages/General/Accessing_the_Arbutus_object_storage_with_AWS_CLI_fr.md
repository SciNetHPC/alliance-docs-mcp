---
title: "Accessing the Arbutus object storage with AWS CLI/fr"
url: "https://docs.alliancecan.ca/wiki/Accessing_the_Arbutus_object_storage_with_AWS_CLI/fr"
category: "General"
last_modified: "2024-06-20T18:17:26Z"
page_id: 22502
display_title: "Stockage objet sur Arbutus : Accès avec AWS CLI"
language: "fr"
---

Cette page contient les renseignements sur la configuration et l'accès au stockage objet sur Arbutus avec AWS CLI, l'un des  clients pour le stockage de ce type.

En comparaison des autres clients utilisés pour le stockage objet, AWS CLI offre un meilleur support pour les grands fichiers (>5Go) en plus de la commande sync qui est très utile. Notez cependant que nous n'avons pas testé toutes les fonctionnalités.

== Installation ==

pip install awscli awscli-plugin-endpoint

== Configuration  ==

Générez l'ID de la clé d'accès et la clé secrète.

openstack ec2 credentials create

Modifiez ou créez ~/.aws/credentials et ajoutez les renseignements qui viennent d'être générés.

[default]
aws_access_key_id =
aws_secret_access_key =

Modifiez ~/.aws/config et ajoutez la configuration suivante :

[plugins]
endpoint = awscli_plugin_endpoint

[profile default]
s3 =
  endpoint_url = https://object-arbutus.cloud.computecanada.ca
  signature_version = s3v4
s3api =
  endpoint_url = https://object-arbutus.cloud.computecanada.ca

== Utilisation ==

export AWS_PROFILE=default
aws s3 ls
aws s3 sync local_directory s3://container-name/prefix

Vous trouverez d'autres exemples d'utilisation de AWS CLI sur ce site externe.