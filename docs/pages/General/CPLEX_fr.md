---
title: "CPLEX/fr"
url: "https://docs.alliancecan.ca/wiki/CPLEX/fr"
category: "General"
last_modified: "2023-03-24T16:25:44Z"
page_id: 6704
display_title: "CPLEX"
language: "fr"
---

CPLEX est un logiciel d'optimisation développé par IBM qui est disponible aux usagers universitaires via le programme Academic Initiative.

==Téléchargement==
Pour pouvoir utiliser CPLEX sur une grappe de Calcul Canada, vous devez d'abord vous inscrire avec IBM puis télécharger votre version personnelle du logiciel.  Si vous avez le choix entre plusieurs architectures, choisissez Linux x86-64.

==Installation==
Le fichier est une archive exécutable qui fera l'installation en posant quelques questions. Pour exécuter l'archive, il faut faire bash ./cplex_studioXYZ.linux-x86.bin.

Pour accéder au logiciel, vous pouvez créer des modules personnels. Les modules sont habituellement créés et mis dans une arborescence de répertories. Pour que vos modules soient trouvés, vous devez modifier le fichier de configuration $HOME/.bashrc pour pointer vers la racine de cette arborescence, en ajoutant la ligne suivante:

$HOME/modulefiles:$MODULEPATH}}

Ensuite, il faut créer une structure de répertoires pour y mettre votre nouveau module cplex:

Dans ce répertoire, vous allez créer un fichier (par exemple $HOME/modulefiles/mycplex/12.8.0) ayant le numéro de la version que vous avez téléchargé précédemment (le XYX) et qui contient ceci:

Ajustez les lignes qui correspondent aux variables cplexversion et studio_root afin qu'elles aient les bonnes valeurs : la version téléchargée et du chemin d'accès pour ce logiciel (c'est-à-dire le chemin que vous avez spécifié lors de l'extraction de l'archive).

==Java==
Si vous utilisez java, vous aurez quelques étapes supplémentaires. Tout d'abord, dans votre fichier .bashrc, vous pouvez ajouter la ligne,
.}}
qui permettra de trouver votre code lors de l'exécution.

Ensuite, vous devrez modifier la librairie dynamique de CPLEX. Cherchez cette librairie dans l'arborescence du répertoire d'installation, faites-en une copie et exécutez la commande:

Il est possible que lors de votre compilation, vous ayez un message d'erreur à cause d'un manque de mémoire. Si c'est le cas, vous devrez demander un noeud de calcul interactif pour pouvoir faire la compilation.
Par exemple:
1:0:0 --ntasks1 --mem-per-cpu8G}}

==Python==
Après avoir fait l'installation de CPLEX comme indiqué dans la section précédente, il faut tout d'abord charger CPLEX:

Pour installer les paquets de CPLEX tel que docplex, nous vous suggérons de le faire à partir d'un  environnement virtuel.

Une fois l'environnement virtuel activé, vous devrez aller dans le répertoire $STUDIO_ROOT/python et ensuite vous pouvez faire l'installation de la librairie avec la commande:

L'installation des paquets CPLEX doivent se faire sur le noeud de tête, puisqu'elles ne sont pas disponibles sur notre  pile logicielle.