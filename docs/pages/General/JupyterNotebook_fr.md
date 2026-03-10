---
title: "JupyterNotebook/fr"
url: "https://docs.alliancecan.ca/wiki/JupyterNotebook/fr"
category: "General"
last_modified: "2025-09-15T17:44:32Z"
page_id: 4749
display_title: "Jupyter Notebook"
language: "fr"
---

==Introduction==

Project Jupyter est un projet open source sans but lucratif dont la mission est de servir le calcul scientifique et la science des données interactives. Initié en 2014 dans le cadre du IPython Project, la portée de Project Jupyter s'étend à plusieurs autres langages de programmation.
http://jupyter.org/about.html

L'application web Jupyter Notebook rend possibles la création et le partage de documents contenant aussi bien du code, des équations et des visualisations que du texte.
http://www.jupyter.org/

Jupyter Notebook fonctionne sur un nœud de calcul ou sur un nœud de connexion (non recommandé). Dans le cas du nœud de connexion, diverses limites sont imposées tant pour l'utilisateur que pour les processus, et les applications sont parfois terminées quand elles utilisent trop de temps CPU ou de mémoire. Dans le cas du nœud de calcul, la tâche est soumise avec la spécification du nombre de CPU ou de GPU à utiliser, la quantité de mémoire et le temps d'exécution. Les directives qui suivent concernent la soumission d'une tâche Jupyter Notebook.

Autre information :
* Jupyter Notebook n'étant pas la plus récente interface de Jupyter, nous vous suggérons d'installer plutôt JupyterLab.
* Pour utiliser un environnement Jupyter préconfiguré, voyez la page Jupyter.

== Installation ==

Ces directives permettent d'installer Jupyter Notebook avec la commande pip dans un environnement virtuel Python dans votre répertoire personnel (home).  Les directives sont valides pour la version 3.6 de Python, mais vous pouvez installer l'application pour d'autres versions en chargeant le module Python approprié.

Chargez le module Python.

Créez un nouvel environnement virtuel Python.

Activez votre nouvel environnement virtuel Python.

Installez Jupyter Notebook dans votre nouvel environnement virtuel Python.

Dans votre nouvel environnement virtuel, créez un script (wrapper) pour lancer Jupyter Notebook.
$SLURM_TMPDIR/jupyter\njupyter notebook --ip $(hostname -f) --no-browser' > $VIRTUAL_ENV/bin/notebook.sh
}}
Enfin, rendez le script exécutable.

== Installer des modules d'extension ==

Les modules d'extension ajoutent des fonctionnalités et peuvent modifier l'interface utilisateur de l'application.

=== Jupyter Lmod ===

Jupyter Lmod est un module d'extension permettant d'interagir avec les modules d'environnement avant le lancement des noyaux (kernels). Il utilise l'interface Python de Lmod pour accomplir des tâches reliées aux modules comme le chargement, le déchargement, la sauvegarde des collections, etc.

=== Services web mandataires (proxy) ===

nbserverproxy permet d'accéder à des services web mandataires démarrés dans un serveur Jupyter. Ceci est utile dans le cas de services web qui n'écoutent que sur un port du serveur local, par exemple TensorBoard.

==== Exemple ====

Avec Jupyter, un service web est démarré via Terminal dans la liste déroulante New.

8008
}}

Le service est disponible via /proxy/ sur https://address.of.notebook.server/user/theuser/proxy/8008.

=== RStudio Launcher ===

Jupyter Notebook peut démarrer une session RStudio qui utilise le système d'authentification par jeton de Jupyter Notebook. RStudio Launcher crée l'option RStudio Session dans la liste déroulante New de Jupyter Notebook.

Remarque : la procédure suivante fonctionne uniquement avec les environnements logiciels StdEnv/2016.4 et StdEnv/2018.3.

== Activer l'environnement ==

Une fois que Jupyter Notebook est installé, vous n'aurez qu'à recharger le module Python associé à votre environnement lorsque vous vous connectez à la grappe.

Activez ensuite l'environnement virtuel dans lequel Jupyter Notebook est installé.

=== RStudio Server (optionnel) ===

Pour utiliser  RStudio Launcher, chargez le module RStudio Server.

== Lancer Jupyter Notebook ==

Pour lancer l'application, soumettez une tâche interactive. Ajustez les paramètres selon vos besoins. Pour plus d'information, consultez Exécuter des tâches.

1:0:0 --ntasks1 --cpus-per-task2 --mem-per-cpu1024M --accountdef-yourpi srun $VIRTUAL_ENV/bin/notebook.sh
|result=
salloc: Granted job allocation 1422754
salloc: Waiting for resource configuration
salloc: Nodes cdr544 are ready for job
[I 14:07:08.661 NotebookApp] Serving notebooks from local directory: /home/fafor10
[I 14:07:08.662 NotebookApp] 0 active kernels
[I 14:07:08.662 NotebookApp] The Jupyter Notebook is running at:
[I 14:07:08.663 NotebookApp] http://cdr544.int.cedar.computecanada.ca:8888/?token=7ed7059fad64446f837567e32af8d20efa72e72476eb72ca
[I 14:07:08.663 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 14:07:08.669 NotebookApp]

Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://cdr544.int.cedar.computecanada.ca:8888/?token=7ed7059fad64446f837567e3}}

== Se connecter à Jupyter Notebook ==

Puisque les nœuds de calcul ne sont pas directement accessibles par l'Internet, vous devez créer un tunnel SSH entre la grappe et votre poste de travail pour que votre fureteur web puisse avoir accès à Jupyter Notebook opérant sur un nœud de calcul.

=== Sous Linux ou macOS X ===

Nous recommandons le paquet Python sshuttle.

Sur votre poste de travail, ouvrez une nouvelle fenêtre terminal et lancez la commande sshuttle pour créer le tunnel.

Dans la commande précédente, remplacez  par votre nom d'utilisateur et  par la grappe à laquelle vous vous êtes connecté pour lancer Jupyter Notebook.

Puis copiez-collez l'adresse URL dans votre fureteur. Avec l'exemple précédent, le résultat serait

 http://cdr544.int.cedar.computecanada.ca:8888/?token=7ed7059fad64446f837567e3

=== Sous Windows ===

Pour créer un tunnel SSH, utilisez MobaXTerm comme suit, ce qui fonctionne aussi avec Unix (macOS, Linux, etc.).

Dans MobaXTerm, ouvrez un premier onglet Terminal (session 1) et connectez-vous à une grappe. Suivez ensuite les directives de la section  Lancer Jupyter Notebook ci-dessus. L'adresse URL suivante devrait s'afficher.

http://cdr544.int.cedar.computecanada.ca:8888/?token= 7ed7059fad64446f837567e3
       └────────────────┬───────────────────┘         └──────────┬───────────┘
              nom du serveur:port                              jeton

Dans MobaXTerm, ouvrez un second onglet Terminal (session 2). Dans la commande suivante, remplacez  par la valeur correspondante dans l'adresse URL obtenue à la session 1 (voir l'image précédente); remplacez  par votre nom d'utilisateur et; remplacez  par la grappe à laquelle vous vous êtes connecté à la session 1. Lancez la commande.

 Par votre fureteur, allez à

 http://localhost:8888/?token=

Remplacez  par la valeur obtenue à la session 1.

== Fermer Jupyter Notebook ==

Pour fermer le serveur Jupyter Notebook avant la fin du temps d'exécution, appuyez deux fois sur CTRL-C dans le terminal où la tâche interactive a été lancée.

Si le tunnel a été créé avec MobaXTerm, appuyez sur CTRL-D dans la session 2 pour fermer le tunnel.

==Ajouter des noyaux (kernels)==

Il est possible d'ajouter des noyaux pour d'autres langages de programmation ou pour des versions de Python différentes de celle dans laquelle fonctionne Jupyter Notebook. Pour plus d'information, consultez Making kernels for Jupyter.

L'installation se fait en deux étapes :
#Installation des paquets permettant à l'interpréteur de communiquer avec Jupyter Notebook.
#Création du fichier pour que Jupyter Notebook puisse créer un canal de communication avec l'interpréteur : il s'agit du fichier de configuration du noyau.

:Chacun des fichiers de configuration du noyau doit être créé dans son propre sous-répertoire dans un répertoire de votre répertoire personnel (home) par le chemin  ~/.local/share/jupyter/kernels. Jupyter Notebook ne crée pas ce fichier; dans tous les cas, la première étape est de le créer avec la commande .

Les prochaines sections présentent des exemples de procédures d'installation d'un noyau.

=== Julia ===

Chargez le module Julia.
Activez l'environnement virtuel Jupyter Notebook.
Installez IJulia.  julia}}

Pour plus d'information, consultez la documentation IJulia.

=== Python ===

Chargez le module Python.
Créez un nouvel environnement Python.
Activez le nouvel environnement Python.
Installez la bibliothèque ipykernel.
Générez le fichier de configuration du noyau.  Remplacez  par un nom unique pour votre noyau.
Désactivez l'environnement virtuel.

Pour plus d'information, voyez la  documentation ipykernel.

=== R ===

Chargez le module R.
Activez l'environnement virtuel Jupyter Notebook.
Installez les dépendances du noyau. 'http://cran.us.r-project.org')"}}
Installez le noyau R.
Installez le fichier de configuration du noyau  R.

Pour plus d'information, consultez la documentation IRKernel.

== Références ==