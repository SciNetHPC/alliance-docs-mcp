---
title: "Killarney/fr"
url: "https://docs.alliancecan.ca/wiki/Killarney/fr"
category: "General"
last_modified: "2025-11-03T21:01:57Z"
page_id: 28161
display_title: "Killarney"
language: "fr"
---

Disponibilité : 2025-06-09
Nœud de connexion :  killarney.alliancecan.ca
Collection Globus : [en préparation]
Page d'état : https://status.alliancecan.ca/system/Killarney

Killarney est une grappe qui répond aux besoins de la communauté scientifique canadienne en intelligence artificielle. Elle est située à l'Université de Toronto et gérée par  l'Institut Vecteur et SciNet. Son nom rappelle   le parc provincial Killarney qui se trouve près de la baie Georgienne, en Ontario.

Killarney fait partie de ECPIA, l'environnement de calcul pan-canadien pour l'intelligence artificielle.

==Particularités==
Killarney est présentement disponible pour les chercheuses principales et chercheurs principaux titulaires d'une chaire en intelligence artificielle (IACC) et affiliés à Vector, ainsi que celles et ceux qui sont dans un programme d'IA d'une université canadienne ou qui utilisent l'IA dans leurs travaux de recherche.

==Accès==
Demandez l'accès dans le portail CCDB.

Les chercheuses principales et chercheurs principaux doivent obtenir un RAP de type AIP (avec le préfixe aip-) de la part de leur établissement. Pour parrainer les personnes qui participent au projet RAP, la chercheuse principale ou le chercheur principal doit

* Faites afficher le tableau Projet(s) avec allocation de ressources dans CCDB.
* Cliquez sur le RAPI de votre projet AIP (préfixé aip-).
* Au bas de la page Détails pour le projet, cliquez sur Gerer l'appartenance aux projets.
* Entrez le ou les CCRI des personnes que vous voulez ajouter.

Dans le cadre de ses mesures de cybersécurité, Vector applique le blocage géographique à Killarney afin d'assurer l'intégrité et la sécurité. Vector restreint l'accès aux pays identifiés dans Évaluation des cybermenaces nationales 2025-2026 publié par le gouvernement du Canada.

==Matériel==

Performance          	Nœuds	Modèle     	CPU                      	Cœurs	Mémoire système	GPU par nœud            	Total de GPU
Calcul standard      	168  	Dell 750xa 	2 x Intel Xeon Gold 6338 	64   	512 GB         	4 x NVIDIA L40S 48GB    	672
Calcul de performance	10   	Dell XE9680	2 x Intel Xeon Gold 6442Y	48   	2048 GB        	8 x NVIDIA H100 SXM 80GB	80

==Stockage==

Le système de stockage est une plateforme NVME VastData avec une capacité utilisable de 1.7Po.

/home
* emplacement des répertoires /home
* quota fixe  pour chaque répertoire
* les demandes pour plus d'espace sont dirigées vers /project
* sauvegarde quotidienne
/scratch
* conçu pour le stockage actif ou temporaire
*  grand quota fixe par utilisateur
* les données inactives sont purgées
/project
* grand quota ajustable par projet
* sauvegarde quotidienne

==Réseautique==

* Nœuds de calcul standard : Infiniband HDR100, débit de 100Gbps
* Nœuds de calcul de performance : 2 x HDR 200, débit agrégé de 400Gbps

==Ordonnancement==
L'ordonnanceur Slurm exécute les tâches soumises par les utilisateurs. Les commandes Slurm de base sont semblables à celles pour les autres systèmes nationaux.

==Logiciel==
* Pile logicielle de modules.
* Pile logicielle standard de l'Alliance et logiciels particuliers à chaque grappe.