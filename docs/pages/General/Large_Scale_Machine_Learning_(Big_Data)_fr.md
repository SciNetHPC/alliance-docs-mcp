---
title: "Large Scale Machine Learning (Big Data)/fr"
url: "https://docs.alliancecan.ca/wiki/Large_Scale_Machine_Learning_(Big_Data)/fr"
category: "General"
last_modified: "2023-11-28T21:02:52Z"
page_id: 19196
display_title: "Apprentissage machine à grande échelle (mégadonnées)"
language: "fr"
---

Dans le domaine de l’apprentissage profond, la scalabilité en termes de quantité de données est rendue possible par l'utilisation de stratégies de traitement en très petits lots et de solveurs itératifs du premier ordre. En apprentissage de réseaux de neurones profonds, le code s’effectue plus ou moins de la même manière qu’il s’agisse de quelques milliers ou de centaines de millions d’exemples : quelques exemples sont chargés à partir d’une source (disque, mémoire, source à distance, etc.), les gradients sont calculés pendant les itérations, ce qui modifie les paramètres du modèle au fur et à mesure. Par contre, avec plusieurs paquets d’apprentissage machine traditionnel, notamment scikit-learn, écrire du code pour faire de l'entraînement à large échelle n’est souvent pas évident. Pour plusieurs algorithmes qui conviennent à des modèles communs comme les modèles linéaires généralisés (GLM pour Generalized Linear Models) et les machines à vecteurs de support (SVM pour Support Vector Machines), leur implémentation par défaut exige que la totalité de l’ensemble d’apprentissage soit chargée en mémoire et n’offre aucune fonctionnalité de parallélisme par fils ou par processus. De plus, certaines de ces implémentations se basent sur des solveurs plutôt gourmands en mémoire qui, pour bien fonctionner, exigent une quantité de mémoire plusieurs fois supérieure à la taille de l’ensemble de données à entraîner.

Nous abordons ici des options permettant d’adapter les méthodes d’apprentissage machine traditionnel à de très grands ensembles de données dans les cas où un nœud de type Large Memory est insuffisant ou que le traitement séquentiel est excessivement long.

=Scikit-learn=

Scikit-learn  est un module Python pour l’apprentissage machine basé sur SciPy et distribué sous la licence BSD-3-Clause. Le paquet est doté d’une API intuitive qui simplifie la construction de pipelines complexes d’apprentissage machine. Toutefois, plusieurs implémentations des méthodes GLM et SVM supposent que l’ensemble d’entraînement est complètement chargé en mémoire, ce qui n'est pas toujours souhaitable. De plus, certains de ces algorithmes utilisent par défaut des solveurs très exigeants en mémoire. Dans certains cas, les suggestions qui suivent vous permettront de contourner ces limitations.

==Solveurs du gradient stochastique==

Si votre ensemble de données est assez petit pour être chargé au complet en mémoire, mais que pendant l’entraînement vous obtenez des erreurs de mémoire insuffisante (OOM pour Out-Of-Memory), le problème est probablement dû à un solveur trop gourmand en mémoire.  Avec  scikit-learn, plusieurs méthodes offrent en option des variations de l’algorithme du gradient stochastique et le remplacement du solveur par défaut par un solveur du gradient stochastique est souvent une solution facile.

Dans les exemples suivants, une régression de crête (ridge regression)  utilise le solveur par défaut et un solveur du gradient stochastique. Pour observer l’utilisation de la mémoire, lancez la commande htop, dans le terminal lorsque le programme Python s’exécute.

Une autre option qui réduit encore plus l'utilisation de la mémoire est de travailler avec SGDRegressor  plutôt qu’avec Ridge. Cette classe implémente plusieurs types de modèles linéaires généralisés (GLM) pour les régressions avec comme solveur l’algorithme du gradient stochastique (SGD). Il faut cependant noter que SGDRegressor ne fonctionne que si le résultat est unidimensionnel (scalaire).

==Apprentissage en lots==

Dans les cas où votre ensemble de données est trop grand pour la mémoire disponible, ou juste assez grand pour ne pas laisser assez de mémoire pour l'entraînement, il est possible de garder les données sur disque et de les charger en lots, comme c’est le cas avec les paquets d’apprentissage profond. Scikit-learn nomme cette technique out-of-core learning (apprentissage hors mémoire) et c’est une option viable quand l’estimateur offre la   méthode partial_fit. Dans les exemples ci-dessous, l’apprentissage hors mémoire se fait par des itérations sur des ensembles de données enregistrés sur disque.

Dans le premier exemple, nous utilisons SGDClassifier pour ajuster un classifieur linéaire SVM avec des lots de données en provenance d’une paire de vecteurs numpy. Les vecteurs sont stockés sur disque dans des fichiers npy qui seront mappés en mémoire. Puisque SGDClassifier possède la méthode partial_fit, les itérations peuvent se faire dans les grands fichiers en mémoire en ne chargeant que des petits lots à la fois en provenance des vecteurs. Chaque appel à partial_fit exécutera alors une époque de l’algorithme du gradient stochastique sur un lot de données.

Une autre méthode de stockage des données est d’utiliser des fichiers CSV. Dans le prochain exemple, l'entraînement d’un modèle de régression lasso se fait par la lecture en lots de données à partir d’un fichier CSV avec le paquet pandas.

=Snap ML=
Snap ML est une bibliothèque d’apprentissage machine propriétaire développée par IBM qui prend en charge plusieurs modèles classiques et s’adapte facilement à des ensembles de données contenant des milliards d’exemples et/ou de variables. Elle permet l’entraînement distribué, l’accélération de GPU et l’utilisation de structures creuses. Une de ses API est très semblable à celle de scikit-learn et peut la remplacer dans le cas d’ensembles de données massifs.

== Installation ==

===Wheels ajoutés récemment ===
Pour connaître la plus récente version de Snap ML que nous avons construite, lancez

Pour plus d'information, voir Wheels disponibles.

===Installer le wheel===

L'option à privilégier est d'utiliser le wheel Python comme suit :
:1. Chargez un module Python avec module load python.
:2. Créez et lancez un  environnement virtuel Python.
:3. Installez Snap ML dans l'environnement virtuel avec pip install.

:

==Multifil==

Tous les estimateurs de Snap ML prennent en charge le parallélisme par fils qui est contrôlé avec le paramètre n_jobs. En définissant ce paramètre comme étant égal au nombre de cœurs disponibles pour votre tâche, on peut typiquement observer une accélération en comparaison de l’implémentation du même estimateur avec scikit-learn. Voici comment se compare la performance de Ridge entre scikit-learn et Snap ML.

==Entraînement sur GPU==

Tous les estimateurs de Snap ML prennent en charge l'accélération d’un ou plusieurs GPU. Pour l’entraînement avec un GPU, le paramètre est use_gpu=True. Pour l’entraînement avec plusieurs GPU, le paramètre est aussi use_gpu, et la liste des ID des GPU disponibles est passée à device_ids. Par exemple, pour une tâche qui demande deux GPU, device_ids=[0,1] utilisera les deux GPU. Le prochain exemple fait la même comparaison que dans la section  précédente, mais pour l’entraînement d’un classifieur SVM avec un noyau non linéaire.

==Entraînement hors mémoire==

Tous les estimateurs de Snap ML utilisent par défaut des solveurs itératifs du premier ordre comme SGD. Il est donc possible de faire des entraînements en lots sans avoir à charger en mémoire les ensembles de données au complet. Par contre, Snap ML accepte les entrées de vecteurs numpy par le mappage en mémoire, contrairement à scikit-learn.

==MPI==

Snap ML offre des implémentations distribuées de plusieurs estimateurs. Pour utiliser le mode distribué, appelez un script Python avec mpirun ou srun.

=Spark ML=

Spark ML est une bibliothèque basée sur Apache Spark qui permet la scalabilité de plusieurs méthodes d’apprentissage machine à d’énormes quantités de données et sur plusieurs nœuds, sans avoir à distribuer des ensembles de données ou à créer du code distribué ou parallèle. Elle inclut plusieurs outils utiles en algèbre linéaire et en statistique. Avant de reproduire les exemples de la documentation Spark ML, voyez notre tutoriel sur comment soumettre une tâche Spark.