---
title: "Using a new empty volume on a Linux VM/fr"
url: "https://docs.alliancecan.ca/wiki/Using_a_new_empty_volume_on_a_Linux_VM/fr"
category: "General"
last_modified: "2024-07-15T19:37:58Z"
page_id: 6660
display_title: "Nouveau volume d'une instance sous Linux"
language: "fr"
---

Dans la plupart des environnements Linux, les étapes suivantes serviront à partitionner, formater et monter un nouveau volume.
Note : S'il ne s'agit pas d'un nouveau volume, n'exécutez pas le partitionnement et le formatage puisqu'une partie des données sera perdue; procédez uniquement au montage.

Partitionnez le volume avec

fdisk vous demande d'entrer une commande;  utilisez les caractères suivants :

 n => nouvelle partition
 p => primaire, une seule partition sur le disque
 1 => partition numéro 1
  => premier secteur (utiliser la valeur par défaut)
  => dernier secteur (utiliser la valeur par défaut)
 w => écrire la table de partition sur disque et quitter

Formatez la nouvelle partition avec

Créez l'endroit où monter le périphérique

Montez le volume avec

Si pour une raison quelconque l'instance est réinitialisée, le volume devra être monté à nouveau. Pour que l'instance remonte le volume automatiquement, modifiez /etc/fstab en ajoutant une ligne comme

  /dev/vdb1 /media/data ext4 defaults 0 2

La page fstab présente plus de renseignements. Si l'instance n'est pas réinitialisée, le nouveau périphérique ajouté à /etc/fstab peut être monté avec

==Démonter un volume ou autre périphérique==
Si vous devez démonter un volume ou un autre périphérique pour par exemple en créer une image ou pour l'attacher à une autre instance, il est préférable de le démonter d'abord afin d'éviter la corruption des données.

Pour démonter le volume que nous avons monté dans l'exemple ci-dessus, utilisez la commande

Pour que cette commande fonctionne, aucun fichier ne doit être utilisé en lecture ou en écriture par le système d'exploitation ou par une autre application active dans l'instance, autrement un message sera affiché indiquant que le volume est occupé.