---
title: "Uv/fr"
url: "https://docs.alliancecan.ca/wiki/Uv/fr"
category: "General"
last_modified: "2025-11-18T21:50:37Z"
page_id: 31908
display_title: "uv"
language: "fr"
---

uv est un gestionnaire de paquets et de projets Python extrêmement rapide, écrit en Rust. Son utilisation pourrait fonctionner, mais vous risquez de rencontrer des problèmes.

Voici quelques difficultés que vous pourriez rencontrer :
* certains paquets sont distribués dans un format incompatible avec nos grappes, mais uv tente quand même de les installer;
* uv est incapable de trouver les paquets Python fournis par les modules chargés;
* uv peut rapidement saturer le quota de votre répertoire /home, car il stocke un très grand nombre de fichiers dans la cache.

=Installation de paquets Python=
Pour installer des paquets sur nos grappes, utilisez pip; voir Python.

Assurez-vous d'avoir au moins la version pip>=25.0.

=Dépannage=

==Vider la cache==
Pour vider la cache, la commande est

Par la suite, pour ne pas utiliser la cache, la commande est uv --no-cache.