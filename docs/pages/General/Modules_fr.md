---
title: "Modules/fr"
url: "https://docs.alliancecan.ca/wiki/Modules/fr"
category: "General"
last_modified: "2022-06-22T22:49:26Z"
page_id: 17116
display_title: "Modules"
language: "fr"
---

En programmation, un module est un logiciel indépendant et interchangeable qui contient tout ce qu'il faut pour fournir une certaine fonctionnalité.

 Voir Programmation modulaire sur Wikipédia.
Selon le contexte, le terme module peut avoir sens différent. Nous décrivons ici quelques types de modules et suggérons d'autres références de documentation.

== Précision ==

=== Modules Lmod ===

Aussi nommés modules d'environnement, les modules Lmod sont employés pour modifier votre environnement (shell) pour permettre l'utilisation d'un paquet logiciel ou d'une version d'un logiciel autre que celle offerte par défaut, par exemple pour les compilateurs (voir Utiliser des modules).

=== Modules Python ===

Un module Python est un fichier constitué habituellement de code Python qui peut être chargé avec les énoncés import ... ou from ... import .... Un paquet Python est une collection de modules Python; notez que les termes paquet et module sont souvent employés sans distinction. Voir What is the difference between a python module and a python package?

Certains modules Python tels que Numpy peuvent être importés si vous chargez d'abord le module Lmod scipy-stack au niveau du shell
(voir Pile logicielle SpiCy).

Nous offrons une importante collection de wheels Python
qui sont des des modules précompilés compatibles avec nos environnements logiciels standards.
Avant d'importer des modules d'un wheel, vous devez créer un environnement virtuel.

Les modules Python qui ne sont ni dans le module Lmod scipy-stack ni dans notre collection de wheels peuvent être installés à partir de l'internet tel que décrit dans Installer des paquets.

== Information complémentaire ==

*Page wiki Logiciels disponibles
*Environnements logiciels standards; par défaut, la collection de modules est StdEnv/2020 (depuis le 1er avril 2021)
*Modules Lmod particuliers sur Niagara
*Modules Lmod optimisés avec instructions CPU pour AVX, AVX2 and AVX512
* Page wiki Category Software : liste des pages de notre site wiki relatives aux logiciels du commerce ou offerts avec licence

== Références ==