---
title: "Gurobi/fr"
url: "https://docs.alliancecan.ca/wiki/Gurobi/fr"
category: "General"
last_modified: "2026-01-22T13:38:26Z"
page_id: 14015
display_title: "Gurobi"
language: "fr"
---

Gurobi est une suite logicielle commerciale qui permet de résoudre des problèmes complexes d'optimisation. Nous abordons ici son utilisation pour la recherche sur nos grappes.

==Limites de la licence==

Nous dispensons le soutien technique pour la licence gratuite disponible sur Nibi, Fir, Rorqual et Trillium. Cette licence permet 4096 utilisations simultanées (avec jetons) et l'optimisation distribuée sur un maximum de 100 nœuds. Plusieurs tâches peuvent être exécutées en simultané.
Vous devez cependant accepter certaines conditions. Faites parvenir un courriel au  soutien technique avec l'entente (Academic Usage Agreement) dûment complétée; vous pourrez ensuite utiliser les applications après un délai de quelques jours.

===Academic Usage Agreement===

My Alliance username is "_______" and I am a member of the academic institution "_____________________."  This message confirms that I will only use the Gurobi license provided on Digital Research Alliance of Canada systems for the purpose of non-commercial research project(s) to be published in publicly available article(s).

===Configurer votre compte===
Il n'est pas nécessaire de créer le fichier ~/.licenses/gurobi.lic. Les paramètres pour l'utilisation de notre licence Gurobi sont configurés par défaut quand un module Gurobi est chargé sur une grappe.

===Tester votre licence===
Pour vous assurer que votre nom d'utilisateur a bien été ajouté à la licence Gurobi, lancez la commande suivante%nbsp;:

 $ module load gurobi
 $ gurobi_cl 1> /dev/null && echo Success || echo Fail

Si vous obtenez Success, vous pouvez utiliser Gurobi immédiatement. Si vous obtenez Fail, vérifiez s'il existe un fichier nommé ~/.license/gurobi; si c'est le cas, supprimez ce fichier, chargez le module Gurobi de nouveau et refaites le test.
Si vous obtenez encore Fail, vérifiez si une variable d'environnement définit GUROBI dans vos fichiers ~/.bashrc ou ~/.bash_profile; si c'est le cas, supprimez la ou les lignes correspondantes ou mettez-les en commentaire, déconnectez-vous et connectez-vous de nouveau, chargez le module Gurobi de nouveau et refaites le test.

Si vous obtenez toujours Fail, contactez le  soutien technique pour de l'assistance.

===Utiliser un minimum de licences===

Les licences peuvent être obtenues d’un seul serveur situé en Ontario; il est donc important de limiter les demandes autant que possible. Plutôt que d’obtenir une licence chaque fois que votre tâche requiert Gurobi, ce qui peut être des centaines, voire même des milliers de fois, faites en sorte que votre code fonctionne avec une seule licence pour toute la durée de la tâche, et ce, peu importe l’environnement que vous utilisez. Vous y gagnerez en performance puisque le temps de communication avec un serveur à distance est très long et de plus, notre serveur sera plus accessible aux autres utilisateurs de Gurobi.  Si votre code n’est pas adapté en conséquence, le serveur pourrait ne pas pouvoir émettre de licence de façon intermittente; si ceci se produit, nous vous demanderons de mettre fin à toutes vos tâches et de corriger votre programme. Voyez comment créer un environnement pour tous vos modèles avec vos programmes en C++ ou en Python. Le même problème peut aussi survenir avec d’autres programmes qui fonctionnent en parallèle comme R, surtout quand plusieurs tâches parallèles simultanées sont soumises à l'ordonnanceur,

== Allocations interactives ==

===Ligne de commande===

 [gra-login2:~] salloc --time=1:00:0 --cpus-per-task=8 --mem=1G --account=def-xyz
 [gra800:~] module load gurobi
 [gra800:~] gurobi_cl Record=1 Threads=8 Method=2 ResultFile=p0033.sol LogFile=p0033.log $GUROBI_HOME/examples/data/p0033.mps
 [gra800:~] gurobi_cl --help

===Interpréteur interactif ===

 [gra-login2:~] salloc --time=1:00:0 --cpus-per-task=8 --mem=1G --account=def-xyz
 [gra800:~] module load gurobi
 [gra800:~] echo "Record 1" > gurobi.env    see *
 [gra800:~] gurobi.sh
 gurobi> m = read('/cvmfs/restricted.computecanada.ca/easybuild/software/2017/Core/gurobi/8.1.1/examples/data/glass4.mps')
 gurobi> m.Params.Threads = 8               see **
 gurobi> m.Params.Method = 2
 gurobi> m.Params.ResultFile = "glass4.sol"
 gurobi> m.Params.LogFile = "glass4.log"
 gurobi> m.optimize()
 gurobi> m.write('glass4.lp')
 gurobi> m.status                           see ***
 gurobi> m.runtime                          see ****
 gurobi> help()

où
    * https://www.gurobi.com/documentation/8.1/refman/recording_api_calls.html
   ** https://www.gurobi.com/documentation/8.1/refman/parameter_descriptions.html
  *** https://www.gurobi.com/documentation/8.1/refman/optimization_status_codes.html
 **** https://www.gurobi.com/documentation/8.1/refman/attributes.html

===Répéter des appels API===
Il est possible d'enregistrer des appels API et de rejouer l'enregistrement avec

 [gra800:~] gurobi_cl recording000.grbr

Référence : https://www.gurobi.com/documentation/8.1/refman/recording_api_calls.html

== Soumettre une tâche en lots sur une grappe ==

Une fois que votre script Slurm est prêt, vous pouvez le soumettre à la queue avec la commande sbatch script-name.sh. Vous pouvez vérifier l'état de vos tâches dans la queue avec la commande sq. Les scripts suivants solutionnent deux problèmes qui se trouvent dans le répertoire  examples de chaque module Gurobi.

=== Exemple de données ===

Le script Slurm suivant utilise l' interface ligne de commande pour résoudre un modèle simple pour produire des pièces de monnaie écrit en format LP. La dernière ligne montre comment des paramètres peuvent être passés directement à l'outil ligne de commande gurobi_cl avec des arguments simples. Pour sélectionner les meilleurs paramètres pour un problème particulier et pour choisir les valeurs optimales, voyez les sections Performance and Parameters et Algorithms and Search dans la page Knowledge Base et dans la documentation Gurobi.

=== Exemple avec Python ===

Le script Slurm suivant solutionne un modèle simple de l'emplacement de divers sites avec Gurobi Python. L'exemple montre comment  paramétrer les fils en nombre égal à celui des cœurs alloués à la tâche en générant un fichier  gurobi.env dans le répertoire de travail quand vous utilisez l'interface Gurobi Python.
Ceci doit être fait pour chaque tâche soumise, autrement Gurobi lancera par défaut autant de fils d'exécution qu'il y a de cœurs physiques dans le nœud de calcul plutôt que d'utiliser le nombre de cœurs physiques alloués à la tâche par l'ordonnanceur, ce qui risque de ralentir la tâche et nuire aux tâches exécutées sur le même nœud par les autres utilisateurs.

== Environnements virtuels Python ==

Gurobi a sa propre version de Python qui ne contient aucun autre paquet de tiers autre que Gurobi. Pour utiliser Gurobi avec d'autres paquets Python comme NumPy, Matplotlib, Pandas et autres, il faut créer un environnement virtuel Python dans lequel seront installés gurobipy et par exemple pandas.
Avant de commencer, il faut décider quelle combinaison des versions Gurobi et Python nous voulons utiliser. La liste suivante montre les versions de Python supportées par les versions principales de Gurobi dans les environnements standards (StdEnv).

 [name@server ~] module load StdEnv/2016; module load gurobi/8.1.1; cd $EBROOTGUROBI/lib; ls -d python*
 python2.7  python2.7_utf16  python2.7_utf32  python3.5_utf32  python3.6_utf32  python3.7_utf32

 [name@server ~] module load StdEnv/2020; module load gurobi/9.5.2; cd $EBROOTGUROBI/lib; ls -d python*
 python2.7_utf16  python2.7_utf32  python3.10_utf32  python3.7  python3.7_utf32  python3.8_utf32  python3.9_utf32

 [name@server ~] module load StdEnv/2023; module load gurobi/10.0.3; cd $EBROOTGUROBI/lib; ls -d python*
 python3.10_utf32  python3.11_utf32  python3.7  python3.7_utf32  python3.8_utf32  python3.9_utf32

 [name@server ~] module load StdEnv/2023; module load gurobi/11.0.1; cd $EBROOTGUROBI/lib; ls -d python*
 python3.11

=== Installer Gurobi pour Python ===

Tel que mentionné vers la fin de  How do I install Gurobi for Python?, la méthode précédemment recommandée pour installer Gurobi pour Python avec setup.py est désormais obsolète et ne peut être utilisée qu'avec les versions Gurobi 10 (et plus anciennes). La section Versions Gurobi 11.0.0 (et plus récentes) montre comment télécharger simultanément une roue binaire compatible à partir de pypi.org et la convertir dans un format utilisable avec la nouvelle commande recommandée.

=== Versions Gurobi 10.0.3 et moins récentes ===

Il faut suivre les étapes suivantes une fois sur chaque système avec StdEnv2023 et moins récents. Chargez d'abord les modules pour créer l'environnement virtuel, puis activez cet environnement.

Installez les paquets que vous voulez utiliser, ici pandas. Par exemple

Installez maintenant gurobipy dans l'environnement. À partir de StdEnv/2023, il n'est plus possible de l'installer dans $EBROOTGUROBI avec la commande python setup.py build --build-base /tmp/${USER} install, ce qui causerait une erreur fatale et le message error: could not create 'gurobipy.egg-info': Read-only file system. Copiez les fichiers ailleurs  (par exemple dans /tmp/$USER) où l'installation sera faite, comme ci-dessous :

=== Versions Gurobi 11.0.0 (et plus récentes) ===

Encore une fois, les étapes suivantes doivent être effectuées une fois pour chaque système sous StdEnv/2023 et les versions antérieures. Chargez d'abord les modules dans Créer et utiliser un environnement virtuel, puis activez-le. La version 11.0.0 est ignorée car il a été observé qu'elle produit une erreur de segmentation dans au moins un exemple, comparé à la version 11.0.1 qui fonctionne sans problème.

Comme précédemment, installez tous les paquets Python nécessaires. Étant donné que l'exemple suivant nécessite numpy, nous installons le paquet Pandas.

Installez ensuite gurobipy dans l'environnement. Comme mentionné ci-dessus et dans [article], l'utilisation de setup.py pour installer Gurobi pour Python est déconseillée à partir de Gurobi 11. Pip et conda sont tous deux proposés comme alternatives; cependant, comme conda ne doit pas être utilisé sur nos systèmes, l'approche avec pip sera démontrée ici. L'installation de gurobipy est légèrement compliquée car nos systèmes Linux sont configurés avec le préfixe gentoo. En conséquence, ni A) la commande recommandée pour télécharger et installer l'extension gurobipy depuis le serveur public PyPI pip install gurobipy==11.0.1 mentionnée dans l'article B) ni la commande hors ligne pour installer la roue avec python -m pip install --find-links  --no-index gurobipy ne fonctionneront. Au lieu de cela, nous avons préparé un script pour télécharger et convertir simultanément la roue existante dans un format utilisable avec un nouveau nom. Il y a une mise en garde; pour chaque nouvelle version de Gurobi, vous devez vous rendre sur https://pypi.org/project/gurobipy/11.0.1/#history, cliquer sur la version souhaitée et cliquer sur le bouton Download files situé dans le menu de gauche. Enfin, cliquez pour copier le lien https du fichier wheel (nommé gurobipy-11.0.1-cp311-cp311-manylinux2014_x86_64.manylinux_2_17_x86_64.whl dans le cas de Gurobi 11.0.1) et collez-le comme argument --url comme ci-dessous.

=== Travailler dans l'environnement avec Gurobi ===

Une fois créé, notre environnement Gurobi peut être activé et utilisé à tout moment. Pour démontrer cela, nous chargeons également Gurobi (donc $EBROOTGUROBI est défini) et scipy-stack (donc scipy est disponible). Les deux sont nécessaires pour exécuter l'exemple de matrice (avec numpy qui était déjà installé dans notre environnement via pandas avec pip dans une étape précédente).

Les scripts Python comme les exemples fournis avec le module gurobi peuvent alors être exécutés dans l'environnement virtuel avec Python.

De même, des scripts Python personnalisés tels que les  suivants peuvent être soumis en tant que tâches à la file d'attente en écrivant des scripts Slurm qui chargent votre environnement virtuel.

Soumettez le script dans la queue avec sbatch my_slurm_script.sh.

Pour plus d'information sur la création et l'utilisation des environnements virtuels Python, voir   Créer un environnement virtuel dans vos tâches.

== Utiliser Gurobi avec Java ==

Vous devez aussi charger un module Java et ajouter une option à la commande Java pour permettre à l'environnement virtuel Java de localiser les bibliothèques Gurobi, comme dans l'exemple suivant.

== Utiliser Gurobi avec des notebooks Jupyter ==

Vous trouverez de l'information sur Resources, Code and Modeling Examples et Optimization with Python – Jupyter Notebook Modeling Examples.  Sur le site support.gurobi.com faites une recherche avec Jupyter Notebooks.

Voir aussi cette démonstration, à 38min 28sec de la vidéo.

== Comment citer Gurobi ==

Voir How do I cite Gurobi software for an academic publication?