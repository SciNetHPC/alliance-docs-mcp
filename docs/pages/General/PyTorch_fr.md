---
title: "PyTorch/fr"
url: "https://docs.alliancecan.ca/wiki/PyTorch/fr"
category: "General"
last_modified: "2026-01-30T09:03:01Z"
page_id: 4699
display_title: "PyTorch"
language: "fr"
---

PyTorch est un paquet Python qui offre deux fonctionnalités de haut niveau :
*le calcul tensoriel (semblable à celui effectué par NumPy) avec forte accélération de GPU, et
*des réseaux de neurones d’apprentissage profond dans un système de gradients conçu sur le modèle d’un magnétophone.

Si vous voulez porter un programme PyTorch sur une de nos grappes, il serait bon de prendre connaissance  de ce tutoriel.

= Clarification =

Il y a une certaine ressemblance entre PyTorch et Torch, mais pour des raisons pratiques vous pouvez considérer que ce sont des projets différents.

Les développeurs PyTorch offrent aussi LibTorch qui permet d'implémenter des extensions à PyTorch à l'aide de C++ et d'implémenter des applications d'apprentissage machine en C++ pur. Les modèles Python écrits avec PyTorch peuvent être convertis et utilisés en C++ avec TorchScript.

= Installation =

==Wheels récemment ajoutés==
Pour connaître la dernière version de PyTorch, utilisez

Pour plus d'information, voyez Wheels disponibles.

==Installation du wheel ==

La meilleure option est d'installer avec Python wheels comme suit :

::1. Chargez un module  Python avec module load python.
::2. Créez et démarrez un  environnement virtuel.
::3. Installez PyTorch dans l'environnement virtuel avec pip install.

==== GPU et CPU ====
:

====En supplément====
En plus de  torch, vous pouvez aussi installer torchvision, torchtext et torchaudio.

=Soumettre une tâche=

Le script suivant est un exemple de soumission d'une tâche utilisant le wheel Python avec un environnement virtuel.

Pour plus d'information sur le nombre de CPU et la mémoire des GPU sur nos grappes, voir Ratios dans les bundles.

Le script Python pytorch-ddp-test.py a la forme suivante :

Vous pouvez alors soumettre une tâche PyTorch avec

= Haute performance=

== TF32 : Performance vs précision ==

Avec sa version 1.7.0, PyTorch a ajouté le support pour le mode TensorFloat-32 (TF32) de Nvidia et est seulement disponible pour les architectures GPU d'Ampere et de Nvidia. Avec ce mode qui est offert par défaut dans les versions 1.7.x à 1.11.x, les opérations tensorielles se font jusqu'à 20x plus rapidement que les opérations équivalentes en simple précision (FP32). Cependant, ce gain en performance peut engendrer une baisse dans la précision du résultat des opérations, ce qui pose problème avec les modèles d'apprentissage profond qui utilisent à l'occasion des matrices mal conditionnées ou qui effectuent de longues séquences d'opérations tensorielles. Suite aux commentaires de la communauté des utilisateurs, TF32 est désactivé par défaut pour les multiplications matricielle et activé par défaut pour les convolutions à partir de la version 1.12.0.

Sur les grappes équipées de GPU A100, H100 ou Nvidia plus récents, sachez que
* vous pourriez remarquer un fort ralentissement dans l'exécution sur GPU du même code avec torch < 1.12.0 et torch >= 1.12.0;
* vous pourriez obtenir des résultats différents dans l'exécution sur GPU du même code avec torch < 1.12.0 et torch >= 1.12.0.

Pour activer ou désactiver TF32 pour torch >= 1.12.0, donnez la valeur True ou False aux indicateurs suivants :

 torch.backends.cuda.matmul.allow_tf32 = False # Enable/disable TF32 for matrix multiplications
 torch.backends.cudnn.allow_tf32 = False # Enable/disable TF32 for convolutions

Pour plus d'information, consultez cette partie de la documentation PyTorch.

== Travailler avec un CPU ==

Par défaut, PyTorch permet le parallélisme avec plusieurs CPU de deux façons :
* intra-op, par l’implémentation parallèle d’opérateurs souvent utilisés en apprentissage profond comme le produit matriciel ou le produit de convolution, en utilisant OpenMP directement ou avec des bibliothèques de bas niveau comme MKL et OneDNN. Quand du code PyTorch doit effectuer de telles opérations, elles utilisent automatiquement de multiples fils avec tous les cœurs CPU disponibles.
* inter-op, par la capacité d’exécuter différentes parties de code de manière concurrente. Ce mode de parallélisme nécessite habituellement que le programme soit conçu de manière à exécuter plusieurs parties en parallèle, par exemple en faisant usage du compilateur en temps réel torch.jit pour exécuter des tâches asynchrones dans un programme TorchScript.

Pour les petits modèles, nous recommandons fortement d’utiliser plusieurs CPU plutôt qu’un GPU. L’entraînement sera certainement plus rapide avec un GPU (sauf dans les cas de très petits modèles), mais si le modèle et le jeu de données ne sont pas assez grands, la vitesse gagnée avec le GPU ne sera probablement pas très importante et la tâche n’utilisera qu’une petite part de la capacité de calcul. Ce n’est peut-être pas grave sur votre propre ordinateur, mais dans un environnement partagé comme sur nos grappes, vous bloqueriez une ressource qui pourrait servir à effectuer de calculs de grande échelle par un autre projet. De plus, l’utilisation d’un GPU contribuerait à la diminution de l’allocation de votre groupe et aurait une incidence sur la priorité accordée aux tâches de vos collègues.

Dans le code suivant, il y a plusieurs occasions d’utiliser le parallélisme intra-op.

Avant de tester le code ci-dessus, vous devez d'abord télécharger les données.

En demandant plus de CPU et sans changer le code, on peut constater l’effet sur la performance.

== Travailler avec un GPU ==

On entend souvent dire qu’il faut absolument entraîner un modèle avec un GPU s’il y en a un à notre disposition. Ceci est presque toujours vrai (l'entraînement de très petits modèles est souvent plus rapide avec un ou plusieurs CPU) sur un poste de travail local, mais ce n’est pas le cas sur nos grappes.

Autrement dit, vous ne devriez pas demander un GPU si votre code ne peut pas faire un usage raisonnable de sa capacité de calcul.

La performance avantageuse des GPU pour les tâches d’apprentissage profond provient de deux sources :

# La capacité de paralléliser l’exécution de certaines opérations clés, par exemple le multiplieur-accumulateur, sur plusieurs milliers de cœurs de calcul, en comparaison du très petit nombre de cœurs disponibles avec la plupart des CPU.
# Une bande passante de mémoire beaucoup plus grande que pour un CPU, ce qui permet aux GPU d’utiliser efficacement leur très grand nombre de cœurs pour traiter une plus grande quantité de données par cycle de calcul.

Comme c’est le cas avec plusieurs CPU, PyTorch offre des implémentations parallèles  d’opérateurs souvent utilisés en apprentissage profond, comme le produit matriciel et le produit de convolution et utilise des bibliothèques spécialisées pour les GPU comme CUDNN ou MIOpen, selon la plateforme matérielle. Ceci signifie que pour qu’il vaille la peine d’utiliser un GPU pour une tâche d’apprentissage, elle doit être composée d’éléments qui peuvent être élargis à une application massive du parallélisme de par le nombre d’opérations pouvant être parallélisées, de par la quantité des données à traiter ou idéalement de par les deux. Un exemple concret serait un grand modèle qui a un grand nombre d’unités et de couches ou qui a beaucoup de données en entrée, et idéalement qui présente ces deux caractéristiques.

Dans l’exemple ci-dessous, nous adaptons le code de la section précédente pour utiliser un GPU et nous examinons la performance. Nous observons que deux paramètres jouent un rôle important : batch_size et num_workers. Le premier paramètre améliore la performance en augmentant la taille des entrées à chaque itération et en utilisant mieux la capacité du GPU. Dans le cas du second paramètre, la performance est améliorée en facilitant  le mouvement des données entre la mémoire de l’hôte (le CPU) et la mémoire du GPU, ce qui réduit la durée d’inactivité du GPU en attente de données à traiter.

Nous pouvons tirer deux conclusions :

# Augmenter la valeur de batch_size au maximum qu’il est possible pour la mémoire du GPU optimise la performance.
# Utiliser un DataLoader avec autant de workers que cpus-per-task facilite l’apport de données au GPU.

Bien entendu, le paramètre batch_size a aussi un impact sur la performance d’un modèle dans une tâche (c.-à-d. l’exactitude, l’erreur, etc.) et il existe différentes écoles de pensée sur l’utilisation de grands lots. Nous n’abordons pas le sujet ici, mais si vous croyez qu’un petit lot conviendrait mieux à votre application, allez à la section Parallélisme des données avec un seul GPU  pour savoir comment maximiser l’utilisation du GPU avec de petites entrées de données.

== Parallélisme des données ==

Dans ce contexte, le parallélisme des données désigne les méthodes permettant d'entraîner un modèle sur plusieurs copies (replicas) en parallèle, chaque copie recevant un ensemble différent de données d'entraînement à chaque itération. Les gradients sont ensuite agrégés à la fin de chaque itération et les paramètres de toutes les copies sont mis à jour de manière synchrone ou asynchrone, selon la méthode utilisée.

Cette approche pourrait permettre un gain de vitesse significatif en parcourant tous les exemples d'un grand ensemble de données environ N fois plus rapidement, où N est le nombre de copies du modèle.

Un inconvénient majeur de cette approche est que, pour obtenir un modèle entraîné équivalent à celui entraîné sans parallélisme de données, il faut ajuster le taux d'apprentissage ou la taille des lots en fonction du nombre de copies. Voir cette discussion pour plus d'information.

Avec plusieurs GPU, chaque GPU héberge une copie de votre modèle. Par conséquent, le modèle doit être suffisamment petit pour occuper la mémoire d'un seul GPU.

Il existe plusieurs manières de paralléliser les données avec PyTorch. Nous présentons ici des tutoriels avec la classe DistributedDataParallel avec un ou plusieurs GPU en utilisant le paquet  PyTorch Lightning.

====Parallélisme des données avec plusieurs GPU====

Avec plusieurs GPU, la classe DistributedDataParallel est  recommandée par les développeurs PyTorch, que ce soit avec un nœud unique ou avec plusieurs nœuds. Dans le cas qui suit, plusieurs GPU sont distribués sur deux nœuds.

Le script Python pytorch-ddp-test.py a la forme suivante :

====PyTorch Lightning====
Ce paquet fournit des interfaces à PyTorch afin de simplifier plusieurs tâches communes exigeant beaucoup de code; ceci inclut les tâches d'entraînement de modèles avec plusieurs GPU. Dans le tutoriel suivant avec PyTorch Lightning, nous reprenons le même exemple que ci-dessus, mais sans avoir explicitement recours à la classe DistributedDataParallel.

=== Parallélisme des données avec un seul GPU ===

Il n’est pas conseillé d’utiliser un GPU avec un modèle de taille relativement petite qui n’utilise pas une grande part de la mémoire du GPU et une part raisonnable de sa capacité de calcul; utilisez plutôt un ou plusieurs CPU. Par contre, profiter du parallélisme du GPU devient une bonne option si vous avez un tel modèle avec un très grand jeu de données et que vous voulez effectuer l’entraînement avec des lots de petite taille.

Dans l'exemple suivant, nous adaptons le code de la section précédente pour l'exécuter sur un seul GPU. Cette tâche est relativement simple : avec un lot de 512 images, notre modèle occupe environ 1Go de mémoire GPU et n'utilise qu'environ 6% de sa capacité de calcul pendant l'entraînement. Ce modèle ne devrait pas être entraîné sur un seul GPU de nos grappes. Cependant, avec le parallélisme des données, nous pouvons exécuter plusieurs copies (replicas) de ce modèle sur un seul GPU et optimiser l'utilisation des ressources, tout en obtenant un gain de vitesse appréciable. Nous utilisons le service Multi-Process (MPS) de Nvidia ainsi que MPI pour déployer efficacement plusieurs copies du modèle sur un seul GPU.

==Parallélisme de données entièrement fragmentées (FSDP)==
Similar to Deepspeed, Fully Sharded Data Parallelism (FSDP) enables distributed storage and computing of different elements of a training task - such as optimizer states, model weights, model gradients and model activations - across multiple devices, including GPU, CPU, local hard disk, and/or combinations of these devices. This "pooling" of resources, notably for storage, allows models with massive amounts of parameters to be trained efficiently, across multiple nodes.

Note that, with FSDP, a model layer that gets sharded across devices may be collected inside a single device during a forward or backward pass. You should not use FSDP if your model has layers that do not fit entirely in the memory of a single GPU. See the section on Tensor Parallelism to see how to deal with this case.

==Parallélisme tensoriel==
Tensor Parallelism (TP) is a model sharding approach that differs from FSDP in that the computation of a forward or backward pass through a model layer is split along with the layers' weights across multiple devices. In other words, while FSDP shards model weights across devices, it must still collect  shards together in the same device during certain computation steps. This introduces overhead from having to move model shards across devices, and it implies that individual FSDP layers, or sharded model blocks, must fit entirely in the memory of a single device. With TP on the other hand, computation steps are done locally in the device where a model shard is placed.

==Parallélisme de pipeline==
Pipeline Parallelism (PP) is a model sharding approach where the shards are groups of consecutive of layers of a model. Each shard, or block of sequential layers, gets placed on a different device, thus a forward or backward pass through the model means performing computations on each device in sequence. This means that the farther away a block of layers is from the current block being used in a computation at any given time, the longer the device hosting it will have to wait for its turn to perform any computations. To mitigate this, in PP, every input batch is broken into "micro-batches", which are fed to the model in sequence. This ensures all devices stay busy as the first micro-batch reaches the last model block.

=Créer des points de contrôle=
Que votre code s'exécute ou non pendant de longues périodes, il est recommandé de créer des points de contrôle pendant l'entraînement. Un point de contrôle est un instantané de votre modèle à un moment donné du processus d'entraînement (après un certain nombre d'itérations ou d'époques), enregistré sur disque et pouvant être chargé ultérieurement. C'est un moyen pratique de découper les tâches de longue durée en plusieurs tâches plus courtes, qui peuvent être allouées plus rapidement sur la grappe. C'est également un bon moyen d'éviter de perdre le déroulement en cas d'erreurs inattendues dans votre code ou de panne du nœud.

==Avec PyTorch Lightning==

Nous recommandons d'utiliser le paramètre de rappels (callbacks parameter) de la classe Trainer(). Dans l'exemple suivant, on demande à PyTorch de créer un point de contrôle à la fin de chacune des époques d'entraînement. Vérifiez que le chemin où créer le point de contrôle existe.

 callbacks = [pl.callbacks.ModelCheckpoint(dirpath="./ckpt",every_n_epochs=1)]
 trainer = pl.Trainer(callbacks=callbacks)
 trainer.fit(model)

Ce bout de code chargera un point de contrôle de ./ckpt (s'il en existe) et poursuivra l'entraînement à partir de ce point. Pour plus d'information, consultez la documentation PyTorch Lightning.

==Avec des boucles d'entraînement personnalisées==

Pour des exemples, consultez la documentation PyTorch.

== Pendant l’entraînement distribué ==

Les points de contrôle peuvent être utilisés pendant l’exécution d’un programme d’entraînement distribué. Avec PyTorch Lightning, aucun code supplémentaire n’est requis, autre que d’insérer le paramètre de rappels (callbacks parameter) comme mentionné ci-dessus. Cependant, si vous utilisez  DistributedDataParallel ou Horovod, les points de contrôle devront être créés par un seul processus (rank) de votre programme puisque tous les processus auront le même état après chaque itération. Dans cet exemple, le premier processus (rank 0) crée un point de contrôle.

 if global_rank == 0:
        torch.save(ddp_model.state_dict(), "./checkpoint_path")

Faites attention aux points de contrôle ainsi créés. Si un processus tente de charger un point de contrôle qui n’a pas encore été sauvegardé par un autre, des erreurs peuvent survenir ou de mauvais résultats peuvent être produits. Pour éviter ceci, vous pouvez ajouter une barrière à votre code pour faire en sorte que le processus qui crée le point de contrôle a terminé son écriture sur le disque avant que d’autres processus tentent de le charger. Remarquez aussi que torch.load essaiera par défaut de charger les tenseurs sur le GPU sur lequel ils étaient initialement sauvegardés, dans notre cas cuda:0. Pour éviter les problèmes, passez map_location à torch.load pour charger les tenseurs sur le GPU identifié par chaque processus.

 torch.distributed.barrier()
 map_location = f"cuda:{local_rank}"
 ddp_model.load_state_dict(
 torch.load("./checkpoint_path", map_location=map_location))

= Dépannage =

== Erreur CUDA : no kernel image is available for execution on the device ==

Cette exception signifie que l'installation courante de Torch ne prend pas en charge l'architecture de calcul ou le GPU utilisé.
Vous pouvez installer une version plus récente de torch ou demander un GPU compatible avec la version que vous utilisez.

= LibTorch =

LibTorch permet d'implémenter à PyTorch des extensions C++ et des applications d'apprentissage machine en C++ pur.  La distribution LibTorch possède les en-têtes, bibliothèques et fichiers de configuration CMake nécessaires pour travailler avec PyTorch, tel que décrit dans la documentation.

=== Utiliser LibTorch ===

==== Configurer l'environnement ====

Chargez les modules requis par LibTorch, puis installez PyTorch dans un environnement virtuel Python.

 module load StdEnv/2023 gcc cuda/12.2 cmake protobuf cudnn python/3.11 abseil  cusparselt  opencv/4.8.1
 virtualenv --no-download --clear ~/ENV && source ~/ENV/bin/activate
 pip install --no-index torch numpy

Vous devrez peut-être ajuster les versions des modules abseil, cusparselt et opencv, dépendant du paquet torch que vous utilisez. Pour savoir quelle version d'un module a été utilisée pour compiler le wheel Python, lancez la commande

 sed -n 's&^.*/\(\(opencv\abseil\cusparselt\)/[^/]*\).*&\1&p'  sort -u
|result=
abseil/20230125.3
cusparselt/0.5.0.1
opencv/4.8.1
}}

 module load gcc cuda/11.4 cmake protobuf cudnn python/3.10
 virtualenv --no-download --clear ~/ENV && source ~/ENV/bin/activate
 pip install --no-index torch numpy

==== Compiler un exemple simple ====

Créez les deux fichiers suivants :

Activez l'environnement virtuel Python, configurez le projet et compilez le programme.

 cmake -B build -S . -DCMAKE_PREFIX_PATH=$VIRTUAL_ENV/lib/python3.11/site-packages \
                     -DCMAKE_EXE_LINKER_FLAGS=-Wl,-rpath=$VIRTUAL_ENV/lib/python3.11/site-packages/torch/lib,-L$EBROOTCUDA/extras/CUPTI/lib64 \
                     -DCMAKE_SKIP_RPATH=ON -DTORCH_CUDA_ARCH_LIST="6.0;7.0;7.5;8.0;9.0"
 cmake --build build

 cmake -B build -S . -DCMAKE_PREFIX_PATH=$VIRTUAL_ENV/lib/python3.10/site-packages \
                     -DCMAKE_EXE_LINKER_FLAGS=-Wl,-rpath=$VIRTUAL_ENV/lib/python3.10/site-packages/torch/lib \
                     -DCMAKE_SKIP_RPATH=ON
 cmake --build build

Lancez le programme avec
 build/example

Pour tester une application avec CUDA, demandez une tâche interactive avec GPU.

= rTorch =

Pour installer rTorch à partir d'un nœud de connexion%nbsp;:

1. Chargez les modules requis.

2. Créez votre répertoire d'installation avec les instructions dans Installation pour une ou plusieurs versions de R :
~/.local/R/$EBVERSIONR/
}}

3. Installez la plus récente version disponible de rtorch.
"https://cloud.r-project.org/")'}}

4. Installez les dépendances.

5. Corrigez la bibliothèque partagée qui a été téléchargée (lantern).

6. Effectuez un test rapide de CPU.

7. Effectuez un test de GPU.
3500M --gpush100_1g.10gb:1 -- R -e 'torch::torch_tensor(1)$cuda()'
|result=
> torch::torch_tensor(1)$cuda();
torch_tensor
 1
[ CUDAFloatType{1} ]
}}

= Ressources =

https://pytorch.org/cppdocs/