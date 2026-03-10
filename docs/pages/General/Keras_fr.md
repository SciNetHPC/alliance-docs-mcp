---
title: "Keras/fr"
url: "https://docs.alliancecan.ca/wiki/Keras/fr"
category: "General"
last_modified: "2023-06-27T17:43:56Z"
page_id: 10014
display_title: "Keras"
language: "fr"
---

Keras est une bibliothèque open source écrite en Python qui permet d'interagir avec les algorithmes de réseaux de neurones profonds et d'apprentissage machine, notamment Tensorflow, CNTK et Theano.

Si vous voulez porter un programme Keras sur une de nos grappes, il serait bon de prendre connaissance du tutoriel sur le sujet.

==Installation==

#Installez TensorFlow, CNTK ou Theano dans un environnement virtuel Python.
#Activez l’environnement virtuel (dans notre exemple, $HOME/tensorflow).
#:
#Installez Keras dans l’environnement virtuel.
#:

=== Utilisation avec R ===

Pour installer Keras pour R avec TensorFlow comme application dorsale (backend) :

#Installez TensorFlow  suivant ces directives.
#Suivez les directives de la section parent.
#Chargez les modules nécessaires.
#:
# Lancez R.
#:
#Avec devtools, installez Keras dans R.
#:
devtools::install_github('rstudio/keras')

Puisque Keras et TensorFlow sont installés dans l’environnement virtuel avec  pip, n’utilisez pas install_keras().
Pour utiliser Keras, activez l’environnement virtuel et lancez les commandes

library(keras)
use_virtualenv(Sys.getenv('VIRTUAL_ENV'))

== Références ==

page Wikipédia sur Keras