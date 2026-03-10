---
title: "LS-DYNA/fr"
url: "https://docs.alliancecan.ca/wiki/LS-DYNA/fr"
category: "General"
last_modified: "2026-01-09T09:27:02Z"
page_id: 16746
display_title: "LS-DYNA"
language: "fr"
---

= Introduction =
Le logiciel LS-DYNA est disponible sur toutes nos grappes. Il a plusieurs applications en multiphysique, en mécanique des fluides, en transfert thermique et en dynamique des fluides. Les analyses peuvent s'effectuer  sur des phénomènes distincts ou sur des simulations de phénomènes multiples comme le stress thermique ou l'interaction fluide-structure.  LSTC a été acquis par Ansys et LS-DYNA pourrait éventuellement être offert uniquement via le module Ansys. Pour le moment, nous recommandons l'utilisation que nous décrivons ici.

= Licence =
Nous fournissons l'hébergement pour LS-DYNA; le logiciel est installé sur nos grappes, mais nous n'avons pas une licence générique fournissant l'accès à tous, ni de service d'hébergement de licences. Cependant, plusieurs établissements, facultés et départements possèdent des licences qui peuvent être utilisées sur nos grappes. Avant d'utiliser ces licences, il peut être nécessaire d'effectuer des modifications de réseau pour garantir son accessibilité à partir des nœuds de calcul. Dans les cas où une licence a déjà été utilisée sur une grappe en particulier, ces modifications ont peut-être déjà été effectuées. Si vous ne parvenez pas à localiser ou à obtenir une licence de votre campus, contactez Microsystems. Les licences achetées auprès de CMC n'ont pas de frais généraux liés à l'hébergement d'un serveur de licences local puisqu'elles sont hébergées sur un système de serveur distant que CMC gère avec l'avantage supplémentaire d'être utilisables n'importe où. Si vous avez votre propre serveur et avez besoin d'un devis pour une licence gérée localement, vous pouvez contacter Simutech ou contacter Ansys directement. SHARCNET ne fournit actuellement pas de licence LS-DYNA gratuite, ni aucun service d'hébergement de licence.

=== Configuration initiale et test ===

Si votre serveur de licences n'a jamais  été utilisé sur la grappe où vos tâches seront exécutées, des modifications devront être apportées du côté de l'Alliance et de celui de l'établissement. Pour ce faire, faites parvenir à notre soutien technique le numéro du port et l'adresse IP de votre serveur de licence flottante. Pour vérifier le fonctionnement du fichier de licence, lancez

 module load ls-dyna
 ls-dyna_s or ls-dyna_d

Il n'est pas nécessaire de spécifier un fichier d'entrée ou des arguments pour exécuter ce test. L'entête de sortie doit contenir une valeur (non vide) pour Licensed to:, à l'exception des serveurs de licences CMC. Appuyez sur ^C pour quitter le programme et revenir en ligne de commande.

== Configuration de votre licence ==

Ansys a acheté en 2019 Livermore Software Technology Corporation (LSTC), développeur de LS-DYNA. Les licences LS-DYNA émises par Ansys depuis cette date utilisent des serveurs de licences Ansys. Les licences émises par LSTC peuvent toujours utiliser un serveur de licences LSTC. Une licence LS-DYNA peut aussi être obtenue de Microsystems. Nous expliquons ici comment configurer votre compte ou votre script de tâche dans chacun de ces cas.

=== Licence LSTC ===

Les options suivantes s'offrent à vous si vous avez une licence pour utilisation sur un serveur de licence LSTC.

Option 1) Spécifiez votre serveur de licence en créant un petit fichier nommé ls-dyna.lic ayant contenu suivant :

où  est un nombre entier et  est le nom d'hôte de votre serveur de licence LSTC. Placez ce fichier dans le répertoire $HOME/.licenses/ de chaque grappe sur laquelle vous prévoyez soumettre des tâches. Les valeurs du fichier sont récupérées par LS-DYNA lors de son exécution. Cela se produit parce que notre système de modules définit LSTC_FILE=/home/$USER.licenses/ls-dyna.lic chaque fois que vous chargez le module ls-dyna ou ls-dyna-mpi. Cette approche est recommandée si vous disposez d'une licence hébergée sur un serveur de licence LSTC car (par rapport à l'option suivante) les paramètres identiques seront automatiquement utilisés par toutes les tâches que vous soumettez sur la grappe, sans qu'il soit nécessaire de les spécifier dans chaque script ou de les définir dans votre environnement.

Option 2) Spécifiez votre serveur de licence en définissant les deux variables d'environnement suivantes dans vos scripts :
 export LSTC_LICENSE=network
 export LSTC_LICENSE_SERVER=@
où  est un nombre entier et  est le nom d'hôte ou l'adresse IP de votre serveur de licence LSTC. Ces variables auront la priorité sur toutes les valeurs spécifiées dans votre fichier ~/.licenses/ls-dyna.lic qui doit exister (même s'il est vide) pour que tout module ls-dyna ou ls-dyna-mpi soit correctement chargé; pour vous assurer qu'il existe, exécutez touch ~/.licenses/ls-dyna.lic en ligne de commande pour chaque grappe sur laquelle vous soumettrez des tâches. Pour plus de détails, consultez la documentation officielle.

=== Licence Ansys ===

Si votre licence LS-DYNA est hébergée sur un serveur de licence Ansys, définissez les deux variables d'environnement suivantes dans vos scripts :
 export LSTC_LICENSE=ansys
 export ANSYSLMD_LICENSE_FILE=@
où  est un nombre entier et  est le nom d'hôte ou l'adresse IP de votre serveur de licence Ansys. Ces variables ne peuvent pas être définies dans votre fichier ~/.licenses/ls-dyna.lic. Le fichier doit cependant exister (même s'il est vide) pour que tout module ls-dyna puisse se charger. Pour vous en assurer, exécutez touch ~/.licenses/ls-dyna.lic en ligne de commande (ou à chaque fois dans vos scripts). Notez que seules les versions de module >= 12.2.1 fonctionneront avec les serveurs de licence Ansys.

==== SHARCNET ====

La licence Ansys de SHARCNET prend en charge l'exécution de tâches SMP (Shared Memory Parallel) et MPP (Message Passing Parallel) de LS-DYNA. Elle peut être utilisée librement par n'importe qui (sur une base limitée au nombre de cœurs et de tâches) sur la grappe Nibi en ajoutant les lignes suivantes dans votre script :
 export LSTC_LICENSE=ansys
 export ANSYSLMD_LICENSE_FILE=1055@license1.computecanada.ca

=== Licence CMC ===

Si votre licence a été achetée de CMC, définissez les deux variables d'environnement suivantes selon la grappe utilisée :
 export LSTC_LICENSE=ansys
 Fir :      export ANSYSLMD_LICENSE_FILE=6624@172.26.0.101
 Nibi :     export ANSYSLMD_LICENSE_FILE=6624@10.25.1.56
 Narval :   export ANSYSLMD_LICENSE_FILE=6624@10.100.64.10
 Rorqual :  export ANSYSLMD_LICENSE_FILE=6624@10.100.64.10
 Trillium:  export ANSYSLMD_LICENSE_FILE=6624@scinet-cmc

où les différentes adresses IP correspondent aux serveurs CADpass respectifs. Aucune modification du pare-feu n'est requise pour utiliser une licence CMC sur une grappe, car elles ont déjà été effectuées. Étant donné que le serveur CMC distant qui héberge les licences LS-DYNA est basé sur Ansys, ces variables ne peuvent pas être définies dans votre fichier ~/.licenses/ls-dyna.lic. Le fichier doit cependant exister (même s'il est vide) pour que tout module ls-dyna puisse se charger. Pour vous assurer que c'est le cas, exécutez touch ~/.licenses/ls-dyna.lic en ligne de commande (ou à chaque fois dans vos scripts). Notez que seules les versions de module >= 13.1.1 fonctionneront avec les serveurs de licence Ansys.

= Soumettre des tâches sur une grappe =

LS-DYNA offre des binaires pour faire exécuter des tâches sur des nœuds uniques (SMP, Shared Memory Parallel avec OpenMP) ou sur plusieurs nœuds (MPP, Message Passing Parallel  avec MPI).  Vous trouverez ci-dessous des scripts pour chacun des types de tâches.

== Tâches avec un nœud unique  ==

Pour connaître les modules pour faire exécuter les tâches sur un nœud unique, utilisez module spider ls-dyna. Pour soumettre des tâches à la queue, utilisez  sbatch script-smp.sh. Le script suivant demande 8 cœurs sur un nœud de calcul unique.

Pour ce qui est de l’option AUTO de la variable d'environnement LSTC_MEMORY, ce paramètre permet d'étendre dynamiquement la mémoire au-delà du paramètre memory=1500M  spécifié lorsqu'il est adapté à une analyse explicite telle que les simulations de formage de métal, mais pas à une analyse de collision. Étant donné qu'il y a 4 octets/mot pour le solveur à simple précision et 8 octets/mot pour le solveur à double précision, le paramètre 1500M dans l'exemple ci-dessous équivaut soit à 1) une quantité maximale de (1500Mw*8octets/mot) = 12 Go de mémoire avant que LSDYNA s'arrête automatiquement lors de la résolution d'un problème implicite ou 2) une quantité de départ de 12 Go de mémoire avant de l'étendre (jusqu'à 25% si nécessaire) lors de la résolution d'un problème explicite en supposant que LSTC_MEMORY=AUTO  n'est pas commenté. Notez que 12 Go représentent 75% du total mem=16 Go réservé pour le travail et sont considérés comme étant parfaits pour les travaux implicites sur un seul nœud. En résumé, pour les analyses implicites et explicites, une fois qu'une estimation de la mémoire totale du solveur est déterminée en Go, le paramètre de mémoire totale pour l’ordonnanceur peut être déterminé en multipliant par 25% tandis que la valeur du paramètre de mémoire en mégamots peut être calculée comme (0,75*memGB/88octets/mot))*1000M et (0,75*memGB/4 octets/mot)*1000M pour les solutions à double et simple précision respectivement.

où
*ls-dyna_s = solveur smp simple précision
*ls-dyna_d = solveur smp double précision

== Tâches avec plusieurs nœuds ==

Plusieurs modules sont installés pour exécuter des tâches sur plusieurs nœuds à l'aide de la version MPP (Message Passing Parallel) de LS-DYNA. La méthode est basée sur mpi et peut s'adapter à de très nombreux cœurs (8 ou plus). Les modules peuvent être répertoriés en exécutant module spider ls-dyna-mpi. Les exemples de scripts ci-dessous montrent comment utiliser ces modules pour soumettre des tâches à un nombre spécifié de nœuds entiers *OU* à un nombre total spécifié de cœurs à l'aide de  sbatch script-mpp-bynode.sh ou sbatch script-mpp-bycore.sh  respectivement. La version MPP nécessite une quantité de mémoire suffisamment importante (memory1) pour que le premier cœur (processor 0) du nœud maître puisse décomposer et simuler le modèle. Cette quantité peut être satisfaite en spécifiant une valeur de mémoire par processeur légèrement supérieure à la mémoire (memory2) requise par cœur pour la simulation, puis en plaçant suffisamment de cœurs sur le nœud principal  pour faire en sorte que leur somme différentielle (mémoire par processeur moins memory2) soit supérieure ou égale à memory1. Comme avec le modèle à nœud unique, pour de meilleurs résultats, maintenez la somme de toute la mémoire attendue par nœud dans les 75 % de la RAM réservée sur un nœud. Ainsi, dans le premier script ci-dessous, en supposant un nœud de calcul de mémoire complète de 128 Go, mémoire1 peut être de 6 000 Mo (48 Go) maximum et mémoire2 de 200 Mo (48 Go/31 cœurs). (0,75*mémoireGo/4 octets/s)*1 000 Mo pour les solutions double précision et simple précision respectivement.

=== Spécifier le nombre de nœuds ===

Le script suivant demande un nombre spécifique de nœuds de calcul entiers.

où
*ls-dyna_s = solveur mpp simple précision
*ls-dyna_d = solveur mpp double précision

=== Spécifier le nombre de cœurs ===

Les tâches peuvent être soumises à un nombre arbitraire de nœuds de calcul en spécifiant le nombre de cœurs. Ceci permet à l'ordonnanceur de déterminer le nombre optimal de nœuds de calcul pour minimiser le temps d'attente dans la queue. Comme la limite de mémoire s'applique aux cœurs, la valeur de mem-per-cpu doit être assez élevée pour permettre au processeur principal de bien décomposer et gérer les calculs; pour les détails, référez-vous au premier paragraphe de la présente section.

où
*ls-dyna_s = solveur mpp simple précision
*ls-dyna_d = solveur mpp double précision

== Test de performance ==

Selon la simulation, LS-DYNA peut ne pas  pouvoir utiliser efficacement un très grand nombre de cœurs en parallèle. Il est donc conseillé de toujours exécuter des tests de scalabilité avant de soumettre des longues tâches. Ceci aidera à déterminer le nombre maximal de cœurs pouvant être utilisés avant que la performance ne commence à se dégrader. Pour extraire les statistiques des travaux de test telles que le temps d'exécution total, l'efficacité CPU et l'efficacité de la mémoire, on peut utiliser soit la commande seff jobnumber, soit un portail tel que celui-ci. Par le passé, les tests pour le problème standard des coussins gonflables ont montré des caractéristiques de performance très différentes selon la grappe sur laquelle ils étaient exécutés. Cependant, ces tests étaient assez petits, utilisant seulement 6 cœurs sur un seul nœud avec le module ls-dyna/12.2.1 et 6 cœurs répartis uniformément sur deux nœuds avec le module ls-dyna-mpi/12.2.1. Les tests de scalabilité devraient plutôt être effectués en utilisant la simulation réelle et la grappe où les exécutions de production complètes seront réalisées afin d'obtenir des résultats fiables.

= Mode graphique =

Le programme LS-PrePost permet le prétraitement et le post-traitement des modèles LS-DYNA. Il est disponible via un autre module et vous n'avez pas besoin de licence. Utilisez Abaqus en mode graphique sur un bureau à distance avec OnDemand (recommandé) ou VncViewer, comme décrit ci-dessous.

== Nœuds VDI ==
1. Avec le navigateur de votre ordinateur, connectez-vous à un système OnDemand avec l'une des URL suivantes :
 NIBI : https://ondemand.sharcnet.ca
 FIR : https://jupyterhub.fir.alliancecan.ca
 RORQUAL : https://jupyterhub.rorqual.alliancecan.ca
 TRILLIUM : https://ondemand.scinet.utoronto.ca
2. Sur votre ordinateur, ouvrez une nouvelle fenêtre de terminal et lancez
 module load StdEnv/2020
 module load ls-prepost/4.9
 lsprepost OU lspp49

== VncViewer  ==
1. Avec un client VncViewer, connectez-vous à un nœud de calcul ou à un nœud de connexion avec TigerVNC.

2. Sur votre ordinateur, ouvrez une nouvelle fenêtre de terminal et lancez
 module load StdEnv/2020
 module load ls-prepost/4.9
 lsprepost OR lspp49