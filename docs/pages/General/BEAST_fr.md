---
title: "BEAST/fr"
url: "https://docs.alliancecan.ca/wiki/BEAST/fr"
category: "General"
last_modified: "2022-12-12T15:34:15Z"
page_id: 8996
display_title: "BEAST"
language: "fr"
---

== Description ==

BEAST est une application multiplateforme pour l’analyse bayésienne MCMC de séquences moléculaires, spécifiquement pour les phylogénies enracinées chronologiques inférées par des modèles d’horloge moléculaire stricte ou relaxée. On l’utilise comme méthode de reconstruction de phylogénies, mais BEAST est aussi un environnement de test pour les hypothèses sur l’évolution sans conditionnement d’une topologie arborescente. MCMC est utilisée pour faire la moyenne d’une partie d’un arbre pour que chacun des arbres reçoive un poids proportionnel à sa probabilité antérieure.

BEAST peut utiliser la bibliothèque hautement performante  beagle-lib pour effectuer les calculs à la base des bibliothèques phylogénétiques bayésiennes ou utilisant l’estimation du maximum de similitude (maximum likelyhood).

== Utilisation ==

Charger le module BEAST avec module load beast charge également les modules dépendants beagle-lib et java et configure la variable d’environnement EBROOTBEAST pour la diriger vers le répertoire qui contient les fichiers de l’application.

=== Extensions ===

BEAST est installé sans paquets d'extension. Pour les ajouter à votre répertoire /home, utilisez les commandes suivantes :
* packagemanager pour les versions à partir de 2.5.1;
* addonmanager pour les versions antérieures.

  $ module load beast/2.5.1
  $ packagemanager -list
  Name    | Installation Status | Latest Version | Dependencies | Description
  --------------------------------------------------------------------------
  BEAST   | 2.5.1               | 2.5.0          |              | BEAST core
  --------------------------------------------------------------------------
  bacter  | NA                  | 2.2.0          |              | Bacterial ARG inference.
  BADTRIP | NA                  | 1.0.0          |              | Infer transmission time for [...]
  [...]
  SNAPP   | NA                  | 1.4.1          |              | SNP and AFLP Phylogenies
  [...]

  $ packagemanager -add SNAPP
  Package SNAPP is installed in ~/.beast/2.5/SNAPP.

  $ packagemanager -list
  Name    | Installation Status | Latest Version | Dependencies | Description
  --------------------------------------------------------------------------
  BEAST   | 2.5.1               | 2.5.0          |              | BEAST core
  --------------------------------------------------------------------------
  [...]
  SNAPP   | 1.4.1               | 1.4.1          |              | SNP and AFLP Phylogenies
  [...]

  $ module load beast/2.4.0
  $ addonmanager -list
  Name    | Installation Status | Latest Version | Dependencies | Description
  ---------------------------------------------------------------------------
  BEAST   | 2.4.0               | 2.4.8          |              | BEAST core
  ---------------------------------------------------------------------------
  bacter  | not installed       | 1.2.3          |              | Bacterial ARG inference.
  BASTA   | not installed       | 2.3.2          |              | Bayesian structured coalescent approximation
  [...]
  SNAPP   | not installed       | 1.3.0          |              | SNP and AFLP Phylogenies
  [...]

  $ addonmanager -add SNAPP
  Package SNAPP is installed in ~/.beast/2.4/SNAPP.

  $ addonmanager -list
  Name    | Installation Status | Latest Version | Dependencies | Description
  ---------------------------------------------------------------------------
  BEAST   | 2.4.0               | 2.4.8          |              | BEAST core
  ---------------------------------------------------------------------------
  [...]
  SNAPP   | 1.3.0               | 1.3.0          |              | SNP and AFLP Phylogenies
  [...]

Pour plus d’information, voyez la section Server machines dans http://www.beast2.org/managing-packages/.

=== Script simple ===

=== Script demandant plus de mémoire ===

== Références ==
http://www.beast2.org/ BEAST