---
title: "Arbutus/fr"
url: "https://docs.alliancecan.ca/wiki/Arbutus/fr"
category: "General"
last_modified: "2025-06-27T18:07:04Z"
page_id: 27326
display_title: "Arbutus"
language: "fr"
---

Disponibilité : fin de l'été de 2025
Tableau de bord OpenStack : https://arbutus.cloud.alliancecan.ca
Point de chute Globus : à confirmer
Stockage objet (S3 ou Swift) : https://object-arbutus.cloud.computecanada.ca/

Arbutus est un  nuage IaaS (Infrastructure-as-a-Service) hébergé à l'Université de Victoria.

==Stockage==
7 Po de stockage Ceph pour les volumes et les instantanés
26 Po de stockage Ceph pour le stockage objet et les systèmes de fichiers partagés
3 Po de stockage Ceph NVMe pour les volumes et les instantanés

==Caractéristiques des nœuds==

nœuds	cœurs	mémoire disponible	stockage            	CPU                                             	GPU
338  	96   	768Go DDR5        	1 x NVMe SSD, 7.68To	2 x Intel Platinum 8568Y+ 2.3GHz, cache de 300Mo
22   	96   	1536GB DDR5       	1 x NVMe SSD, 7.68To	2 x Intel Platinum 8568Y+ 2.3GHz, cache de 300Mo
11   	64   	2048Go DDR5       	1 x NVMe SSD, 7.68To	2 x Intel Platinum 6548Y+ 2.5GHz, cache de 60Mo
16   	48   	1024Go DDR4       	1 x NVMe SSD, 3.84To	2 x Intel Gold 6342 2.8 GHz, 36MB cache         	4 x NVidia H100 PCIe Gen5 (94Go)
10   	48   	128GB DDR5        	1 x NVMe SSD, 3.84To	2 x Intel Gold 6542Y 2.9 GHz, cache de 60Mo     	1 x NVidia L40s PCIe Gen4 (48Go)

Voir le sommaire du matériel sur la page Ressources infonuagiques.