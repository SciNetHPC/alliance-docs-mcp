---
title: "Nibi/fr"
url: "https://docs.alliancecan.ca/wiki/Nibi/fr"
category: "General"
last_modified: "2026-01-21T13:13:31Z"
page_id: 27511
display_title: "Nibi"
language: "fr"
---

Disponibilité : 31 juillet 2025
Nœud de connexion SSH : nibi.alliancecan.ca
Nœud d'automatisation : robot.nibi.alliancecan.ca
Interface web : ondemand.sharcnet.ca
Collection Globus : alliancecan#nibi
Nœud de copie (rsync, scp, sftp, etc.) : utiliser les nœuds de connexion
Portail : portal.nibi.sharcnet.ca

Dans la langue anishinaabe, Nibi est un terme qui désigne l'eau. Cette nouvelle grappe offre 134 400 CPU et 288 GPU H100 de NVIDIA. Conçue par Hypertec Nibi est hébergée et exploitée par SHARCNET à l'Université de Waterloo.

=Stockage=
Stockage parallèle : 25 Po, SSD (Solid-State Drive) de VAST Data pour /home, /project et /scratch.

Notez que Vast comptabilise différemment l'espace utilisé pour calculer les quotas. La taille apparente de vos fichiers est prise en compte alors que certaines configurations de Lustre compressent les fichiers de manière transparente et comptabilisent l'espace utilisé après compression.

Notez également que Nibi utilise un nouveau mécanisme expérimental pour gérer /scratch. Comme sur tous les systèmes, vous disposez d'une limite souple et d'une limite stricte, mais sur Nibi, la limite souple est basse (1 TB) et vous disposez d'un délai de grâce de 60 jours. Après l'expiration de ce délai, la limite souple est imposée (plus aucune création ni extension de fichier). Pour résoudre ce problème, votre utilisation doit revenir sous la limite souple.

=Interconnexion=
* ethernet Nokia, 200/400 G
** bande passante pour nœuds CPU, 200 Gbit/s
** bande passante non bloquante pour tous les nœuds GPU Nvidia, 200 Gbit/s
** bande passante pour tous les nœuds GPU AMD, 200 Gbit/s
** connexion aux nœuds de stockage VAST, 24x100 Gbit/s
** liaisons montantes (uplinks) pour tous les nœuds,  400 Gbit/s; blocage 2:1

La topologie du réseau est décrite dans le fichier

 /opt/software/slurm/24.11.6/etc/topology.conf

Pour améliorer la performance des tâches multi-nœuds fortement couplées, vous pouvez forcer l'utilisation d'un seul commutateur (network switch) en ajoutant l'option suivante au script de la tâche.

 #SBATCH --switches=1

= Caractéristiques des nœuds =

nœuds	cœurs	mémoire disponible	stockage local au nœud	CPU                                                 	GPU
700  	192  	748G ou 766000M   	3 T              	2 x Intel 6972P @ 2.4 GHz, cache L3 384 Mo
10   	192  	6000G ou 6144000M 	3 T              	2 x Intel 6972P @ 2.4 GHz, cache L3 384 Mo
36   	112  	2000G ou 2048000M 	11 T             	2 x Intel 8570 @ 2.1 GHz, cache L3 300 Mo 	8 x Nvidia H100 SXM (mémoire de 80 Go), connexion via NVLink
6    	96   	495G ou 507000M   	3 T              	4 x AMD MI300A @ 2.1 GHz (Zen4+CDNA3)          	Les cœurs CPU et les GPU de l'architecture CDNA3 sont dans le même socket et partagent la même mémoire à ultra-haute performance.

=Particularités=
==Accès à l'internet==
Tous les nœuds ont accès à l'internet; aucune autorisation de pare-feu spéciale ou proxy n'est nécessaire.

==Espace /project==
Les répertoires des utilisateurs ne sont plus créés par défaut dans /project. Vous pouvez toujours créer vos propres répertoires dans l'espace /project du groupe à l'aide de mkdir. Ceci permet aux groupes de décider de l'organisation de leur espace /project pour le partage de données entre les membres.

==Quota pour l'espace /scratch==
Un quota souple de 1 TB sur /scratch s'applique à chaque utilisateur. Ce quota souple peut être dépassé pendant 60 jours maximum, après quoi aucun fichier supplémentaire ne peut être écrit sur /scratch. Les fichiers peuvent être réécrits une fois que l'utilisateur a supprimé suffisamment de fichiers pour ramener son utilisation /scratch totale sous 1 TB. Pour plus d'information, voir Stockage et gestion de fichiers.

==Accès via Open OnDemand (OOD)==
Il est possible d'accéder à la grappe Nibi simplement via un navigateur web. Nibi utilise Open OnDemand (OOD), une plateforme web qui simplifie l'accès en fournissant une interface web aux nœuds de connexion et un environnement de bureau à distance. Pour vous connecter à Nibi, rendez-vous sur https://ondemand.sharcnet.ca/ et connectez-vous avec l'authentification multifacteur. Une interface conviviale s'affichera, proposant des options pour ouvrir un terminal Bash ou lancer une session de bureau à distance.

==Utilisation de JupyterLab via OOD==

Vous pouvez exécuter JupyterLab de manière interactive via le portail Nibi Open OnDemand.

Option 1 : travailler dans un environnement préconfiguré, le même que pour JupyterHub

Quand la connexion au portail Nibi Open OnDemand est établie, cliquez sur Compute Node dans le menu du haut et sélectionnez Nibi JupyterLab. Une page sera affichée dans laquelle un formulaire vous permet de demander une nouvelle session Nibi JupyterLab.

Après avoir rempli le formulaire avec les détails, cliquez sur Launch pour soumettre votre demande. Quand l'état des modifications pour Nibi JupyterLab passe à Running, cliquez sur Connect to Jupyter pour ouvrir JupyterLab dans le navigateur web.

Pour les détails sur la préconfiguration, voir Interface JupyterLab.

Option 2 : travailler dans un
environnement virtuel Python que vous avez créé

Quand la connexion au portail Nibi Open OnDemand est établie, cliquez sur Compute Node dans le menu du haut et sélectionnez Compute Desktop. Une page sera affichée dans laquelle un formulaire vous permet de demander une nouvelle session Compute Desktop.

Après avoir rempli le formulaire avec les détails, cliquez sur Launch pour soumettre votre demande. Quand le bureau Compute passe à Running, cliquez sur Launch Compute Desktop pour vous connecter au bureau. Un bureau Linux sera affiché.

Sur le bureau Compute, faites un clic droit dans une zone vide; un menu contextuel apparaît. Sélectionnez Open in Terminal pour ouvrir une fenêtre de terminal où vous pouvez créer ou activer votre environnement virtuel Python dans lequel JupyterLab est installé.

Si JupyterLab n'est pas installé dans l'environnement virtuel Python que vous souhaitez utiliser, vous pouvez l'installer avec la commande

Vous pouvez ensuite lancer JupyterLab à partir de votre environnement virtuel Python avec

JupyterLab s'ouvre dans le navigateur sur le bureau et le contenu de votre espace $HOME est listé dans le panneau de gauche.

==Prise en charge de VDI via OOD==
Nibi n'offre plus d'infrastructure de bureau virtuel (VDI), mais fournit un environnement de bureau à distance via le portail Open OnDemand (OOD) avec des performances matérielles et une prise en charge logicielle améliorées.