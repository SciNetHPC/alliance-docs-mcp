---
title: "MAFFT/fr"
url: "https://docs.alliancecan.ca/wiki/MAFFT/fr"
category: "General"
last_modified: "2020-08-03T16:35:36Z"
page_id: 14120
display_title: "MAFFT"
language: "fr"
---

MAFFT est un programme d'alignement de séquences multiples pour des systèmes d'exploitation comme Unix. Il offre plusieurs méthodes d'alignement dont (précis, pour l'alignement de <∼200 séquences), FFT-NS-2 (rapide, pour l'alignement de <∼30,000 séquences), etc.

== Nœud unique ==
MAFFT profite de cœurs multiples sur des nœuds uniques; voir https://mafft.cbrc.jp/alignment/software/multithreading.html.

Note : Au chargement du module, la variable d'environnement MAFFT_TMPDIR est fixée à $SLURM_TMPDIR/maffttmp.

== Nœuds multiples (MPI) ==
MAFFT peut utiliser MPI pour aligner un grand nombre de séquences; voir https://mafft.cbrc.jp/alignment/software/mpi.html.

Note :  Au chargement du module, la variable d'environnement MAFFT_TMPDIR est fixée à $SLURM_TMPDIR/maffttmp. Si vous changez de répertoire temporaire, il devra être partagé par tous les hôtes.