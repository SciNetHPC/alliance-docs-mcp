---
title: "MXNet/fr"
url: "https://docs.alliancecan.ca/wiki/MXNet/fr"
category: "General"
last_modified: "2022-11-11T17:16:02Z"
page_id: 11095
display_title: "MXNet"
language: "fr"
---

MXNet est une librairie en apprentissage profond à la fois souple et efficace qui permet de combiner la programmation symbolique et impérative pour maximiser l'efficacité et la productivité. À la base, MXNet contient un planificateur de dépendances dynamique qui parallélise automatiquement les opérations symboliques et impératives comme elles se présentent. Une couche supérieure d'optimisation des graphes rend l'exécution symbolique rapide et économe en mémoire. MXNet est portable et léger, évolutif pour de nombreux GPU et machines.

= Paquets disponibles=
Pour savoir quels paquets sont disponibles, utilisez la commande avail_wheels.

= Installation dans un environnement virtuel Python =
1. Créez et activez un environnement virtuel Python.

2. Installez MXNet et ses dépendances Python.

3. Validez.

= Exécution d'une tâche =
Couche simple pour les convolutions :

2. Modifiez le script suivant selon les besoins de votre tâche.

3. Soumettez la tâche à l'ordonnanceur.

= Haute performance =

== Utiliser plusieurs CPU ou un seul GPU ==

Tout comme PyTorch et TensorFlow, MXNet offre des implémentations semblables d'opérateurs pour le travail avec CPU et GPU, dont les multiplications matricielles et les convolutions, soit avec OpenMP et MKLDNN (CPU) ou CUDA et CUDNN (GPU). Que votre code effectue ou non ces opérations, elles utiliseront automatiquement le mode multifils sur plusieurs coeurs CPU ou utiliser un GPU si la tâche le demande.

Ceci étant dit, nous vous encourageons fortement à utiliser plusieurs CPU plutôt qu'un seul GPU. Si votre modèle et votre ensemble de données ne sont pas assez grands, l'entraînement sera certainement plus rapide avec un GPU (sauf dans le cas de très petits modèles), mais la différence avec la vitesse obtenue par les CPU ne sera pas bien importante et la tâche n'utilisera qu'un faible pourcentage de la capacité de calcul du GPU. Ce n'est peut-être pas un problème pour votre ordinateur, mais dans un environnement partagé comme nos grappes, vous bloquez sans raison valable une ressource qui pourrait être utilisée par d'autres pour effectuer des calculs à grande échelle, sans compter que vous utilisez l'allocation de  votre groupe et avez un effet négatif sur la priorité accordée aux tâches de vos collègues.

Autrement dit, ne demandez pas un GPU si votre code est incapable de faire un usage raisonnable de sa capacité de calcul. L'exemple suivant montre l'entraînement d'un réseau neuronal convolutif avec ou sans GPU.