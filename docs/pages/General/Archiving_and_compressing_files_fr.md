---
title: "Archiving and compressing files/fr"
url: "https://docs.alliancecan.ca/wiki/Archiving_and_compressing_files/fr"
category: "General"
last_modified: "2019-07-21T15:42:51Z"
page_id: 11801
display_title: "Archivage et compression de fichiers"
language: "fr"
---

Page enfant de: Stockage et gestion de fichiers

Archiver signifie créer un fichier qui contient plusieurs petits fichiers. Le fait de créer un fichier archive peut améliorer l'efficacité du stockage et vous aider à respecter les quotas. L'archivage peut aussi rendre plus efficace le transfert de fichiers. Par exemple, le protocole scp (secure copy protocol) transfère plus rapidement un fichier archive de taille raisonnable que des milliers de petits fichiers totalisant la même taille.

Compresser signifie modifier le code d'un fichier pour en réduire le nombre de bits. Les avantages sont évidents en ce qui concerne le stockage à long terme des données. Dans le cas du  transfert de données, il faut comparer le temps de compression au temps nécessaire pour déplacer une quantité moindre de bits; voyez ce texte du National Center for Supercomputing Applications.

* Sous Linux, tar est un outil d'archivage et de compression bien connu; voyez le  tutoriel tar.
* Aussi pour l'archivage et la compression, dar offre certaines fonctions avantageuses; voyez le tutoriel dar.
* L'utilitaire zip est bien connu pour l'archivage et la compression dans l'environnement Windows, mais il est disponible avec les grappes de Calcul Canada.
* Les outils de compression gzip, bzip2 et xz peuvent être utilisés par eux-mêmes ou avec tar.