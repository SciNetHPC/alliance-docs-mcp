---
title: "Working with volumes/fr"
url: "https://docs.alliancecan.ca/wiki/Working_with_volumes/fr"
category: "General"
last_modified: "2023-03-30T21:38:08Z"
page_id: 21270
display_title: "Travailler avec des volumes"
language: "fr"
---

Un volume fournit un espace de stockage qui n'est pas détruit quand on termine l'instance à laquelle il est attaché.  Dans nos nuages, le stockage est assuré par Ceph, soit avec triple réplication, soit avec des codes d'effacement comme protection contre les défaillances de matériel. Dans Arbutus, le type de volume Default utilise des codes d'effacement et réduit les coûts de stockage additionnels de la triple réplication; le type de volume OS or Database utilise la triple réplication. Consultez la documentation OpenStack sur les volumes.

=Créer un volume=

Cliquez sur le bouton + Créer un volume et remplissez les champs comme suit :

*Nom du volume : par exemple, data
*Description : (optionnel)
*Source du volume : Aucune source, volume vide
*Type : Pas de type de volume
*Taille (Gio) : 40 ou toute autre valeur appropriée pour vos données ou votre système d'exploitation
*Zone de disponibilité : nova est la seule option disponible

Cliquez sur le bouton Créer un volume.

=Monter un volume sur une instance=
==Attacher un volume==

* Attacher un volume signifie l'associer à une instance. Ceci est comme insérer une clé USB ou ajouter un disque externe à votre ordinateur.
* Vous pouvez attacher un volume à partir de la page Volume du tableau de bord.
* Dans la colonne Actions, sélectionnez Gérer les attachements du menu déroulant.
* Dans le champ Attacher à l'instance, sélectionnez l'instance appropriée.
* Cliquez sur le bouton Attacher le volume.
Après quelques secondes, la page des volumes est rafraîchie et montre le nouveau volume lié à l'instance sélectionnée avec /dev/vdb ou autre endroit semblable.

==Formater un nouveau volume==
* NE FORMATEZ PAS si vous attachez un nouveau volume. Le volume a dû être formaté si vous l'avez déjà utilisé pour stocker des données.
* Le formatage efface toute l'information qui se trouve dans le volume, ce qui peut avoir d'importantes répercussions; la prudence est de mise.
* Formater un volume signifie le préparer pour y enregistrer des fichiers et des répertoires.
* Avant de pouvoir utiliser un nouveau volume, il faut le formater.
* Voyez les directives sur Linux ou Windows.

==Monter un volume==
* Monter un volume signifie créer une association logique entre le système de fichiers du volume d'une part et les répertoires et la structure des fichiers de l'instance d'autre part.
* Montez le volume avec une commande semblable à [name@server ~]$ sudo mount /dev/vdb1 /mnt selon le nom du dispositif, l'organisation du disque et le point de montage.
Le répertoire et la structure de fichiers du volume seront ainsi disponibles dans le répertoire /mnt de l'instance. Cependant, au redémarrage de l'instance, le volume devra être monté de nouveau avec la même commande mount.

Il est possible de monter automatiquement des volumes au démarrage d'une instance en ajoutant au fichier /etc/fstab une ligne qui contient les détails sur comment les monter.

Pour voir l'information, lancez la commande
blkid

Selon le UUID, ajoutez à /etc/fstab la ligne

/dev/disk/by-uuid/anananan-anan-anana-anan-ananananana /mnt auto defaults,nofail 0 3

Remplacez anananan-anan-anana-anan-ananananana par l'UUID du dispositif que vous voulez monter automatiquement.

Pour plus d'information, voyez cette page de la documentation Ubuntu.

=Démarrer depuis un volume=
Pour créer un service persistant, il est recommandé de démarrer l'instance depuis un volume. Une instance démarrée depuis une image plutôt que d'un volume est stockée sur le disque local de la machine qui opère cette même instance. L'instance pourrait être perdue si un problème survient sur la machine ou sur son disque. Le stockage du volume procure une redondance qui protège l'instance de défaillance du matériel. De façon générale, les gabarits pour démarrer depuis un volume commencent par la lettre p (voir la page Gabarits d'instances).

Démarrer une instance depuis un volume peut se faire
* à partir d'une image en créant un nouveau volume;
* à partir d'un volume existant;
* à partir d'un instantané (snapshot), en créant un nouveau volume.

Si vous faites ceci pour la première fois, utilisez la première option; les deux autres options ne sont possibles que si vous avez déjà créé un volume ou un instantané de volume.

Un volume peut être créé au lancement d'une instance. Sélectionnez Démarrer depuis une image (crée un volume). Remplissez ensuite les champs Nom de l'image et Taille du périphérique. Pour que le volume persiste après l'utilisation de l'instance, ne cochez pas la case Supprimer le volume lors de la suppression de l'instance, au bas de la fenêtre. Il est préférable de ne jamais cocher cette case puisque le volume peut être supprimé manuellement plus tard.

=Créer une image depuis un volume=

Créer une image depuis un volume permet de télécharger l'image pour servir de copie de sécurité ou pour créer une instance sur un autre nuage, par exemple avec VirtualBox. Pour copier un volume vers un autre volume dans le même nuage, procédez plutôt par clonage.

Pour créer l'image d'un volume, elle doit d'abord être détachée de l'instance. Dans le cas d'un volume de démarrage (boot volume), l'image ne peut être détachée que si l'instance est supprimée. Assurez-vous que la case Supprimer le volume lors de la suppression de l'instance n'a pas été cochée à la création de l'instance.

Les grandes images (plus de 10-20Go) peuvent prendre beaucoup de temps à créer, téléverser ou autres opérations. Une solution serait de  séparer les données si possible.

==Utiliser le tableau de bord==
# Sous Projet->Volumes, sélectionnez le volume.
# Dans la colonne Actions, sélectionnez Charger dans l'image du menu déroulant.
# Entrez un nom pour la nouvelle image.
# Sélectionnez le Format du disque. QCOW2 est recommandé pour le nuage OpenStack parce que ce format est relativement plus compact que Raw et plus efficace avec OpenStack. Si vous voulez utiliser l'image avec Virtualbox sélectionnez de préférence vmdk ou vdi.
# Cliquez sur le bouton Charger.

==Utiliser le client ligne de commande==
Un client ligne de commande peut faire ceci :

où
*  est le format du disque; les deux options sont  qcow2 etvmdk,
*  peut se trouver en cliquant sur le nom du volume à partir du tableau de bord OpenStack,
*  est le nom  que vous donnez à l'image.
Vous pouvez ensuite télécharger l'image.

=Cloner un volume=
Le clonage est le moyen recommandé pour la copie de volumes. Il est toujours possible de créer un nouveau volume depuis l'image d'un volume existant, mais le clonage est plus rapide et demande moins d'échange de données. C'est un moyen très utile si vous avez une instance persistante et que vous voulez faire des tests avant de passer en production. Nous recommandons fortement de terminer l’instance avant de cloner un volume pour éviter que l’état du volume cloné soit incohérent par rapport au volume source dans le cas où ce dernier aurait été modifié pendant la création du clone. Pour cloner un volume, utilisez un client ligne de commande et entrez

=Détacher un volume=
Avant de détacher un volume, il est important de vérifier si des fichiers de ce volume sont utilisés par le système d'exploitation ou des applications actives dans votre instance; si c'est le cas, le volume détaché pourrait être corrompu ou les applications pourraient avoir des comportements inattendus. Il est donc recommandé de fermer l'instance ou de démonter le volume.

Pour détacher un volume, connectez-vous à OpenStack (voir la liste des liens à nos ressources infonuagiques) et sélectionnez le projet qui contient le volume à détacher. Sélectionnez Volumes -> Volumes pour faire afficher les volumes. La colonne Attaché à indique ce à quoi chaque volume est attaché.

*Si la colonne indique /dev/vda, il s'agit d'un volume de démarrage; vous devez détacher l'instance avant de détacher le volume, autrement le message d'erreur  Impossible de déconnecter le volume sera affiché.

*Si la colonne indique /dev/vdb, /dev/vdc, etc., il n'est pas nécessaire de détacher l'instance. Dans la liste déroulante sous Actions, sélectionnez Gérer les attachements, cliquez sur le bouton Détacher le volume puis sur l'autre bouton  Détacher le volume pour confirmer.