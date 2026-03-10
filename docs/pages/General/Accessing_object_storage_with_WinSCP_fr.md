---
title: "Accessing object storage with WinSCP/fr"
url: "https://docs.alliancecan.ca/wiki/Accessing_object_storage_with_WinSCP/fr"
category: "General"
last_modified: "2023-03-14T22:23:02Z"
page_id: 22573
display_title: "Stockage objet : Accès avec WinSCP"
language: "fr"
---

Cette page contient les renseignements sur la configuration et l'accès au stockage objet sur Arbutus avec WinSCP, l'un des  clients pour le stockage de ce type.

== Installation ==
Installez WinSCP à partir de https://winscp.net/.

== Configuration ==
Sous New Session, entrez

File protocol: Amazon S3
Host name: object-arbutus.cloud.computecanada.ca
Port number: 443
Access key ID: 20_DIGIT_ACCESS_KEY

Cliquez ensuite sur le bouton Save

Cliquez ensuite sur le bouton Edit et sur Advanced.... Sous Environment sélectionnez S3. Dans les options pour le protocole, sélectionnez Path dans le champ URL style.

Le choix de Path est important pour que WinSCP fonctionne et évite les erreurs comme

=== Utilisation ===
Cliquez sur le bouton Login et utilisez l’interface de WinSCP pour créer des buckets et y transférer des fichiers.

== Listes de conbtrôle d'accès (ACL) et politiques ==
Cliquez avec le bouton droit sur le nom du fichier pour obtenir la liste des accès, par exemple