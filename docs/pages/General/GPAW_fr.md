---
title: "GPAW/fr"
url: "https://docs.alliancecan.ca/wiki/GPAW/fr"
category: "General"
last_modified: "2023-04-21T20:21:11Z"
page_id: 21751
display_title: "GPAW"
language: "fr"
---

__TOC__
= Description =
GPAW est un code de théorie de la fonctionnelle de la densité (DFT) Python basé sur
la méthode des ondes augmentées par projecteur (PAW) et l'environnement de simulation atomique (ASE).

= Créer un environnement virtuel GPAW =
Nous offrons des wheels Python précompilés pour GPAW qui peuvent être installés dans un  environnement virtuel Python.

1. Vérifiez quelles versions sont disponibles.

2. Chargez un module Python (ici python/3.10)

3. Créez un nouvel environnement virtuel.

4. Activez l'environnement virtuel (venv).

5. Installez gpaw dans venv.

6. Téléchargez les données et installez-les dans le système de fichiers SCRATCH.

7. Configurez GPAW_SETUP_PATH pour pointer vers le répertoire des données.
$SCRATCH/gpaw-setups-0.9.20000
}}

8. Lancez les tests, qui sont très rapides.
 python-3.10.2     /home/name/venv_gpaw/bin/python
 gpaw-22.8.0       /home/name/venv_gpaw/lib/python3.10/site-packages/gpaw/
 ase-3.22.1        /home/name/venv_gpaw/lib/python3.10/site-packages/ase/
 numpy-1.23.0      /home/name/venv_gpaw/lib/python3.10/site-packages/numpy/
 scipy-1.9.3       /home/name/venv_gpaw/lib/python3.10/site-packages/scipy/
 libxc-5.2.3       yes
 _gpaw             /home/name/venv_gpaw/lib/python3.10/site-packages/_gpaw.cpython-310-x86_64-linux-gnu.so
 MPI enabled       yes
 OpenMP enabled    yes
 scalapack         yes
 Elpa              no
 FFTW              yes
 libvdwxc          no
 PAW-datasets (1)  /scratch/name/gpaw-setups-0.9.20000
 -----------------------------------------------------------------------------------------------------------
Doing a test calculation (cores: 1): ... Done
Test parallel calculation with "gpaw -P 4 test".
}}

 python-3.10.2     /home/name/venv_gpaw/bin/python
 gpaw-22.8.0       /home/name/venv_gpaw/lib/python3.10/site-packages/gpaw/
 ase-3.22.1        /home/name/venv_gpaw/lib/python3.10/site-packages/ase/
 numpy-1.23.0      /home/name/venv_gpaw/lib/python3.10/site-packages/numpy/
 scipy-1.9.3       /home/name/venv_gpaw/lib/python3.10/site-packages/scipy/
 libxc-5.2.3       yes
 _gpaw             /home/name/venv_gpaw/lib/python3.10/site-packages/_gpaw.cpython-310-x86_64-linux-gnu.so
 MPI enabled       yes
 OpenMP enabled    yes
 scalapack         yes
 Elpa              no
 FFTW              yes
 libvdwxc          no
 PAW-datasets (1)  /scratch/name/gpaw-setups-0.9.20000
 -----------------------------------------------------------------------------------------------------------
Doing a test calculation (cores: 4): ... Done
}}

Les résultats du dernier test se trouvent dans le fichier test.txt qui se trouvera dans le répertoire courant.

= Exemple de script =
Le script suivant est un exemple de parallélisation hybride OpenMP et MPI.
Ici, virtualenv se trouve dans votre répertoire $HOME et les ensembles de données sont dans  $SCRATCH comme ci-dessus.

Le scrip utilise un nœud simple avec 8 rangs MPI (ntasks) et 4 fils OpenMP par rang MPI  pour un total de 32 CPU.
Vous voudrez probablement modifier ces valeurs pour que le produit corresponde au nombre de cœurs d'un nœud entier
(soit 32 sur Graham, 40 sur Béluga et Niagara, 48 sur Cedar ou 64 sur Narval).

Le fait de configurer  OMP_NUM_THREADS comme expliqué ci-dessus fait en sorte qu'il a toujours la même valeur que cpus-per-task ou 1 quand cpus-per-task n'est pas défini.
Le chargement des modules gcc/9.3.0 et  openmpi/4.0.3 fait en sorte que la bonne bibliothèque MPI est utilisée pour la tâche, la même qui a été utilisée pour construire les wheels.