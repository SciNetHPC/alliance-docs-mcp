---
title: "TensorFlow/fr"
url: "https://docs.alliancecan.ca/wiki/TensorFlow/fr"
category: "General"
last_modified: "2025-09-08T20:57:52Z"
page_id: 3617
display_title: "TensorFlow"
language: "fr"
---

TensorFlow est une bibliothèque logicielle open source d'apprentissage machine.

Si vous voulez porter un programme TensorFlow sur une de nos grappes, nous vous recommandons de prendre connaissance du tutoriel sur l'apprentissage machine.

==Installation==

Les directives suivantes servent à installer TensorFlow dans votre répertoire home à l'aide des (wheels Python ) qui se trouvent dans /cvmfs/soft.computecanada.ca/custom/python/wheelhouse/.

Le wheel TensorFlow sera installé dans un environnement virtuel Python avec la commande pip.

Chargez les modules requis par TensorFlow; dans certains cas, d'autres modules pourraient être requis (par exemple CUDA).

Créez un nouvel environnement Python.

Activez le nouvel environnement.

Installez TensorFlow dans votre nouvel environnement virtuel en utilisant la commande suivante.

Chargez les modules requis par TensorFlow. TF 1.x requiert StdEnv/2018.

Remarque : TF 1.x n'est pas disponible sur Narval, puisque cette grappe n'offre pas StdEnv/2018.

Créez un nouvel environnement Python.

Activez le nouvel environnement.

Installez TensorFlow dans votre nouvel environnement virtuel en utilisant une des commandes suivantes, dépendant de si vous avez besoin d'utiliser un GPU.

N'installez pas le paquet tensorflow sans le suffixe _cpu ou _gpu car il existe des problèmes de compatibilité avec d'autres bibliothèques.

=== CPU seulement ===
1.15.0}}

=== GPU ===
1.15.0}}

=== Le paquet R  ===

Pour utiliser TensorFlow en R, suivez les directives données ci-dessus pour créer un environnement virtuel et y installer TensorFlow. Suivez ensuite cette procédure ː

Chargez les modules requis.

Activez votre environnement virtuel Python.

Lancez R.

En R, installez le paquet devtools, puis TensorFlow.

install.packages('devtools', repos='https://cloud.r-project.org')
devtools::install_github('rstudio/tensorflow')

Vous pouvez maintenant procéder. N'appelez pas install_tensorflow() en R puisque TensorFlow est déjà installé dans votre environnement virtuel avec pip. Pour utiliser TensorFlow tel qu'installé dans votre environnement virtuel, entrez les commandes suivantes en R, après que l'environnement est activé.

library(tensorflow)
use_virtualenv(Sys.getenv('VIRTUAL_ENV'))

==Soumettre une tâche TensorFlow avec un GPU==
Soumettez une tâche TensorFlow ainsi

Le script contient

Le script Python se lit

Une fois la tâche terminée, ce qui devrait nécessiter moins d'une minute, un fichier de sortie avec un nom semblable à node_id-job_id.out devrait être généré. Le contenu de ce fichier serait similaire à ce qui suit; il s'agit d'exemples de messages TensorFlow et il est possible que vous en ayez d'autres.

TensorFlow fonctionne sur tous les types de nœuds GPU. Pour plus d'information, voir cette page.

==Suivi==

Il est possible de se connecter à un nœud sur lequel une tâche est en cours pour y exécuter des processus. On peut ainsi faire le suivi des ressources utilisées par TensorFlow et visualiser le déroulement de l'entraînement. Pour des exemples, consultez Surveillance d'une tâche en cours.

===TensorBoard===

TensorFlow propose la suite d'outils de visualisation TensorBoard qui lit les événements TensorFlow et modélise les fichiers. Pour savoir comment créer ces fichiers, consultez TensorBoard tutorial on summaries.

Sachez toutefois que TensorBoard exige trop de puissance de calcul pour être exécuté sur un nœud de connexion. Nous vous recommandons de l'exécuter dans la même tâche que le processus TensorFlow. Pour ce faire, lancez TensorBoard en arrière-plan en l'appelant avant le script Python, en y ajoutant le caractère (&).

 # Your SBATCH arguments here

 tensorboard --logdir=/tmp/your_log_dir --host 0.0.0.0 --load_fast false &
 python train.py  # example

Pour accéder TensorBoard avec un fureteur une fois que la tâche est en cours, il faut créer un lien entre votre ordinateur et le nœud sur lequel TensorFlow et TensorBoard sont exécutés. Pour ce faire, vous avez besoin du hostname du nœud de calcul sur lequel le serveur TensorFlow se trouve. Pour le trouver, faites afficher la liste de vos tâches avec la commande sq et repérez la tâche; le hostname est la valeur qui se trouve dans la colonne NODELIST.

Pour créer la connexion, lancez la commande sur votre ordinateur local.

Remplacez computenode par le hostname obtenu à l'étape précédente; userid par votre nom d'utilisateur de l'Alliance et; cluster par le hostname de la grappe, soit rorqual, fir, nibi, etc. Si le port 6006 était déjà utilisé, tensorboard va en utiliser un autre (p. ex. 6007, 6008...).

Une fois que la connexion est établie, allez à http://localhost:6006.

==Utiliser plusieurs GPU==

TensorFlow offre des stratégies différentes pour utiliser plusieurs GPU avec l'API de haut niveau tf.distribute. Dans les sections qui suivent, nous montrons des exemples de code pour chacune des stratégies avec Keras. Pour plus d'information, consultez la documentation officielle de TensorFlow.

====Stratégie miroir====

=====Nœud unique=====

Le script Python tensorflow-singleworker.py a le format

=====Nœuds multiples=====

La syntaxe pour utiliser des GPU distribués sur plusieurs nœuds ressemble beaucoup au cas du nœud simple; la différence principale est l'emploi de MultiWorkerMirroredStrategy(). Ici, nous utilisons SlurmClusterResolver() pour dire à TensorFlow d'obtenir par Slurm l'information sur la tâche plutôt que d'assigner manuellement un nœud principal et des nœuds secondaires (workers), par exemple. Nous devons aussi ajouter CommunicationImplementation.NCCL à la stratégie de distribution pour indiquer que nous voulons utiliser la bibliothèque NCCL de NVIDIA pour les communications entre les GPU. Ceci n'était pas nécessairement le cas pour un nœud simple puisque NCCL se trouve par défaut avec MirroredStrategy().

où config_env.sh a la forme

Le script launch_training.sh a la forme

Le script Python tensorflow-multiworker.py a la forme suivante :

==Créer des points de contrôle==
Peu importe le temps que dure l'exécution de votre code, une bonne habitude à prendre est de créer des points de contrôle pendant l'entraînement. Un point de contrôle vous donne le portrait de votre modèle à un moment précis du processus d'entraînement (après un certain nombre d'itérations ou d'époques); le portrait est enregistré sur disque et vous pourrez le récupérer par la suite. Ceci est pratique pour diviser en petites tâches une tâche qui doit avoir un long temps d'exécution, ce qui pourrait faire qu'elles soient être allouées plus rapidement à une grappe. C'est aussi un bon moyen d'éviter de perdre votre travail en cas d'erreurs inattendues ou de panne du matériel.

===Avec Keras===

Pour créer un point de contrôle dans un entraînement avec keras, nous recommandons le paramètre callbacks de la méthode model.fit(). Dans l'exemple suivant, nous demandons à TensorFlow de créer un point de contrôle à la fin de chacune des époques d'entraînement.

 callbacks = [tf.keras.callbacks.ModelCheckpoint(filepath="./ckpt",save_freq="epoch")] # Make sure the path where you want to create the checkpoint exists

 model.fit(dataset, epochs=10 , callbacks=callbacks)

Pour plus d'information, consultez la documentation officielle de TensorFlow.

===Avec une boucle d'entraînement personnalisée===

Voyez la documentation officielle de TensorFlow.

==Dépannage==

===scikit-image===

Si vous utilisez la bibliothèque scikit-image, vous pourriez recevoir l'erreur
OMP: Error #15: Initializing libiomp5.so, but found libiomp5.so already initialized.

Ceci se produit quand la bibliothèque TensorFlow essaie de charger une version de OMP incompatible avec la version du système. Pour contourner ceci :
$(strace python -c 'from skimage.transform import AffineTransform' 2>&1  grep -v ENOENT  grep -ohP -e '(?<")[^"]+libiomp5.so(?")'  xargs realpath)
|find -path '*_solib_local*' -name libiomp5.so -exec ln -sf $LIBIOMP_PATH {} \;
}}
L'installation de la bibliothèque TensorFlow pourra alors utiliser libiomp5.so.

===libcupti.so===

Certaines fonctions de suivi de TensorFlow utilisent la bibliothèque libcupti.so; si cette dernière n'est pas disponible, l'erreur suivante pourrait survenir :

I tensorflow/stream_executor/dso_loader.cc:142] Couldn't open CUDA library libcupti.so.9.0. LD_LIBRARY_PATH: /usr/local/cuda-9.0/lib64

La solution est d'exécuter les commandes suivantes avant l'exécution du script.
$LD_LIBRARY_PATH:$CUDA_HOME/extras/CUPTI/lib64/
}}
Remplacez xxx par la version appropriée de CUDA que vous pouvez trouver avec module av cuda.

===libiomp5.so invalid ELF header===

Le fichier objet partagé libiomp5.so est quelquefois par erreur installé en tant que fichier texte, ce qui peut produire des erreurs comme ceci :

/home/username/venv/lib/python3.6/site-packages/tensorflow/python/../../_solib_local/_U@mkl_Ulinux_S_S_Cmkl_Ulibs_Ulinux___Uexternal_Smkl_Ulinux_Slib/libiomp5.so: invalid ELF header

Pour solutionner ces erreurs, accédez au répertoire indiqué dans le message (soit [...]/_U@mkl_Ulinux_S_S_Cmkl_Ulibs_Ulinux___Uexternal_Smkl_Ulinux_Slib) et lancez la commande

Le fichier texte sera remplacé par le bon lien symbolique.

==Contrôle du nombre de CPU et de fils==

Les paramètres de configuration intra_op_parallelism_threads et inter_op_parallelism_threads ont un effet sur le nombre de fils utilisés par TensorFlow; ces paramètres peuvent être définis comme suit :

 tf.config.threading.set_inter_op_parallelism_threads(num_threads)
 tf.config.threading.set_intra_op_parallelism_threads(num_threads)

==Problèmes connus==
Un bogue s'est introduit dans l'implémentation Keras de Tensorflow après la version 2.8.3. Il affecte la performance des layers d'augmentation des données tf.keras.layers.Random (comme tf.keras.layers.RandomRotation, tf.keras.layers.RandomTranslation, etc.). Le processus d'entraînement est ralenti d'un facteur de plus de 100. Ce bogue a été corrigé dans la version 2.12.