---
title: "Arbutus object storage/fr"
url: "https://docs.alliancecan.ca/wiki/Arbutus_object_storage/fr"
category: "General"
last_modified: "2025-02-14T21:18:32Z"
page_id: 19937
display_title: "Stockage objet sur Arbutus"
language: "fr"
---

== Introduction ==

Le stockage objet est une installation de stockage plus simple qu'un système de fichiers hiérarchique normal, mais qui permet d'éviter certains goulots d'étranglement de la performance. Les objets peuvent être créés, remplacés ou supprimés, mais ne peuvent pas être modifiés sur place, comme c'est le cas avec le stockage traditionnel. Ce type de stockage est devenu très populaire en raison de sa capacité de gérer plusieurs fichiers et des fichiers de grande taille, ainsi que l'existence de nombreux outils compatibles.

Un objet est un fichier dans un espace de nommage (namespace) plat : un objet peut être créé ou téléchargé dans son ensemble, mais vous ne pouvez pas modifier les octets qu’il contient. Un objet utilise la nomenclature bucket:tag sans qu’il soit imbriqué davantage. Puisque les opérations sur les buckets concernent l’entièreté d’un fichier, le fournisseur peut utiliser une représentation interne plus simple. L’espace de nommage plat permet au fournisseur d’éviter les goulots d’étranglement des métadonnées; on peut dire que c’est une sorte de stockage de clés et de valeurs.

La meilleure façon d’utiliser le stockage objet est de stocker et d’exporter des éléments qui ne sont pas nommés dans une structure hiérarchique; auxquels on accède principalement de manière totale et en lecture seule; et pour lesquels les règles d’accès et de contrôle sont simples. Nous recommandons son utilisation avec des plateformes ou des logiciels qui sont conçus pour travailler avec des données qui vivent dans un espace de stockage objet.

Sur Arbutus, chaque projet dispose par défaut de 1To de stockage objet. Si ceci est insuffisant, vous pouvez soit utiliser notre service d'accès rapide. Si vous avez besoin de plus de 10To, présentez une demande au prochain concours pour l'allocation des ressources.

Contrairement à un environnement de calcul sur une grappe, les fonctions d'administration du système pour le stockage objet d'un utilisateur sont la responsabilité de cet utilisateur, ce qui signifie que les opérations comme la sauvegarde doivent être effectuées par l'utilisateur. Pour plus d'information, voyez Options de stockage infonuagique.

Nous offrons deux protocoles différents pour accéder à Object Store dans OpenStack : Swift et Amazon Simple Storage Service (S3).

Ces protocoles se ressemblent beaucoup et sont interchangeables dans la plupart des cas. Il n’est pas nécessaire de vous en tenir toujours au même protocole puisque les conteneurs ou compartiments (buckets) et les objets sont accessibles par les protocoles Swift et S3. Certaines différences existent toutefois dans le contexte du stockage objet sur Arbutus.

Swift est le protocole par défaut et est le plus simple à utiliser; vous n’avez pas à gérer les identifiants puisque l’accès se fait avec votre compte Arbutus. Par contre, Swift n’offre pas toutes les fonctionnalités de S3. Le principal cas d'usage est que vous devez utiliser S3 pour gérer vos conteneurs avec des politiques d'accès parce que Swift ne prend pas en charge ces politiques. De plus, S3 vous permet de créer et de gérer vos propres clés, ce qui peut être nécessaire si par exemple vous voulez créer un compte en lecture seule pour une application en particulier. Consultez la the OpenStack S3/Swift liste des compatibilités.

==Accès et gestion du Object Store==

Pour gérer le Object Store vous avez besoin de votre propre identifiant ainsi que de la clé secrète pour accéder au stockage.  Générez-les avec le  votre ID d'accès S3 et la clé secrète pour le protocole avec le client de ligne de commande OpenStack.

openstack ec2 credentials create

= Accès au Object Store =
Les politiques d'accès ne peuvent pas se faire via un navigateur web, mais par un client compatible SWIFT ou S3. L'accès aux conteneurs de données peut se faire de plusieurs façons :

# via un client compatible avec S3 (par exemple s3cmd);
# via Globus;
# via un point HTTPS dans un navigateur, pourvu que vos politiques soient configurés comme étant publiques et non par défaut.
https://object-arbutus.cloud.computecanada.ca:443/DATA_CONTAINER/FILENAME

== Gestion du stockage objet sur Arbutus ==

La manière recommandée de gérer les conteneurs et les objets dans le Stockage d'Objet d'Arbutus est d'utiliser l'outil s3cmd, qui est disponible sous Linux.
Notre documentation fournit des instructions spécifiques sur la configuration et la gestion des accès avec le client s3cmd.
Il est également possible d'utiliser d'autres clients compatibles S3 qui sont également compatibles avec le stockage objet d'Arbutus.

De plus, nous pouvons effectuer certaines tâches de gestion pour notre stockage d'objets en utilisant la section Conteneurs sous l'onglet Stockage d'Objet dans le Tableau de bord OpenStack d'Arbutus.

Cette interface fait référence aux conteneurs de données, également appelés buckets dans d'autres systèmes de stockage objet.

En utilisant le tableau de bord, nous pouvons créer de nouveaux conteneurs de données, téléverser des fichiers et créer des dossiers. Nous pouvons également créer des conteneurs de données en utilisant un client compatible S3.

Si vous créez un nouveau conteneur public, n'importe qui sur internet peut lire son contenu en naviguant simplement à l'adresse suivante

https://object-arbutus.cloud.computecanada.ca//

avec vos noms de conteneurs et d'objets insérés à la place.

Pour rendre un conteneur de données accessible au public, nous pouvons modifier sa politique pour autoriser l'accès public. Cela peut s'avérer pratique si nous avons à partager des fichiers avec une audience élargie. Nous pouvons gérer les politiques de conteneur avec des fichiers JSON, nous permettant de spécifier divers contrôles d'accès pour nos conteneurs et objets.

=== Gestion des politiques de conteneurs de données (bucket) pour le stockage objet sur Arbutus ===

Présentement, le Stockage d'Objet d'Arbutus implémente seulement un sous-ensemble de la spécification AWS pour les politiques de conteneurs de données. L'exemple suivant montre comment créer, appliquer et visualiser une politique. La première étape consiste à créer un fichier JSON de politique.

{
    "Version": "2012-10-17",
    "Id": "S3PolicyId1",
    "Statement": [
        {
            "Sid": "IPAllow",
            "Effect": "Deny",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": [
                "arn:aws:s3:::testbucket",
                "arn:aws:s3:::testbucket/*"
            ],
            "Condition": {
                "NotIpAddress": {
                    "aws:SourceIp": "206.12.0.0/16",
                    "aws:SourceIp": "142.104.0.0/16"
                }
            }
        }
    ]
}

Cet exemple refuse l'accès sauf à partir des plages d'adresses IP sources spécifiées en notation CIDR (Classless Inter-Domain Routing). Dans cet exemple, le s3://testbucket est limité à la plage d'adresses IP publiques (206.12.0.0/16) utilisée par le nuage Arbutus et à la plage d'adresses IP publiques (142.104.0.0/16) utilisée par l'Université de Victoria.

Une fois que vous avez votre fichier de politique, vous pouvez l'appliquer à votre conteneur de données:

s3cmd setpolicy testbucket.policy s3://testbucket

Pour voir la politique, vous pouvez utiliser la commande suivante

s3cmd info s3://testbucket

=== Sous-ensemble ===

En date de septembre 2023, nous supportons les actions suivantes :

* s3:AbortMultipartUpload
* s3:CreateBucket
* s3:DeleteBucketPolicy
* s3:DeleteBucket
* s3:DeleteBucketWebsite
* s3:DeleteObject
* s3:DeleteObjectVersion
* s3:DeleteReplicationConfiguration
* s3:GetAccelerateConfiguration
* s3:GetBucketAcl
* s3:GetBucketCORS
* s3:GetBucketLocation
* s3:GetBucketLogging
* s3:GetBucketNotification
* s3:GetBucketPolicy
* s3:GetBucketRequestPayment
* s3:GetBucketTagging
* s3:GetBucketVersioning
* s3:GetBucketWebsite
* s3:GetLifecycleConfiguration
* s3:GetObjectAcl
* s3:GetObject
* s3:GetObjectTorrent
* s3:GetObjectVersionAcl
* s3:GetObjectVersion
* s3:GetObjectVersionTorrent
* s3:GetReplicationConfiguration
* s3:IPAddress
* s3:NotIpAddress
* s3:ListAllMyBuckets
* s3:ListBucketMultipartUploads
* s3:ListBucket
* s3:ListBucketVersions
* s3:ListMultipartUploadParts
* s3:PutAccelerateConfiguration
* s3:PutBucketAcl
* s3:PutBucketCORS
* s3:PutBucketLogging
* s3:PutBucketNotification
* s3:PutBucketPolicy
* s3:PutBucketRequestPayment
* s3:PutBucketTagging
* s3:PutBucketVersioning
* s3:PutBucketWebsite
* s3:PutLifecycleConfiguration
* s3:PutObjectAcl
* s3:PutObject
* s3:PutObjectVersionAcl
* s3:PutReplicationConfiguration
* s3:RestoreObject