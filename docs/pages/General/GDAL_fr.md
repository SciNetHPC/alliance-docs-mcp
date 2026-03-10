---
title: "GDAL/fr"
url: "https://docs.alliancecan.ca/wiki/GDAL/fr"
category: "General"
last_modified: "2025-02-11T16:24:26Z"
page_id: 24837
display_title: "GDAL"
language: "fr"
---

GDAL est une bibliothèque de traduction open source pour les formats de données géospatiales raster. Elle peut être utilisée comme une bibliothèque, car elle présente un modèle de données abstrait unique à l'application qui l’appelle, pour tous les formats pris en charge. Elle est également livrée avec une variété d'utilitaires de ligne de commande pour la traduction et le traitement des données.

GDAL est utilisée par une longue liste de progiciels
et ses fonctionnalités peuvent être utilisées dans des scripts écrits en Python ou R.

== Utiliser GDAL avec Python ==
La fonctionnalité GDAL peut être utilisée via le package osgeo que nous installons comme une extension du module GDAL. Pour l'utiliser, vous devez charger un module Python compatible avec le module GDAL.

=== Utiliser osgeo avec StdEnv/2020 ===
Pour connaître les modules Python qui sont compatibles avec par exemple gdal/3.5.1, utilisez le code suivant :

Nous avons donc le choix entre 3.8, 3.9 et 3.10. Nous choisissons python/3.10.

=== Utiliser osgeo avec StdEnv/2023 ===
Pour connaître les modules Python qui sont compatibles avec par exemple gdal/3.7.2, utilisez le code suivant :

Nous avons donc le choix entre 3.10 et 3.11. Nous choisissons python/3.11.

== Utiliser GDAL avec  R ==
Plusieurs paquets R pour l’analyse des données spatiales dépendent de GDAL pour leur fonctionnalités, par exemple
* sf: Simple Features for R
* terra: Spatial Data Analysis

L’ancien paquet rgdal a été abandonné et remplacé par sf et terra.

=== Installer sf et terra dans StdEnv/2020 ===
L'installation de ces paquets nécessite non seulement le chargement d'un module gdal, mais également de udunits requis par le paquet units.

=== Installer sf et terra dans StdEnv/2023 ===
Notez qu’avec StdEnv/2023, en plus des modules gdal et udunits, hdf/4.3.1 est également requis.