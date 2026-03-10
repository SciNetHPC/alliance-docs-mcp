---
title: "Python/fr"
url: "https://docs.alliancecan.ca/wiki/Python/fr"
category: "General"
last_modified: "2026-02-27T08:19:36Z"
page_id: 2588
display_title: "Python"
language: "fr"
---

== Description ==
Python est un langage de programmation interprété dont la philosophie de design repose principalement sur la lisibilité du code. Sa syntaxe est simple et expressive et sa bibliothèque de modules standards est très étendue.

Les capacités du langage Python peuvent être étendues à l'aide de paquets développés par des tiers. En général, nous n'installons pas les paquets de tiers dans le répertoire de logiciels disponibles afin de simplifier le plus possible les opérations; il vous revient donc de les installer. En revanche, nous mettons à votre disposition plusieurs versions de l'interpréteur Python et les outils nécessaires pour que vous puissiez facilement installer les paquets dont vous avez besoin.

Les sections suivantes présentent l'interpréteur Python et expliquent comment installer et utiliser les paquets.

== Charger l'interpréteur ==

===Version par défaut===
Une version est disponible quand vous vous connectez à nos grappes, mais vous aurez souvent besoin d'une version différente, surtout si vous voulez installer des paquets. Trouvez la version de Python dont vous avez besoin et  chargez le module approprié. En cas de doute, vous pouvez utiliser la plus récente version disponible.mpi4py comme dépendance d'un autre paquet.

===Charger un module Python ===
Pour connaître les versions disponibles, utilisez

Vous pouvez ensuite charger la version de votre choix avec la commande module load, par exemple,

où X.Y représente la version, par exemple 3.13.

===Version prise en charge===
En règle générale dans l'écosystème Python, la transition vers des versions plus modernes s'accélère et plusieurs paquets ne supportent que les quelques dernières versions de Python 3.x. Dans notre cas, nous offrons uniquement des paquets préconstruits  (wheels Python) pour les trois versions les plus récentes disponibles sur nos systèmes. Des problèmes de dépendance se présenteront quand vous tentez d'installer ces paquets avec les plus anciennes versions de Python. Voir [[Python/fr#Message_Package_'X'_requires_a_different_Python:_X.Y.Z_not_in_'>=X.Y'|la section Dépannage]].

=== Pile logicielle SciPy===

En plus du module Python de base, le paquet SciPy est aussi disponible comme module d'environnement. Le module scipy-stack comprend
* NumPy
* SciPy
* Matplotlib
** dateutil
** pytz
* IPython
** pyzmq
** tornado
* pandas
* Sympy
* nose

Pour utiliser un de ces paquets, chargez une version de Python, puis module load scipy-stack.

Pour la liste et les numéros de version des paquets contenus dans scipy-stack, lancez module spider scipy-stack/2020a, en remplaçant 2020a par la version que vous voulez.

== Créer et utiliser un environnement virtuel ==

Avec chaque version de Python vient l'outil virtualenv qui permet de créer des environnements virtuels à l'intérieur desquels vous pourrez installer facilement vos paquets Python. Ces environnements permettent par exemple d'installer plusieurs versions d'un même paquet, ou encore de compartimenter les installations en fonction des besoins ou des expériences à réaliser. Vous devriez habituellement créer vos environnements virtuels Python dans votre répertoire /home ou dans un de vos répertoires /project. Pour une troisième option, voyez ci-dessous la section Créer un environnement virtuel dans vos tâches.

Pour créer un environnement virtuel, sélectionnez d'abord une version de Python avec module load python/X.Y.Z, comme indiqué ci-dessus dans Charger un module Python. Si vous voulez utiliser les paquets listés dans Pile logicielle SciPy, lancez aussi module load scipy-stack/X.Y.Z. Entrez ensuite la prochaine commande, où ENV est le nom du répertoire pour votre nouvel environnement.

Une fois l'environnement virtuel créé, il ne vous reste plus qu'à l'activer  avec

Vous devriez aussi faire la mise à jour de pip dans l'environnement.

Pour quitter l'environnement virtuel, entrez simplement la commande

Pour réutiliser l'environnement virtuel :
# Chargez les mêmes modules d'environnement que vous avez chargés quand l'environnement virtuel a été créé, soit module load python scipy-stack.
# Activez l'environnement avec source ENV/bin/activate.

=== Installer des paquets ===

Une fois que vous avez chargé un environnement virtuel, vous pouvez lancer la commande pip. Cette commande prend en charge la compilation et l'installation de la plupart des paquets Python et de leurs dépendances. Consultez  l'index complet des paquets Python.

Les commandes disponibles sont expliquées dans le manuel d'utilisation pip. Nous mentionnons ici les commandes les plus importantes en présentant un exemple d'installation du paquet NumPy.

Chargeons d'abord l'interpréteur Python avec

où X.Y représente la version, par exemple 3.13.

Ensuite, activons l'environnement virtuel créé précédemment avec la commande virtualenv.

Enfin, nous pouvons installer la dernière version stable de NumPy avec

La commande pip peut installer des paquets à partir de plusieurs sources, dont PyPI et les paquets de distribution préconstruits appelés  Python wheels. Nous fournissons des wheels Python pour plusieurs paquets. Dans l'exemple ci-dessus, l'option --no-index demande à pip de ne pas installer à partir de PyPI, mais plutôt de n'installer qu'à partir de paquets de source locale, soit de nos wheels.

Si un de nos wheels est disponible pour un paquet que vous voulez, nous vous recommandons fortement de l'utiliser avec l'option --no-index. Contrairement aux paquets de PyPI, les wheels compilés par notre personnel évitent les problèmes de dépendances manquantes ou conflictuelles et sont de plus optimisés pour nos grappes et nos bibliothèques. Voyez Wheels disponibles.

Si vous omettez l'option --no-index, pip cherchera les paquets PyPI et les paquets locaux et utilisera la version la plus récente. Si celle-ci est de PyPI, elle sera installée plutôt que la nôtre et vous aurez possiblement des problèmes. Si vous préférez télécharger un paquet PyPI plutôt que d'utiliser un wheel, utilisez l'option --no-binary qui demande à pip de ne considérer aucun paquet préconstruit; ainsi, les wheels distribués via PyPI ne seront pas considérés et le paquet sera toujours compilé de la source.

Pour savoir d'où provient le paquet Python installé par pip, ajoutez l'option -vvv. Lorsque vous installez plusieurs paquets Python, il est préférable de les installer en une seule étape, puisque pip peut alors résoudre les dépendances croisées.

===Créer un environnement virtuel dans vos tâches===

Attention : Sur Trillium, il est recommandé de créer un environnement virtuel sur un nœud de connexion dans /home et d'activer cet environnement (avec source) dans le script de la tâche.

Les systèmes de fichiers parallèles comme ceux qui sont installés sur nos grappes sont très efficaces lorsqu'il s'agit de lire ou d'écrire de grandes portions de données, mais pas pour une utilisation intensive de petits fichiers. Pour cette raison, le lancement d'un logiciel et le chargement de bibliothèques peuvent être lents, ce qui se produit quand on lance Python et qu'on charge un environnement virtuel.

Pour contrer ce genre de ralentissement, particulièrement pour les tâches Python sur un nœud unique, vous pouvez créer votre environnement virtuel à l'intérieur de votre tâche en utilisant le disque local du nœud de calcul. Il peut sembler déraisonnable de recréer votre environnement pour chacune de vos tâches, mais c'est souvent plus rapide et plus efficace que d'utiliser le système de fichiers parallèles. Il faut créer un virtualenv localement sur chacun des nœuds utilisés par la tâche puisque l'accès à virtualenv se fait par nœud. Le script suivant en est un exemple.

où le fichier requirements.txt aura été créé dans un environnement de test. Par exemple, pour créer un environnement pour TensorFlow, utilisez les commandes suivantes dans un nœud de connexion :
/tmp/$RANDOM
|virtualenv --no-download $ENVDIR
|source $ENVDIR/bin/activate
|pip install --no-index --upgrade pip
|pip install --no-index tensorflow
|pip freeze --local > requirements.txt
|deactivate
|rm -rf $ENVDIR
}}

Ceci produit le fichier requirements.txt dont le contenu ressemble à ceci :

Ce fichier fait en sorte que votre environnement puisse être reproduit pour les autres tâches.

Remarquez que les directives ci-dessus exigent que tous les paquets dont vous avez besoin soient disponibles dans les wheels Python que nous fournissons. Si ce n'est pas le cas, vous pouvez le prétélécharger (voir Prétélécharger des paquets ci-dessous). Si vous croyez que les wheels devraient être fournis, faites-en la demande au soutien technique

==== Créer un environnement virtuel dans vos tâches (plusieurs nœuds)====

Pour que vos scripts utilisent plusieurs nœuds, chacun doit avoir son propre environnement activé.

1. Dans votre script de soumission de la tâche, créez l'environnement virtuel pour chacun des nœuds alloués.

srun --ntasks $SLURM_NNODES --tasks-per-node=1 bash << EOF

virtualenv --no-download $SLURM_TMPDIR/env
source $SLURM_TMPDIR/env/bin/activate

pip install --no-index --upgrade pip
pip install --no-index -r requirements.txt

EOF

2. Activez l'environnement virtuel du nœud principal.
source $SLURM_TMPDIR/env/bin/activate;

3. Exécutez le script avec
srun python myscript.py;

==== Exemple (plusieurs nœuds) ====

=== Wheels disponibles ===
Les wheels présentement disponibles sont listés sur la page Wheels Python. Vous pouvez aussi utiliser la commande avail_wheels sur la grappe.
Par défaut, cette commande montre seulement
* la plus récente version d'un paquet en particulier, à moins qu'une version particulière n'ait été spécifiée;
* les versions compatibles avec le module Python chargé ou l'environnement virtuel activé; autrement, toutes les versions sont affichées;
* les versions compatibles avec l'architecture CPU et l'environnement logiciel (StdEnv) que vous utilisez à ce moment.

==== Noms ====
Pour la liste des wheels qui contiennent cdf (insensible à la casse) dans leur nom, lancez

ou utilisez le nom exact, par exemple

==== Version ====
Pour la liste d'une version particulière, vous pouvez utiliser le même format qu'avec pip :
1.23
|result=
name    version    python    arch
------  ---------  --------  -------
numpy   1.23.0     cp39      generic
numpy   1.23.0     cp38      generic
numpy   1.23.0     cp310     generic
}}
ou employer la version plus longue, comme

Avec le format pip, vous pouvez utiliser différents opérateurs : ==, <, >, ~=, <=,>=, !=, pour lister par exemple les versions précédentes,

et pour lister toutes les versions disponibles,

==== Python ====
Pour lister une version particulière de Python, lancez

La colonne python montre la version de Python pour laquelle le wheel est disponible, où cp39 est utilisé pour cpython 3.9.

==== Fichier de requis ====
Pour savoir si les wheels disponibles incluent ceux qui sont indiqués dans le fichier requirements.txt, lancez

Pour la liste de ceux qui ne sont pas disponibles, la commande est

=== Prétélécharger des paquets ===

La procédure suivante prétélécharge le paquet tensorboardX sur un nœud de connexion et l'installe sur un nœud de calcul :

# Lancez pip download --no-deps tensorboardX pour télécharger le paquet tensorboardX-1.9-py2.py3-none-any.whl (ou semblable) dans le répertoire de travail. La syntaxe pour pip download est la même que celle pour pip install.
# Si le nom du fichier ne se termine pas par none-any, mais par linux_x86_64 ou manylinux*_x86_64, il est possible que le wheel ne fonctionnera pas correctement. Contactez le soutien technique pour que nous le compilions et qu'il soit rendu disponible sur nos superordinateurs.
# À l'installation, utilisez le chemin du fichier pip install tensorboardX-1.9-py2.py3-none-any.whl.

=== Installer à partir d'un dépôt à distance (Github) ===

Dans certains cas, le paquet source ne se trouve pas dans l'index PyPI, mais est disponible dans un autre dépôt distant.
Ce dépôt distant peut être Git, Subversion, Bazaar ou Mercurial; nous traitons ici de Git.

En utilisant l'URL pour le dépôt distant, vous pouvez spécifier
* un nom de branche (the-best-feature)
* un tag (v1.0.1)
* un identifiant de validation court ou long (da39a3ee5e6b4b0d3255bfef95601890afd80709)
* une référence, par exemple une requête pull (refs/pull/123/head)

Dans un environnement virtuel activé,

Il est important d'utiliser un tag (version) ou un ID de validation pour obtenir une installation pouvant être reproduite.
Si vous utilisez le HEAD du dépôt, cela pourrait fonctionner mais vous risquez d'avoir éventuellement des problèmes puisque des modifications ont été faites.

Sur Github, les tags ou versions se trouvent dans la section Releases dans le panneau de gauche.

Pour plus d'information sur l'installation à partir d'un système de contrôle de versions (VCS), voir  vcs-support.

== Programmation parallèle avec le module multiprocessing ==

La programmation parallèle avec Python est un moyen facile d'obtenir des résultats plus rapidement, ce qui est habituellement accompli avec l'utilisation du module multiprocessing. La classe Pool de ce module est particulièrement intéressante car elle permet de contrôler le nombre de processus lancés en parallèle pour exécuter le même calcul avec des données multiples. Supposons que nous voulons calculer le cube d'une liste de nombres; le code série serait semblable à :

Avec la classe Pool le code parallèle devient :

Dans les exemples précédents, nous sommes toutefois limités à quatre processus. Avec une grappe, il est très important d'utiliser les cœurs qui sont alloués à la tâche. Si le nombre de processus exécutés dépasse le nombre de cœurs demandé pour la tâche, les calculs s'effectueront plus lentement et le nœud de calcul sera possiblement surchargé. Si le nombre de processus exécutés est inférieur au nombre de cœurs demandé, certains cœurs resteront inactifs et les ressources ne seront pas utilisées de façon optimale. Votre code devrait faire appel à autant de cœurs que la quantité de ressources demandées à l'ordonnanceur. Par exemple, pour exécuter le même calcul sur des dizaines de données ou plus, il serait sensé d'utiliser tous les cœurs d'un nœud. Dans ce cas, le script de soumission de la tâche aurait l'en-tête suivant :

Le code serait alors :

Remarquez que dans cet exemple, la fonction cube est en elle-même séquentielle. Il est possible qu'une fonction appelée d'une bibliothèque externe comme numpy soit en elle-même parallèle. Pour distribuer des processus avec la technique précédente, vérifiez d'abord si les fonctions appelées sont en elles-mêmes parallèles et si c'est le cas, vous devrez contrôler le nombre de fils qu'elles utiliseront. Si comme dans l'exemple les fonctions utilisent la totalité des cœurs disponibles (ici 32) et que vous lancez 32 processus, votre code sera plus lent et le nœud sera possiblement surchargé.

Comme le module multiprocessing ne peut utiliser qu'un seul nœud de calcul, le gain en performance est habituellement limité au nombre de cœurs CPU du nœud. Si vous voulez dépasser cette limite et utiliser plusieurs nœuds, considérez mpi4py ou PySpark. Il existe d'autres méthodes de parallélisation, mais elles ne peuvent pas toutes être utilisées avec nos grappes. Souvenez-vous toutefois qu'un code de qualité fournira toujours la meilleure performance; avant de le paralléliser, assurez-vous donc que votre code est optimal. Si vous doutez de l'efficacité de votre code, contactez le soutien technique.

== Anaconda ==
Voir la page sur Anaconda.

== Jupyter ==
Voir la page sur Jupyter Notebook.

== Débogage ==

Déboguer votre code Python n'est pas toujours évident. Des méthodes simples, comme l'ajout d'un print ou d'une assertion (assert), peuvent vous aider à corriger certaines erreurs.

Toutefois, il est souvent nécessaire d'explorer le code et son contexte plus en profondeur; l'utilisation d'un débogueur comme pdb est alors plus simple.

Vous pouvez déboguer votre code Python grâce à une petite tâche interactive.

# Ajoutez import pdb; pdb.set_trace() au début de votre fichier ou ajoutez breakpoint() à l'endroit approprié.
# Exécutez votre code (python ...).
# Vous vous trouverez alors dans le débogueur où vous pourrez analyser et évaluer les expressions.

Commandes utiles

Commande         	Description
w (pour where)   	Affiche une trace de la pile d'exécution, avec la plus récente fenêtre dans le bas. Une flèche (>) indique la fenêtre actuelle, ce qui détermine le contexte de la plupart des commandes.
b (pour break)   	Avec un argument lineno, insère un saut au numéro de ligne dans le fichier actuel.
s (pour step)    	Exécute la ligne actuelle, s'arrête à la première occasion (soit dans une fonction appelée, soit sur la ligne suivante dans la fonction actuelle).
n (pour next)    	Continue l'exécution jusqu'à ce que la ligne suivante de la fonction actuelle soit atteinte ou qu'elle retourne à la ligne précédente.
r (pour return)  	Continue l'exécution jusqu'à la fin de la fonction actuelle.
c (pour continue)	Continue l'exécution et s'arrête uniquement à un point d'arrêt (breakpoint).
p exp            	Évalue l'expression dans le contexte actuel et affiche sa valeur.
l (pour list)    	Affiche le code source du fichier actuel.
q (pour quit)    	Quitte le débogueur. Le programme en cours d'exécution est arrêté.

En général, on utilise w, s, l, p, n pour déboguer un fichier.

Pour plus d'information, voir  The Python Debugger.

== S'attacher à un processus en cours ==
Avec Python 3.14 et versions plus récentes, il est possible de s'attacher à un processus en cours et de lancer PDB à l'étape en cours. Dans un autre terminal, lancez

== Dépannage ==

=== Script Python suspendu ===

Avec le module faulthandler, vous pouvez modifier votre script pour qu'une trace de l'origine du problème soit fournie après une certaine durée; voir l'information sur la commande faulthandler.dump_traceback_later(timeout, repeat=False, file=sys.stderr, exit=False).

Vous pouvez aussi inspecter un processus Python pendant l'exécution d'une tâche sans avoir à le modifier au préalable avec py-spy :

# Installez py-spy dans un environnement virtuel de votre répertoire /home.
# Connectez-vous à une tâche en cours avec srun --pty --jobid JOBID bash.
# Trouvez l'ID de la tâche du script Python avec htop -u $USER.
# Activez l'environnement virtuel où py-spy est installé.
# Lancez py-spy top --pid PID pour visionner en direct les endroits où le code utilise beaucoup de temps.
# Lancez py-spy dump --pid PID pour obtenir une trace de l'état de votre code.

===Package 'X' requires a different Python: X.Y.Z not in '>=X.Y' ===
En installant un paquet, vous pourriez avoir une erreur comme ERROR: Package 'X' requires a different Python: 3.6.10 not in '>=3.7'.

Dans ce cas, le module Python 3.6.10 qui est chargé n'est pas supporté par le paquet. Vous pouvez utiliser une version de Python plus récente, comme le dernier module disponible, ou encore installer une version moins récente du paquet X.

=== Package has requirement X, but you'll have Y which is incompatible  ===
En installant un paquet, vous pourriez avoir une erreur comme
ERROR: Package has requirement X, but you'll have Y which is incompatible..

Pour utiliser le nouveau résolveur de dépendances, installez la plus récente version de pip ou une version supérieure à [21.3].

Lancez ensuite de nouveau la commande d'installation.

=== No matching distribution found for X ===
À l'installation d'un paquet, vous pouvez obtenir un message semblable à

pip n'a trouvé aucun paquet à installer qui rencontre les exigences (nom, version ou tags).
Assurez-vous que le nom et la version sont corrects.
Sachez aussi que les wheels manylinux_x_y sont ignorés.

Vous pouvez aussi vérifier si le paquet est disponible avec la commande avail_wheels ou en consultant la page  Wheels disponibles.

=== Installer plusieurs paquets ===
Lorsque possible, il est préférable d'installer plusieurs paquets avec une seule commande.

Ainsi, pip peut résoudre plus facilement les problèmes de dépendance.

=== My virtual environment was working yesterday but not anymore  ===
Les fréquentes mises à jour des paquets font en sorte qu'un environnement virtuel ne peut souvent être reproduit.

Il est possible aussi qu'un environnement virtuel créé dans $SCRATCH soit partiellement détruit lors de la purge automatique de ce système de fichiers, ce qui empêcherait l'environnement virtuel de bien fonctionner.

Pour contrer ceci, gelez les paquets et leurs versions avec
X.Y' 'package2X.Y.Z' 'package3X.Y'
}}
et créez ensuite un fichier de requis qui sera utilisé pour installer ces paquets dans votre tâche.

=== X is not a supported wheel on this platform ===
À l'installation d'un paquet, vous pourriez obtenir une erreur comme  ERROR: package-3.8.1-cp311-cp311-manylinux_2_28_x86_64.whl is not a supported wheel on this platform.

Certains paquets peuvent être incompatibles ou non pris en charge par nos systèmes.
Deux cas fréquents sont :
* installation d'un  paquet manylinux
* ou un paquet Python construit pour une autre version de Python (par exemple, installer un paquet construit pour Python 3.11 quand vous avez Python 3.9).

Certains paquets manylinux peuvent se trouver parmi nos wheels Python.

=== AttributeError: module ‘numpy’ has no attribute ‘X’ ===
À l'installation d'un whell, la plus récente version de Numpy est installée si aucune version spécifique n'est demandée.
Plusieurs attributs  ont été déclarés obsolètes dans Numpy v1.20 et  ne sont plus offerts dans v1.24.

Dépendant de l'attribut, une erreur comme AttributeError: module ‘numpy’ has no attribute ‘bool’ pourrait survenir.

Ceci est résolu avec l'installation d'une version précédente de Numpy avec pip install --no-index 'numpy<1.24'.

=== ModuleNotFoundError: No module named 'X' ===
Il est possible qu'un module Python que vous voulez importer ne soit pas trouvé. Il y a plusieurs explications pour ceci, mais les plus fréquentes sont que
* le paquet n'est pas installé ou encore il n'est pas visible pour l'interpréteur Python;
* le nom du module ne correspond pas au nom réel;
* l'environnement virtuel est défectueux.

Pour contrer ceci, évitez de
* modifier la variable d'environnement PYTHONPATH;
* modifier la variable d'environnement PATH;
* charger un module alors qu'un environnement virtuel est activé; chargez d'abord tous les modules avant d'activer l'environnement virtuel.

Si vous avez ce problème,
* avec pip list, vérifiez si le paquet est installé;
* vérifiez encore si le nom que vous entrez correspond exactement au nom du module (majuscules, minuscules, traits de soulignement, etc.);
* vérifiez si le module est importé au bon niveau quand il provient de son répertoire source.

Dans le doute, recommencez avec un nouvel environnement.

=== ImportError: numpy.core.multiarray failed to import ===

Ce message peut survenir quand vous tentez d'importer un module Python qui dépend de Numpy.

Ceci se produit quand une version incompatible de Numpy est installée ou utilisée; vous devez installer une version compatible.

Le cas type est  la version 2.0 de Numpy qui brise l'ABI.
Dans le cas d'un wheel construit avec une version 1.x mais installé avec une version 2.x, vous devez installer une version antérieure avec pip install --no-index 'numpy<2.0'.

=== Defaulting to user installation because normal site-packages is not writeable ===
À l'installation d'un paquet peut s'afficher le message Defaulting to user installation because normal site-packages is not writeable.

Il s'agit du comportement par défaut de pip en dehors d'un environnement virtuel.
Cela signifie qu'aucun environnement virtuel n'a été trouvé ni activé et que pip a essayé d'installer à un endroit où il ne dispose pas des permissions nécessaires.

Ceci causera des installations locales pouvant causer des problèmes.

=== Installation locale ===
Une installation locale peut se produire de manière inattendue (si une erreur se produit avec votre environnement virtuel ou des problèmes de permission) ou par une installation définie avec (pip install --user).

Une installation locale est essentiellement le transfert des dépendances dans un espace partagé, ce qui n'est pas du tout souhaitable. Cela crée des problèmes d'importation ou d'exécution étranges avec vos paquets Python, ou encore des conflits de versions pouvant engendrer un véritable enfer de dépendances.

Il est préférable d'utiliser un  environnement virtuel pour isoler, reproduire et gérer les diffférentes versions dans vos projets.

==== Supprimer une installation locale ====
Pour bien supprimer une installation locale, lancez

Vous devrez peut-être spécifier les binaires directement si vous utilisez ~/.local/bin pour des binaires locaux dans des paquets autres que Python.

Quand les installations locales sont supprimées, recommencez avec un nouvel environnement virtuel jamais utilisé.