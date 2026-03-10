---
title: "Advanced Jupyter configuration/fr"
url: "https://docs.alliancecan.ca/wiki/Advanced_Jupyter_configuration/fr"
category: "General"
last_modified: "2026-01-09T09:17:02Z"
page_id: 18581
display_title: "Jupyter : Configuration avancée"
language: "fr"
---

= Introduction =

* Project Jupyter est un projet open-source sans but lucratif issu en 2014 du IPython Project pour que tous les langages de programmation puissent être utilisés pour la science des données interactives et le calcul scientifique. Voir https://jupyter.org/about.html.
* JupyterLab est un environnement de développement web interactif pour les notebooks, le code et les données. La souplesse de son interface permet la configuration et l'utilisation des flux de travail en science des données, en calcul scientifique, en journalisme computationnel et en apprentissage automatique. Sa conception modulaire permet l'ajout d'extensions qui enrichissent ses fonctionnalités.Voir https://jupyter.org/.

Un serveur JupyterLab devrait toujours se trouver sur un nœud de calcul ou sur une instance infonuagique. Les nœuds de connexion ne sont pas un bon choix parce qu'ils imposent des limites qui peuvent interrompre une application qui consommerait trop de temps CPU ou de mémoire vive. Pour obtenir un nœud de calcul, vous pouvez réserver des ressources en soumettant une tâche qui demande un nombre prédéterminé de CPU ou de GPU, une certaine quantité de mémoire et un temps limite d'exécution. Nous décrivons ici comment configurer et soumettre une tâche JupyterLab sur nos grappes nationales.

Si vous recherchez un environnement Jupyter préconfiguré, consultez la page Jupyter.

= Installer JupyterLab =

Ces directives installent JupyterLab avec la commande pip dans un
environnement virtuel Python.

Si vous n'avez pas déjà un environnement virtuel Python, créez-en un, puis activez-le.

Chargez le module Python par défaut (comme démontré ci-dessous) ou chargez une version spécifique (voir les versions disponibles avec module avail python).
Si vous avez l'intention d'utiliser RStudio Server, chargez d'abord rstudio-server avec
Créez un nouvel environnement virtuel Python.
Activez le nouvel environnement virtuel.

Installez JupyterLab dans votre nouvel environnement virtuel (ceci prendra quelques minutes).
Dans l'environnement virtuel, créez un script enveloppeur (wrapper) pour le lancement automatique de JupyterLab.
Enfin, rendez ce script exécutable.

= Installer des modules d'extension =

Les modules d'extension ajoutent des fonctionnalités et peuvent modifier l'interface utilisateur de JupyterLab.

=== Jupyter Lmod ===

Jupyter Lmod est un module d'extension permettant d'interagir avec les modules d'environnement avant le lancement des noyaux (kernels). Il utilise l'interface Python de Lmod pour accomplir des tâches reliées aux modules comme le chargement, le déchargement, la sauvegarde des collections, etc.

Les commandes suivantes installeront et activeront l'extension Jupyter Lmod dans votre environnement (la troisième commande prendra quelques minutes).

Vous trouverez dans la page JupyterHub les directives pour gérer les modules chargés dans l'interface JupyterLab.

=== RStudio Server ===

RStudio Server vous permet de développer du code R dans un environnement RStudio, sous un onglet de votre navigateur. Il y a quelques différences avec la procédure d'installation de JupyterLab.

Avant de charger le module python et avant de créer un nouvel environnement virtuel, chargez le module rstudio-server.
Une fois que JupyterLab est installé dans le nouvel environnement virtuel, installez le serveur mandataire (proxy) Jupyter RSession.

Toutes les autres étapes de configuration et d'utilisation sont les mêmes. Vous devriez voir une application RStudio sous l'onglet Launcher.

= Utiliser votre installation =

== Activer l'environnement ==

Assurez-vous que l'environnement virtuel Python dans lequel vous avez installé JupyterLab est activé. Par exemple, quand vous vous connectez à la grappe, vous devez l'activer à nouveau avec
Pour vérifier que votre environnement est prêt, vous pouvez obtenir une liste des paquets jupyter* installés avec la commande grep jupyter
|result=
jupyter-client==7.1.0+computecanada
jupyter-core==4.9.1+computecanada
jupyter-server==1.9.0+computecanada
jupyterlab==3.1.7+computecanada
jupyterlab-pygments==0.1.2+computecanada
jupyterlab-server==2.3.0+computecanada
}}

== Lancer JupyterLab ==

Pour démarrer un serveur JupyterLab, soumettez une tâche interactive avec salloc. Ajustez les paramètres selon vos besoins. Pour plus d'information, voyez Exécuter des tâches.
1:0:0 --ntasks1 --cpus-per-task2 --mem-per-cpu1024M --accountdef-yourpi srun $VIRTUAL_ENV/bin/jupyterlab.sh
|result=
...
[I 2021-12-06 10:37:14.262 ServerApp] jupyterlab  extension was successfully linked.
...
[I 2021-12-06 10:37:39.259 ServerApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 2021-12-06 10:37:39.356 ServerApp]

    To access the server, open this file in a browser:
        file:///home/name/.local/share/jupyter/runtime/jpserver-198146-open.html
    Or copy and paste one of these URLs:
        http://node_name.int.cluster.computecanada.ca:8888/lab?token=101c3688298e78ab554ef86d93a196deaf5bcd2728fad4eb
     or http://127.0.0.1:8888/lab?token=101c3688298e78ab554ef86d93a196deaf5bcd2728fad4eb
}}

== Se connecter à JupyterLab ==

Pour avoir accès au serveur JupyterLab dans un nœud de calcul à partir de votre navigateur web, vous devez créer un tunnel SSH de votre ordinateur vers la grappe puisque les nœuds de calcul ne sont pas accessibles directement à partir de l'internet.

=== Sous Linux ou macOS ===

Nous recommandons l'utilisation du paquet Python sshuttle.

Sur votre ordinateur, ouvrez une nouvelle fenêtre de terminal et créez le tunnel SSH avec la commande sshuttle où vous remplacerez  par le nom d'utilisateur pour votre compte avec l'Alliance et  par la grappe sur laquelle vous avez lancé JupyterLab.

Copiez et collez la première adresse HTTP dans votre navigateur web; dans l'exemple salloc ci-dessus, ce serait

http://node_name.int.cluster.alliancecan.ca:8888/lab?token=101c3688298e78ab554ef86d93a196deaf5bcd2728fad4eb

=== Sous Windows ===

Pour créer un tunnel SSH à partir de Windows, utilisez MobaXTerm ou n’importe quel terminal qui supporte la commande ssh.

Une fois que JupyterLab est lancé sur un nœud de calcul (voir Lancer JupyterLab), vous pouvez extraire le hostname:port et le token de la première adresse HTTP fournie, par exemple
http://node_name.int.cluster.alliancecan.ca:8888/lab?token=101c368829...2728fad4eb
       └────────────────────┬────────────────────┘           └──────────┬──────────┘
                      hostname:port                                   token

Ouvrez un nouvel onglet Terminal dans MobaXTerm. Dans la commande suivante, remplacez  par la valeur correspondante (voir l'image ci-dessus); remplacez  par le nom d'utilisateur pour votre compte avec l'Alliance; remplacez  par la grappe sur laquelle vous avez lancé JupyterLab.
Ouvrez votre navigateur web et allez à l'adresse suivante, où  doit être remplacé par la valeur alphanumérique provenant de l'adresse illustrée ci-dessus.
http://localhost:8888/?token=

== Fermer JupyterLab ==

Pour arrêter le serveur JupyterLab avant la fin du temps d'exécution, appuyez deux fois sur CTRL-C dans le terminal où la tâche interactive a été lancée.

Si vous avez utilisé MobaXterm pour créer un tunnel SSH, appuyez sur Ctrl-D pour fermer le tunnel.

= Ajouter des noyaux =

Il est possible d'ajouter des noyaux pour d'autres langages de programmation, pour une version différente de Python ou pour un environnement virtuel persistant qui a tous les paquets et bibliothèques nécessaires à votre projet. Pour plus d'information, voyez Making kernels for Jupyter.

L'installation d'un nouveau noyau se fait en deux étapes :
# Installation des paquets qui permettent à l'interpréteur du langage de communiquer avec l'interface Jupyter.
# Création d'un fichier qui indique à JupyterLab comment amorcer un canal de communication avec l'interpréteur du langage. Ce fichier de configuration du noyau (kernel spec file) est sauvegardé dans un sous-répertoire de ~/.local/share/jupyter/kernels.

Les prochaines sections présentent des exemples de procédures d'installation d'un noyau.

== Noyau Julia ==

Prérequis :
# La configuration d'un noyau Julia dépend d'un environnement virtuel Python et d'un répertoire kernels. Si vous n'avez pas ces dépendances, assurez-vous de suivre les quelques premières directives dans la section Noyau Python ci-dessous (un noyau Python n'est pas requis).
# Puisque l'installation des paquets Julia nécessite un accès à l'internet, la configuration d'un noyau Julia doit se faire à l'invite de commande sur un nœud de connexion.

Une fois que l'environnement virtuel Python est disponible et activé, vous pouvez configurer le noyau Julia.

Chargez le module Julia.
Installez IJulia. julia
}}
Important : Avant d'utiliser le noyau Julia, démarrez ou redémarrez une nouvelle session JupyterLab.

Pour plus d'information, consultez la documentation sur IJulia.

=== Installer d'autres paquets Julia ===

Comme pour la procédure d'installation ci-dessus, il faut installer les paquets Julia à partir d'un nœud de connexion, mais l'environnement virtuel Python peut rester désactivé.

Assurez-vous que le même module Julia est chargé.
Installez les paquets nécessaires, par exemple Glob. julia
}}
Les paquets Julia nouvellement installés devraient être utilisés dans un notebook exécuté par le noyau Julia.

== Noyau Python ==

Dans un terminal avec une session active sur un serveur distant,
vous pouvez configurer un  environnement virtuel Python avec tous les modules Python  nécessaires et un noyau Python adapté à JupyterLab.
La configuration la plus simple de Jupyter dans un nouvel environnement virtuel Python se fait comme suit :

Si vous n'avez pas déjà un environnement virtuel Python, créez-en un, puis activez-le.

Commencez à partir d'un environnement Bash vierge (ceci n'est nécessaire que si vous utilisez le Terminal Jupyter via JupyterHub pour créer et configurer le noyau Python).$HOME bash -l
}}
Chargez un module Python.
Créez un nouvel environnement virtuel Python.
Activez le nouvel environnement virtuel.

Créez le répertoire commun kernels qui est utilisé par tous les noyaux que vous voulez installer.
Enfin, installez le noyau Python.

Installez la bibliothèque ipykernel.
Générez le fichier des spécifications du noyau. Remplacez  par un nom spécifique à votre noyau.

Important : Avant d'utiliser le noyau Python, démarrez ou redémarrez une nouvelle session JupyterLab.

Pour plus d'information, consultez la documentation IPython kernel.

=== Installer d'autres bibliothèques Python ===

Selon l'environnement virtuel Python configuré dans la section précédente :

Terminal Jupyter via JupyterHub, assurez-vous que l'environnement virtuel Python est activé et se trouve dans un environnement Bash vierge. Voir la section ci-dessus pour les détails.
Installez une bibliothèque qui serait requise, par exemple numpy.
Vous pouvez maintenant importer les bibliothèques Python dans un notebook exécuté par le Python 3.x Kernel.

==  Noyau R ==

Prérequis :
# La configuration d'un noyau R dépend d'un environnement virtuel Python et d'un répertoire kernels. Si vous n'avez pas ces dépendances, assurez-vous de suivre les quelques premières directives dans la section Noyau Python ci-dessus (un noyau Python n'est pas requis).
# Puisque l'installation de paquets R nécessite un accès à CRAN, la configuration d'un noyau R doit se faire à l'invite de commande sur un nœud de connexion.

Une fois que l'environnement virtuel Python est disponible et activé, vous pouvez configurer le noyau R.

Chargez un module R.
Installez les dépendances du noyau R, soit crayon, pbdZMQ et devtools; ceci pourrait prendre jusqu'à 10 minutes et les paquets devraient être installés dans un répertoire local tel que ~/R/x86_64-pc-linux-gnu-library/4.1.'http://cran.us.r-project.org')
}}
Installez le noyau R.
Installez le fichier des spécifications du noyau R.
Important : Avant d'utiliser le noyau R, démarrez ou redémarrez une nouvelle session JupyterLab.

Pour plus d'information, consultez la documentation IRkernel.

=== Installer d'autres paquets R ===

L'installation de paquets R ne peut se faire à partir de notebooks parce qu'il n'y a pas d'accès à CRAN. Comme dans la procédure d'installation ci-dessus, il faut installer les paquets R dans un nœud de connexion, mais l'environnement virtuel Python peut rester désactivé.

Assurez-vous que le même module R module est chargé.
Démarrez l'interpréteur R et installez les paquets requis. Voici un exemple avec doParallel :'http://cran.us.r-project.org')
}}
Les paquets R nouvellement installés devraient déjà pouvoir être utilisés dans un notebook exécuté par le noyau R.

= Exécution de notebooks en scripts Python =
Pour des tâches ou des analyses plus longues, soumettez une tâche interactive. Il faut alors convertir le notebook en un script Python, créer le script et le soumettre.

1. Dans un nœud de connexion, créez et activez un environnement virtuel, installez ensuite nbconvert si ce n'est pas déjà installé.

2. Convertissez le ou les notebooks en scripts Python avec

3. Créez le script et soumettez la tâche.

Dans le script de soumission, exécutez le notebook converti avec
python mynotebook.py

Soumettez votre tâche non interactive avec

= Références =