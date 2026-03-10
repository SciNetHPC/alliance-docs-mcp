---
title: "DL POLY/fr"
url: "https://docs.alliancecan.ca/wiki/DL_POLY/fr"
category: "General"
last_modified: "2025-02-28T20:18:01Z"
page_id: 11154
display_title: "DL POLY"
language: "fr"
---

= Généralités =

DL_POLY est un logiciel classique de simulation en mécanique moléculaire. Sa conception permet de l’utiliser avec un ordinateur à processeur unique ou avec un ordinateur parallèle haute performance. DL_POLY_4 permet des opérations I/O entièrement parallèles et une alternative NetCDF (avec dépendance à une bibliothèque HDF5) aux fichiers de trajectoire ASCII par défaut.

Voir cette liste de diffusion

= Licence  =

DL_POLY est maintenant open source et il n'est pas nécessaire de vous enregistrer. Le nouveau module dl_poly4/5.1.0 est installé sous StdEnv/2023 et disponible à tous. Cependant, si vous voulez utiliser une version antérieure  (dl_poly4/4.10.0 et/ou dl_poly4/4.08), écrivez au  soutien technique et demandez de vous ajouter à un groupe POSIX qui contrôle l'accès à DL_POLY4. Il n'est pas nécessaire de vous enregistrer sur le site web de DL_POLY.

= Modules =
Pour connaître les versions disponibles, lancez module spider dl_poly4. La commande module est décrite dans la page Utiliser des modules.

Chargez la version 5.x avec

module load StdEnv/2023  intel/2023.2.1  openmpi/4.1.5 dl_poly4/5.1.0

Pour charger la version précédente 4.10.0, utilisez

module load StdEnv/2023 intel/2020.1.217  openmpi/4.0.3 dl_poly4/4.10.0

Prenez note que cette version doit être ajoutée à un groupe POSIX, comme décrit ci-dessus dans  Licence.

L’interface graphique Java n’est pas offerte.

= Scripts et exemples =

Les fichiers d’entrée CONTROL et FIELD proviennent de l’exemple TEST01 téléchargée à partir de DL_POLY examples.

Pour lancer une simulation, il faut au moins les trois fichiers suivants :

* CONFIG: boîte de simulation (coordonnées atomiques)
* FIELD: paramètres de champs de force
* CONTROL: paramètres de simulation (pas, nombre d’étapes, ensemble de simulation, etc.)

= Logiciels connexes =

* VMD
* LAMMPS