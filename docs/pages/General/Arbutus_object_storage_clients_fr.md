---
title: "Arbutus object storage clients/fr"
url: "https://docs.alliancecan.ca/wiki/Arbutus_object_storage_clients/fr"
category: "General"
last_modified: "2025-02-13T22:23:24Z"
page_id: 19995
display_title: "Arbutus : Clients pour le stockage objet"
language: "fr"
---

Pour l'information sur comment obtenir de l'espace de stockage objet sur Arbutus, voir  cette page wiki. Voyez aussi l'information sur les clients :
*  Stockage objet : Accès avec s3cmd
*  Stockage objet : Accès avec WinSCP
*  Stockage objet : Accès avec AWS CLI
* Stockage objet sur Arbutus : Accès via Globus

Il faut noter que la solution de stockage objet sur Arbutus n'utilise pas l’approche S3 Virtual Hosting d’Amazon avec des buckets DNS, contrairement à ces clients qui l’offrent par défaut. Pour ne pas utiliser cette approche, il faut donc configurer les clients en conséquence.