---
title: "CUDA/fr"
url: "https://docs.alliancecan.ca/wiki/CUDA/fr"
category: "General"
last_modified: "2026-01-22T13:46:02Z"
page_id: 6120
display_title: "CUDA"
language: "fr"
---

[CUDA] Marque de commerce de NVIDIA.est une plateforme de calcul parallèle et un modèle de programmation développé par NVIDIA pour des calculs généraux utilisant le GPU.

On peut voir CUDA comme étant un ensemble de bibliothèques et de compilateurs C, C++ et Fortran qui permettent de créer des programmes pour les GPU. Pour d'autres outils de programmation pour GPU, consultez le Tutoriel OpenACC.

== Un exemple simple ==

===Compilation===
Nous faisons exécuter ici du code créé avec le compilateur CUDA C/C++ nvcc. Ce même exemple plus détaillé se trouve à la page Tutoriel CUDA.

Chargez d'abord le module CUDA.

$ module purge
$ module load cuda

Dans cet exemple, nous additionnons deux nombres. Sauvegardez le fichier sous add.cu; le suffixe cu est important.

nvcc pour créer l'exécutable add.

$ nvcc add.cu -o add

===Soumission de tâches===
Pour exécuter le programme, créez le script Slurm ci-dessous. Assurez-vous de remplacer def-someuser par votre nom de compte (voir Comptes et projets). Pour les détails sur l'ordonnancement, consultez Ordonnancement Slurm des tâches avec GPU.

Soumettez la tâche à l'ordonnanceur.

$ sbatch gpu_job.sh
Submitted batch job 3127733

Pour plus d'information sur la commande sbatch, l'exécution et le suivi des tâches, consultez Exécuter des tâches.

Le fichier en sortie sera semblable à ceci ː

$ cat slurm-3127733.out
2+7=9

Sans GPU, le résultat serait semblable à 2+7=0.

=== Lier des bibliothèques ===
Si votre programme doit établir des liens avec des bibliothèques incluses avec CUDA, par exemple cuBLAS, compilez avec ces indicateurs :

nvcc -lcublas -Xlinker=-rpath,$CUDA_PATH/lib64

Voyez le Tutoriel CUDA pour plus de détails sur cet exemple et pour savoir comment utiliser le parallélisme avec les GPU.

== Dépannage ==

=== Attribut compute capability ===

NVIDIA utilise le terme compute capability pour désigner un des attributs des dispositifs GPU.

Nvidia a créé ce terme technique qui indique les fonctionnalités prises en charge par ce GPU et spécifie certains paramètres matériels de ce dernier.

Pour plus de détails, consultez Compute Capability and Streaming Multiprocessor Versions.

Les messages d’erreur suivants sont causés par un problème en rapport avec cet attribut.

nvcc fatal : Unsupported gpu architecture 'compute_XX'

no kernel image is available for execution on the device (209)

L'ajout d'un indicateur dans l'appel nvcc pourrait résoudre ces problèmes.

-gencode arch=compute_XX,code=[sm_XX,compute_XX]

Si vous utilisez cmake, l'indicateur serait

cmake .. -DCMAKE_CUDA_ARCHITECTURES=XX

où XX est la valeur de compute capability pour le GPU NVIDIA qui sera utilisé pour exécuter votre application. Pour connaître ces valeurs, voir CUDA GPU Compute Capability et omettez le point décimal.

Par exemple, si votre code sera exécuté sur un nœud A100 de Narval, le tableau de NVdia mentionne que sa compute capability a la valeur de 8.0.
L'indicateur à utiliser lors de la compilation avec nvcc est

-gencode arch=compute_80,code=[sm_80,compute_80]

L'indicateur pour cmake est

cmake .. -DCMAKE_CUDA_ARCHITECTURES=80