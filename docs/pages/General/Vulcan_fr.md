---
title: "Vulcan/fr"
url: "https://docs.alliancecan.ca/wiki/Vulcan/fr"
category: "General"
last_modified: "2026-02-23T11:54:01Z"
page_id: 28493
display_title: "Vulcan"
language: "fr"
---

Disponibilité : 15 avril 2025
Nœud de connexion : vulcan.alliancecan.ca
Collection Globus : Vulcan Globus v5
État du système : https://status.alliancecan.ca/system/Vulcan
Portail : https://portal.vulcan.alliancecan.ca

Vulcan est une grappe au service de la communauté scientifique canadienne en intelligence artificielle. Elle est située à l'Université de l'Alberta et est gérée par l'Université de l'Alberta et Amii. La grappe porte le nom de la ville de Vulcan située dans le sud de l'Alberta.

Vulcan fait partie de ECPIA, l'environnement de calcul pan-canadien pour l'intelligence artificielle.

==Politiques spécifiques au site==
L'accès à Internet n'est généralement pas disponible à partir des nœuds de calcul. Un proxy Squid, disponible mondialement, est activé par défaut avec certains domaines sur la liste blanche. Contactez le soutien technique si vous ne parvenez pas à vous connecter à un domaine et nous évaluerons son ajout à la liste blanche.

La durée maximale d'une tâche est de sept jours.

Vulcan est présentement disponible à tous les chercheurs et chercheuses dont la recherche porte sur l'intelligence artificielle ou comporte des méthodes d'IA.

==Accès==
Demandez l'accès dans le portail CCDB.

Si vous êtes chercheuse principale ou chercheur principal et que vous parrainez d'autres personnes, vous devrez les ajouter à votre RAPI.
* Sur la page d'accueil de CCDB (https://ccdb.alliancecan.ca), faites afficher le tableau Projet d'allocation de ressources.
* Localisez votre projet AIP (préfixe aip-) et cliquez dessus pour faire afficher la page de gestion.
* Au bas de la page, cliquez sur Gérer l'appartenance au projet.
* Entrez le CCRI de la personne dans la section Ajouter des membres.

==Matériel==

Nœuds	Modèle     	CPU                      	Cœurs	Mémoire système	GPU par nœud        	Total de GPU
252  	Dell R760xa	2 x Intel Xeon Gold 6448Y	64   	512 GB         	4 x NVIDIA L40S 48GB	1800

==Système de stockage==

Le système de stockage est une combinaison de flash NVMe et HDD sur la plateforme Dell PowerScale, avec une capacité utilisable de 5PB. Les espaces /home, /scratch et /project sont sur le même système PowerScale.

/home
* emplacement des répertoires /home
* quota fixe pour chaque répertoire
* n'est pas alloué via le service d'accès rapide ou le concours pour l'allocation de ressources; les demandes pour plus d'espace sont dirigées vers /project
* sauvegarde quotidienne
/scratch
* stockage actif ou temporaire (/scratch)
* n'est pas alloué
* grand quota fixe par utilisateur
* les données inactives sont purgées
/project
* grand quota ajustable, par projet
* sauvegarde quotidienne

==Réseautique==

Les nœuds sont interconnectés via Ethernet 100Gbps avec le protocole RoCE (RDMA over Converged Ethernet) activé.

==Ordonnancement==
L'ordonnanceur Slurm exécute les tâches soumises à Vulcan. Les commandes Slurm de base sont semblables à celles pour les autres systèmes nationaux.

==Logiciel==
* Pile logicielle de modules.
* Pile logicielle standard de l'Alliance et logiciels particuliers à la grappe.