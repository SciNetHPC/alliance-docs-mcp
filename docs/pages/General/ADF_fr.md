---
title: "ADF/fr"
url: "https://docs.alliancecan.ca/wiki/ADF/fr"
category: "General"
last_modified: "2022-07-20T14:44:16Z"
page_id: 5397
display_title: "ADF"
language: "fr"
---

==Introduction==
Attention : La suite ADF a été renommée AMS depuis la version de 2020. Cette nouvelle version comporte des changements importants, notamment dans les formats d'entrée et de sortie. Pour plus d'information, voir AMS.

La suite logicielle SCM (Software for Chemistry and Materials), à l'origine la suite ADF pour Amsterdam Density Functional, offre des applications très performantes pour la recherche en chimie computationnelle, notamment dans les domaines de la catalyse (homogène et hétérogène), la chimie inorganique, la chimie des éléments lourds, la biochimie et différents types de spectroscopie.

Les produits suivants sont disponibles :
*ADF
*ADF-GUI
*BAND
*BAND-GUI
*DFTB
*ReaxFF
*COSMO-RS
*QE-GUI
*NBO6

==Utiliser SCM sur Graham==
Le module adf est seulement installé sur Graham en raison de restrictions liées à l'octroi des licences. Pour connaître les versions disponibles, lancez la commande

 [name@server $] module spider adf

Pour les commandes en rapport avec les modules, voyez Utiliser des modules.

===Soumettre une tâche===

Les tâches soumises sur Graham sont ordonnancées par Slurm; pour les détails, consultez Exécuter des tâches.

====Tâche unique====
Le script suivant utilise un nœud entier; l'avant-dernière ligne charge la version 2019.305 et la dernière ligne appelle ADF directement.

Le fichier en entrée ci-dessous est utilisé dans le script.

==== Tâches multiples avec ADF ou BAND ====

Plusieurs calculs peuvent être groupés dans une même tâche avec un script semblable à celui-ci :

Le script suivant est identique à celui utilisé pour une tâche unique (mysub.sh), à l’exception de la dernière ligne qui appelle le script GO_H2O.run plutôt que d’appeler ADF directement.

===Exemples===
Pour des exemples d’entrée/sortie pour ADF, voyez sur Graham
 /home/jemmyhu/tests/test_ADF/2019.305/test_adf/

Pour des exemples de fichiers .inp et .sh avec BAND, voyez sur Graham
 /home/jemmyhu/tests/test_ADF/2019.305/test_band

==Utiliser SCM-GUI==
Avec des applications comme ADF-GUI, la redirection X11 via une connexion SSH exige beaucoup de temps pour produire les rendus. Nous recommandons de vous connecter avec VNC.

===Graham===

Sur un nœud de calcul de Graham, ADF peut être utilisé interactivement en mode graphique avec TigerVNC pour une durée maximale de 3 heures.

# Installez un client TigerVNC sur votre ordinateur.
# Connectez-vous à un nœud de calcul avec vncviewer.
# module load adf
# adfinput

===Gra-vdi===

Sur gra-vdi, ADF peut être utilisé interactivement en mode graphique, sans limite de durée.

# Installez un client TigerVNC sur votre ordinateur.
# Connectez-vous à gra-vdi.computecanada.ca avec vncviewer.
# module load clumod
# module load adf
# adfinput

Voyez ce tutoriel sur comment utiliser ADF-GUI avec TigerVNC sur gra-vdi (en anglais).

===Utiliser ADF-GUI localement===
SCM offre une licence distincte pour utiliser ADF-GUI sur un ordinateur de bureau local; pour acquérir votre propre licence, contactez license@scm.com.