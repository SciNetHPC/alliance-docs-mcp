---
title: "National systems/fr"
url: "https://docs.alliancecan.ca/wiki/National_systems/fr"
category: "General"
last_modified: "2025-08-29T12:45:02Z"
page_id: 1004
display_title: "Systèmes nationaux"
language: "fr"
---

==Grappes de calcul==

La plupart de nos grappes sont d'usage général et sont conçues pour l'exécution de plusieurs types de tâches. Elles comportent des nœuds qui présentent des caractéristiques différentes et qui sont classés selon trois groupes :
* nœuds de base (base nodes) qui ont typiquement environ 4Go de mémoire par cœur;
* nœuds de grande capacité (large-memory nodes) qui ont typiquement plus de 8Go par cœur;
* nœuds GPU (GPU nodes) qui ont des processeurs graphiques.

Toutes les grappes disposent de stockage de haute performance. Dans le tableau ci-dessous, cliquez sur le nom d'une grappe pour connaître les détails sur le nombre de nœuds disponibles, le nombre et les modèles de CPU et de GPU le stockage, la mémoire et le stockage.

===Liste des grappes de calcul===

Grappe  	Type                         	Sous-systèmes	État
Béluga  	Usage général
* beluga-compute
* beluga-gpu
* beluga-storage	En fin de vie
Cedar   	Usage général
* cedar-compute
* cedar-gpu
* cedar-storage	En fin de vie
Fir     	Usage général
* fir-compute
* fir-gpu
* fir-storage	En production
Graham  	Usage général
* graham-compute
* graham-gpu
* graham-storage	En fin de vie
Narval  	Usage général
* narval-compute
* narval-gpu
* narval-storage	En production
Niagara 	Tâches massivement parallèles
* niagara-compute
* niagara-storage
* hpss-storage	En fin de vie
Nibi    	Usage général
* nibi-compute
* nibi-storage
* nibi-storage	En production
Rorqual 	Usage général
* rorqual-compute
* rorqual-gpu
* rorqual-storage	En production
Trillium	Tâches massivement parallèles
* trillium-compute
* trillium-gpu
* trillium-storage	En production

==Nuage (IaaS)==
Notre service infonuagique est offert selon le modèle IaaS (Infrastructure as a Service) basé sur OpenStack.

Nuage        	Sous-systèmes	Description	État
Nuage Arbutus
* arbutus-compute-cloud
* arbutus-persistent-cloud
* arbutus-dcache
* vCPU, VGPU, RAM
* Disque local éphémère
* Stockage de volumes et instantanés
* Stockage sur des systèmes de fichiers partagés
* Stockage objet
* Adresses IP flottantes
* Stockage dCache	En production
Nuage Béluga
* beluga-compute-cloud
* beluga-persistent-cloud
* vCPU, RAM
* Disque local éphémère
* Stockage de volumes et instantanés
* Adresses IP flottantes	En production
Nuage Cedar
* cedar-persistent-cloud
* cedar-compute-cloud
* vCPU, RAM
* Disque local éphémère
* Stockage de volumes et instantanés
* Adresses IP flottantes	En production
Nuage Graham
* graham-persistent-cloud
* vCPU, RAM
* Disque local éphémère
* Stockage de volumes et instantanés
* Adresses IP flottantes	En production

==Grappes EIPIA==

Les grappes de l'Environnement informatique pancanadien de l’IA (EIPIA) sont des systèmes dédiés aux besoins émergeants de la communauté de recherche canadienne en intelligence articifielle.

Nom      	Institut        	État
TamIA    	Mila            	en production
Killarney	Institut Vecteur	en production
Vulcan   	Amii            	en production