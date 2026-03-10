---
title: "Recovering data from a compromised VM/fr"
url: "https://docs.alliancecan.ca/wiki/Recovering_data_from_a_compromised_VM/fr"
category: "General"
last_modified: "2023-05-30T18:01:01Z"
page_id: 23160
display_title: "Récupération des données d'une machine virtuelle compromise"
language: "fr"
---

Page enfant de Service infonuagique

Vous êtes responsable de récupérer les données d'une machine virtuelle qui a été compromise.

L'information donnée ici n'est pas complète, mais vous saurez quoi faire dans une telle situation.

==Que se passe-t-il quand une machine virtuelle est compromise?==
# Ceci est confirmé par notre équipe de soutien technique après analyse des journaux du trafic et d'autres sources.
# La machine virtuelle est fermée au niveau sysadmin.
# Nous vous faisons parvenir un courriel à cet effet.

==Pourquoi devez-vous reconstruire la machine virtuelle?==
* Vous ne pouvez pas lancer une machine virtuelle qui a été verrouillée au niveau sysadmin.
* Le contenu de la machine virtuelle n'est plus intègre, mais il est relativement sécuritaire d'en extraire les données.
* Vous devez construire une nouvelle machine virtuelle.

==Comment devez-vous procéder?==
# Écrivez à nuage@tech.alliancecan.ca en expliquant votre plan de récupération. S'il est nécessaire d'accéder aux systèmes de fichiers, le volume sera déverrouillé par notre équipe de soutien technique.
# Connectez-vous à la console OpenStack.
# Lancez une nouvelle instance pour servir à la récupération.
# Dans Volumes, sélectionnez Gérer les attachements de la liste déroulante à droite du volume compromis et cliquez sur le bouton Détacher le volume.
# Dans Volumes, sélectionnez Gérer les attachements de la liste déroulante à droite du volume compromis et cliquez sur le bouton Attacher le volume.(sélectionnez l'instance que vous venez de lancer).
# Connectez-vous via ssh à la nouvelle instance. Le volume compromis est le disque vdb.
# Monter le bon système de fichiers à partir d'une partition ou d'un disque LVM (logical volume manager) dépend largement de comment l'image de base du système d'exploitation a été créée. Vous aurez besoin d'une personne d'expérience pour terminer la récupération de vos données.