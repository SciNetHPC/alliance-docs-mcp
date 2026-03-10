---
title: "SpaCy/fr"
url: "https://docs.alliancecan.ca/wiki/SpaCy/fr"
category: "General"
last_modified: "2022-07-19T22:43:50Z"
page_id: 9579
display_title: "spaCy"
language: "fr"
---

spaCy est un paquet Python pour le traitement avancé du langage naturel.

= Installation =

==Wheels disponibles==

La commande suivante montre le plus récent build de spaCy.

Voyez  Lister les wheels disponibles.

==Wheels précompilés==

L’option privilégiée est de l’installer avec un wheel Python précompilé.
:1. Chargez le module python/3.6
:2. Créez et activez un  environnement virtuel.
:3. Installez spaCy dans l’environnement virtuel avec pip install.
:*Pour les CPU et les GPU :
:
:*Pour les CPU seulement :
:

Version GPU: Pour utiliser la version GPU, vous devez présentement ajouter les bibliothèques CUDA à la variable LD_LIBRARY_PATH:
$CUDA_HOME/lib64:$LD_LIBRARY_PATH
}}

Pour utiliser le script enveloppant (wrapper) Pytorch  avec thinc, vous devez aussi installer le wheel torch_cpu ou torch_gpu.