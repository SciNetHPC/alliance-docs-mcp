---
title: "Rorqual/fr"
url: "https://docs.alliancecan.ca/wiki/Rorqual/fr"
category: "General"
last_modified: "2026-01-14T13:03:02Z"
page_id: 26846
display_title: "Rorqual"
language: "fr"
---

Disponibilité : 19 juin 2025
Nœud de connexion : rorqual.alliancecan.ca
Nœud de copie (rsync, scp, sftp ...) : rorqual.alliancecan.ca
Nœud d'automatisation : robot.rorqual.alliancecan.ca
Collection Globus : alliancecan#rorqual
JupyterHub: jupyterhub.rorqual.alliancecan.ca
Portail : metrix.rorqual.alliancecan.ca
Webinaire : diapositives, vidéo

Rorqual est une grappe hétérogène et polyvalente conçue pour une grande variété de calculs scientifiques. Construite par Dell Canada et CDW Canada, Rorqual est située à l'École de technologie supérieure. Son nom rappelle le  rorqual, un mammifère marin dont plusieurs espèces, par exemple le petit rorqual et le rorqual bleu ont été observées dans les eaux du fleuve Saint-Laurent.

==Accès==
Pour accéder à la grappe de calcul, chaque chercheuse ou chercheur doit compléter une demande d'accès dans le formulaire que l'on trouve  via Ressources > Accès aux systèmes de la barre de menus de CCDB. Dans ce formulaire :

# Sélectionnez Rorqual dans la liste de gauche.
# Dans le premier encadré à droite, sélectionnez la demande d'accès.
# Acceptez ensuite toutes les ententes particulières avec Calcul Québec :
## Consentement pour la collecte et l'utilisation de renseignements personnels,
## Accord de niveau de service de Rorqual,
## Conditions d’utilisation.

L'accès effectif à la grappe peut prendre jusqu'à une heure après avoir complété la demande d'accès.

==Particularités==

Notre politique veut que les nœuds de calcul de Rorqual n'aient pas accès à l'internet. Pour y faire exception, veuillez joindre le soutien technique en expliquant ce dont vous avez besoin et pourquoi. Notez que l'outil crontab n'est pas offert.

Chaque tâche devrait être d'une durée d’au moins une heure (au moins cinq minutes pour les tâches de test) et vous ne pouvez pas avoir plus de 1000 tâches (en exécution et en attente) à la fois. La durée maximale d'une tâche est de 7 jours (168 heures).

==Stockage==

HOME  Système de fichiers Lustre, 116 To d’espace au total

* Cet espace est petit et ne peut pas être agrandi : vous devrez utiliser votre espace project pour les grands besoins en stockage.
* Petits quotas fixes par utilisateur
* Il y a une sauvegarde automatique une fois par jour.
SCRATCH  Système de fichiers Lustre, 6.5 Po d’espace au total

* Accessible via le lien symbolique $HOME/links/scratch
* Grand espace pour stocker les fichiers temporaires pendant les calculs.
* Pas de système de sauvegarde automatique
* Grands quotas fixes par utilisateur
* Il y a une  purge automatique des vieux fichiers dans cet espace.
PROJECT  Système de fichiers Lustre, 62 Po d’espace au total

* Accessible via le lien symbolique $HOME/links/projects/nom-du-projet
* Cet espace est conçu pour le partage de données entre membres d'un groupe et pour le stockage de beaucoup de données.
* Grands quotas ajustables par projet
* Il y a une sauvegarde automatique une fois par jour.

Au tout début de la présente page, un tableau indique plusieurs adresses de connexion. Pour les transferts de données par Globus, il faut utiliser le Point de chute Globus. Par contre, pour les outils comme rsync et scp, il faut utiliser l'adresse du Nœud de copie.

==Réseautique haute performance==
* Réseautique InfiniBand
** HDR 200Gbit/s
** Facteur de blocage maximum : 34:6 ou 5.667:1
** Taille des îlots de nœuds CPU : jusqu'à 31 nœuds de 192 cœurs pouvant communiquer sans blocage.

==Caractéristiques des nœuds==

nœuds	cœurs	mémoire disponible	stockage                       	CPU                                                    	GPU
670  	192  	750G ou 768000M   	1 x SSD SATA de 480Go (6Gbit/s)	2 x AMD EPYC 9654 (Zen 4) @ 2.40 GHz, cache L3 de 384Mo
8    	192  	750G ou 768000M   	1 x SSD NVMe de 3.84To         	2 x AMD EPYC 9654 (Zen 4) @ 2.40 GHz, cache L3 de 384Mo
8    	192  	3013G ou 3086250M 	1 x SSD SATA de 480Go (6Gbit/s)	2 x AMD EPYC 9654 (Zen 4) @ 2.40 GHz, cache L3 de 384Mo
81   	64   	498G ou 510000M   	1 x SSD NVMe de 3.84To         	2 x Intel Xeon Gold 6448Y @ 2.10 GHz, cache L3 de 60Mo 	4 x NVidia H100 SXM5 (mémoire 80Go), connectés via NVLink

* Pour obtenir un plus grand espace $SLURM_TMPDIR, il faut demander --tmp=xG, où x est une valeur entre 370 et 3360.

===Topologie des nœuds CPU===
Dans un nœud CPU, les 192 cœurs et les différents espaces mémoire ne sont pas équidistants, ce qui cause des délais variables (de l'ordre du nanoseconde) pour accéder aux données. Dans chaque nœud, on a :

* Deux (2) prises CPU (sockets) ayant chacune 12 canaux de mémoire système.
** Quatre (4) nœuds NUMA par prise CPU, chacun étant connecté à trois (3) canaux de mémoire système.
*** Trois (3) chiplets par nœud NUMA, chacun ayant sa propre mémoire cache L3 de 32 Mio.
**** Huit (8) cœurs par chiplet, chacun ayant sa propre mémoire cache L2 de 1 Mio et L1 de 32+32 Kio.

Autrement dit, on a :
* Des groupes de 8 cœurs rapprochés qui se partagent une même mémoire cache L3, ce qui est idéal pour des programmes parallèles multifils (par exemple, avec l'option --cpus-per-task=8)
* Des nœuds NUMA de 3×8 = 24 cœurs qui se partagent un trio de canaux de mémoire système.
* Un total de 2×4×3×8 = 192 cœurs par nœud.

Pour profiter pleinement des avantages de cette topologie, il faut réserver des nœuds complets (par exemple, avec --ntasks-per-node=24 --cpus-per-task=8) et contrôler explicitement l'emplacement des processus et des fils d'exécution. Selon le programme parallèle et le nombre de cœurs utilisés, les gains peuvent être marginaux ou significatifs.

===Topologie des nœuds GPU===
Dans les nœuds GPU, l'architecture est moins hiérarchique. On a :

* Deux (2) prises CPU (sockets). Pour chacune, on a :
** Huit (8) canaux de mémoire système
** 60 Mio de mémoire cache L3
** 32 cœurs équidistants ayant chacun sa propre mémoire cache L2 de 2 Mio et L1 de 32+48 Kio.
** Deux (2) accélérateurs NVidia H100

Au total, les quatre (4) accélérateurs du nœud sont interconnectés par SXM5.

===Instances GPU===

Les différents noms d'instances GPU disponibles sur Rorqual sont :

Modèle ou instance	Modèle ou instance	Nom court   	Sans unité	Par sa mémoire	Nom complet
GPU               	H100-80gb         	h100        	h100      	h100_80gb     	nvidia_h100_80gb_hbm3
MIG               	H100-1g.10gb      	h100_1g.10gb	h100_1.10 	h100_10gb     	nvidia_h100_80gb_hbm3_1g.10gb
MIG               	H100-2g.20gb      	h100_2g.20gb	h100_2.20 	h100_20gb     	nvidia_h100_80gb_hbm3_2g.20gb
MIG               	H100-3g.40gb      	h100_3g.40gb	h100_3.40 	h100_40gb     	nvidia_h100_80gb_hbm3_3g.40gb

Pour demander un ou plusieurs GPU H100 complets, il faut utiliser une des options Slurm suivantes :
* Un H100-80gb : --gpus=h100:1 ou --gpus=h100_80gb:1
* Plusieurs H100-80gb par nœud :
** --gpus-per-node=h100:2
** --gpus-per-node=h100:3
** --gpus-per-node=h100:4
* Plusieurs H100-80gb éparpillés n'importe où : --gpus=h100:n (remplacer n par le nombre voulu)

Environ la moitié des nœuds GPU de Rorqual sont configurés avec la technologie MIG et seulement trois tailles d'instances GPU sont disponibles :

* H100-1g.10gb : 1/8 de la puissance de calcul avec 10 Go de mémoire GPU.
* H100-2g.20gb : 2/8 de la puissance de calcul avec 20 Go de mémoire GPU.
* H100-3g.40gb : 3/8 de la puissance de calcul avec 40 Go de mémoire GPU.

Pour demander une et une seule instance GPU pour votre tâche de calcul, voici les options correspondantes :

* H100-1g.10gb : --gpus=h100_1g.10gb:1
* H100-2g.20gb : --gpus=h100_2g.20gb:1
* H100-3g.40gb : --gpus=h100_3g.40gb:1

Les quantités maximales recommandées de cœurs CPU et de mémoire système par instance GPU sont listées dans la table des caractéristiques des bundles.