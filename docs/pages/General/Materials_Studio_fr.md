---
title: "Materials Studio/fr"
url: "https://docs.alliancecan.ca/wiki/Materials_Studio/fr"
category: "General"
last_modified: "2023-07-19T21:20:02Z"
page_id: 8331
display_title: "Materials Studio"
language: "fr"
---

L'Alliance n'a pas la permission d'installer Materials Studio de façon centrale sur toutes les grappes. Si vous disposez d'une licence, suivez ces directives pour installer l'application dans votre compte. Notez que ces directives sont valides pour les environnements logiciels moins récents; si vous utilisez présentement l'environnement 2020 par défaut, vous devrez utiliser une commande comme module load StdEnv/2016.4 avant de commencer (voir Environnements logiciels standards).

= Installation de Materials Studio 2020 =
Si vous avez accès à Materials Studio 2020, vous avez besoin
*du fichier archive BIOVIA_2020.MaterialsStudio2020.tar qui contient l'installeur;
*de l'adresse IP (ou le nom du DNS) et le port du serveur de licence préconfiguré auquel vous voulez vous connecter.

Téléversez maintenant le fichier BIOVIA_2020.MaterialsStudio2020.tar dans votre répertoire /home sur la grappe que vous voulez utiliser. Lancez ensuite les commandes
@}}
et
$HOME}}

Une fois que la commande est terminée, déconnectez-vous de la grappe et connectez-vous de nouveau. Vous devriez alors pouvoir charger le module avec

Pour avoir accès au serveur de licence à partir d'un nœud de connexion,  contactez le soutien technique pour que nous configurions nos pare-feu de façon à permettre au logiciel d'avoir accès à votre serveur de licence.

= Installation de Materials Studio 2018 =
Si vous avez accès à Materials Studio 2018, vous aurez besoin
* du fichier d'archive (MaterialsStudio2018.tgz) qui contient l'installeur,
* de l'adresse IP (ou du nom DNS) et du port d'un serveur de licence déjà configuré auquel vous voulez vous connecter.

Téléchargez ensuite le fichier MaterialsStudio2018.tgz dans votre répertoire  /home de la grappe et lancez les commandes
@}}
et
$HOME}}

Lorsque l'opération est terminée, déconnectez-vous de la grappe et connectez-vous de nouveau. Vous devriez alors pouvoir charger le module avec

Pour accéder au serveur de licence à partir d'un nœud de calcul, contactez le soutien technique pour que nos pare-feu soient configurés en conséquence.

== Installation pour un groupe ==
Si vous êtes chercheur principal et disposez d'une licence, vous n'avez qu'à installer l'application une fois pour tous les utilisateurs de votre groupe. Comme les travaux d'une équipe sont habituellement enregistrés dans l'espace /project, déterminez lequel des répertoires de cet espace vous voulez utiliser. Par exemple, s'il s'agit de ~/projects/A_DIRECTORY, vous devrez connaître ces deux valeurs ː

1. Déterminez le chemin pour A_DIRECTORY avec
 $(readlink -f ~/projects/A_DIRECTORY)|echo $PI_PROJECT_DIR}}
2. Déterminez le groupe pour A_DIRECTORY avec
 $(stat -c%G $PI_PROJECT_DIR)|echo $PI_GROUP}}

Avec ces deux valeurs, installez Materials Studio comme suit ː

# Remplacez le groupe par défaut par le groupe de l'équipe def-.

# Configurez les permissions pour donner accès au groupe.
# Créez un répertoire dans /project pour l'installation.
# Installez l'application. @ eb MaterialsStudio-2018-dummy-dummy.eb --installpath$PI_PROJECT_DIR/MatStudio2018 --sourcepath$HOME}}

Avant de lancer l'application ː

# Lancez la commande
#* Les membres de l'équipe peuvent ajouter ceci à leur fichier ~/.bashrc.
# Chargez le module materialsstudio.

NOTE : Assurez-vous de toujours remplacer les variables PI_GROUP et PI_PROJECT_DIR par les valeurs appropriées.

= Exemples de scripts pour l'ordonnanceur Slurm =
Les exemples suivants sont valides pourvu que vous ayez suivi les directives d'installation ci-dessus.

Le script suivant utilise la commande Materials Studio RunCASTEP.sh.

= Installation de versions antérieures =

Pour utiliser une version de Materials Studio antérieure à 2018, vous devez l'installer dans un conteneur Apptainer.
# Créez un conteneur Apptainer dans lequel est installée une distribution compatible de Linux.
# Installez Materials Studio dans ce conteneur.
# Téléversez le conteneur Apptainer vers votre compte.
#* NOTE : Pour accéder au serveur de licence à partir d'un nœud de calcul, contactez le soutien technique pour que nos pare-feu soient configurés en conséquence.

Comme il se peut que la version de MPI dans le conteneur ne puisse pas être utilisée sur plusieurs nœuds, il est possible que vos tâches soient limitées à des nœuds entiers (nœud unique).