---
title: "CASTEP/fr"
url: "https://docs.alliancecan.ca/wiki/CASTEP/fr"
category: "General"
last_modified: "2024-11-05T17:58:23Z"
page_id: 20335
display_title: "CASTEP"
language: "fr"
---

==Installation==
Par exemple avec la version 20.11%nbsp;:
# Trouvez le fichier archive qui contient l'installateur; le fichier devrait se nommer CASTEP-20.11.tar.gz.
# Téléversez le fichier CASTEP-20.11.tar.gz dans votre répertoire /home/$USER sur la grappe que vous voulez utiliser.
# Sur la grappe, lancez la commande
 [name@server ~]$ eb CASTEP-20.11-iofbf-2020a.eb --sourcepath=$HOME --disable-enforce-checksums
Une fois que la commande est terminée, déconnectez-vous de la grappe et connectez-vous de nouveau.

==Utilisation==
Vous devriez pouvoir charger le module avec
 [name@server ~]$ module load castep
Sur un nœud de calcul, l'exécutable CASTEP peut être utilisé comme une application MPI
 [name@server ~]$ srun castep.mpi seedname
où les fichiers d'entrée seraient seedname.cell et seedname.param (un autre mot peur remplacer seedname).

==Référence==
* Documentation de CASTEP