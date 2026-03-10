---
title: "AlphaFold2/fr"
url: "https://docs.alliancecan.ca/wiki/AlphaFold2/fr"
category: "General"
last_modified: "2024-11-22T13:54:30Z"
page_id: 20595
display_title: "AlphaFold2"
language: "fr"
---

AlphaFold est un modèle d'apprentissage automatique pour la prédiction du repliement des protéines.

Nous expliquons ici comment utiliser la version AlphaFold v2.0 présentée au CASP14 et dont un compte-rendu a été publié dans Nature.

Le code source et la documentation se trouvent sur cette page GitHub.
Toute publication mentionnant des résultats obtenus par l'utilisation du code source ou des paramètres de modèle doit citer  cette publication.

== Versions disponibles ==
AlphaFold est disponible sur nos grappes dans les paquets Python préconstruits (wheels). Vous pouvez afficher les versions disponibles avec la commande avail_wheels.

== Installer AlphaFold dans un environnement virtuel Python ==

1. Chargez les dépendances d'AlphaFold.

Python 3.7 et 3.8 sont supportés depuis juillet 2022.

2. Créez et activez un environnement virtuel Python.

3. Installez une version spécifique d'AlphaFold et ses dépendances Python.
X.Y.Z
}}
où X.Y.Z représente le numéro de la version, par exemple 2.2.4.
Pour installer la plus récente version disponible pour nos grappes, n'indiquez pas de version.

4. Validez.

5. Gelez l'environnement et les dépendances.

== Bases de données ==
AlphaFold requiert un jeu de bases de données.

Les bases de données sont disponibles dans /cvmfs/bio.data.computecanada.ca/content/databases/Core/alphafold2_dbs/2023_07/.

Les bases de données AlphaFold  sont mises à jour annuellement dans CVMFS. La mise à jour de janvier 2024 est disponible dans le répertoire 2024_01.
/cvmfs/bio.data.computecanada.ca/content/databases/Core/alphafold2_dbs/2023_07/
}}

Vous pouvez aussi télécharger les bases de données localement dans votre répertoire /scratch.

Important : Les bases de données doivent se trouver dans le répertoire $SCRATCH.

1. Créez le répertoire de données à partir d'un nœud de connexion ou d'un nœud de transfert de données (DTN).
$SCRATCH/alphafold/data
|mkdir -p $DOWNLOAD_DIR
}}

2. Lorsque vos modules sont chargés et votre environnement virtuel est activé, vous pouvez télécharger les données.

Ceci ne doit pas se faire à partir d'un nœud de calcul. Sur les grappes qui ont des nœuds de transfert de données (DTN), utilisez ce type de nœud (voir la page Transfert de données); autrement, utilisez un nœud de connexion. Puisque la durée du téléchargement peut prendre une journée complète, nous vous suggérons d'utiliser plutôt un multiplexeur de terminal.
Il est possible que vous obteniez le message d'erreur Client_loop: send disconnect: Broken pipe; voir Dépannage ci-dessous.

1. Configurez DOWNLOAD_DIR.
/datashare/alphafold
}}

Par la suite, la structure des données sera semblable à ceci%nbsp;:

== Exécuter AlphaFold ==

Modifiez un des scripts de soumission suivants, selon vos besoins.

Soumettez la tâche à l'ordonnanceur.

== Dépannage ==
=== Message d'erreur Broken pipe ===
Au téléchargement de la base de données, vous pourriez obtenir le message d'erreur Client_loop: send disconnect: Broken pipe. Il est difficile de déterminer la cause exacte de ce message. Il pourrait tout simplement s'agir qu'un nombre élevé d'utilisateurs travaillent sur le nœud de connexion en même temps, ce qui vous laisse moins de place pour téléverser des données.

* Une solution est d'utiliser un  multiplexeur de terminal. Il n'est pas impossible d'obtenir le message d'erreur, mais les chances sont moindres.

*Une deuxième solution est d'utiliser la base de données qui se trouve sur la grappe. /cvmfs/bio.data.computecanada.ca/content/databases/Core/alphafold2_dbs/2023_07/.

*Une autre solution est de télécharger la base de données par sections. Pour avoir accès aux scripts de téléchargement après avoir chargé le module et activé votre environnement virtuel, il faut simplement entrer download_ dans votre terminal et taper deux fois sur la touche tab du clavier pour voir tous les scripts disponibles. Vous pouvez télécharger manuellement les sections de la base de données en utilisant le script disponible, par exemple  download_pdb.sh.