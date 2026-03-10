---
title: "AMS/fr"
url: "https://docs.alliancecan.ca/wiki/AMS/fr"
category: "General"
last_modified: "2025-09-23T22:32:29Z"
page_id: 15895
display_title: "AMS"
language: "fr"
---

==Introduction==
AMS (Amsterdam Modeling Suite), est la nouvelle appellation de ADF (Amsterdam Density Functional) et fait partie de la suite SCM Software for Chemistry and Materials. AMS offre des outils très performants pour la recherche en chimie computationnelle, notamment dans les domaines de la catalyse (homogène et hétérogène), la chimie inorganique, la chimie des éléments lourds, la biochimie et différents types de spectroscopie.

Tous les produits du module SCM sont disponibles :
*ADF
*ADF-GUI
*BAND
*BAND-GUI
*DFTB
*ReaxFF
*COSMO-RS
*QE-GUI
*NBO6

==Utiliser AMS sur Nibi==
Le module ams est installé sur Nibi. SHARCNET est propriétaire de cette licence qui est réservée aux centres de calcul universitaires; cette licence ne peut être utilisée pour des services de consultation ou pour tout autre usage de nature commerciale. Pour connaître les versions disponibles, lancez la commande

 [name@server $] module spider ams

Pour les commandes en rapport avec les modules, voyez Utiliser des modules.

===Soumettre une tâche===

Les tâches soumises sur nos grappes sont ordonnancées par Slurm; pour les détails, consultez Exécuter des tâches.

====Exemples de scripts pour une tâche AMS ====
Le script suivant demande 32 CPU sur un nœud. Veuillez utiliser un nombre raisonnable de CPU au lieu de simplement exécuter une tâche sur un nœud complet de Nibi, à moins que vous n'ayez démontré que votre tâche peut utiliser efficacement 192 CPU.

Le fichier en entrée ci-dessous est utilisé dans le script.

====Exemples de scripts pour une tâche band ====

===Remarques===
# Le fichier en entrée pour AMS est différent de celui pour ADF; le fichier en entrée précédent pour ADF ne fonctionnera pas avec le nouveau AMS. Vous trouverez des exemples dans  /opt/software/ams/2025.102/examples/.
# À l'exception du fichier en sortie .log, les fichiers sont tous sauvegardés dans le sous-répertoire AMS_JOBNAME.results. Si AMS_JOBNAME n'est pas défini dans le fichier en entrée .run, le nom par défaut sera ams.results.
# Le nom du fichier de point de sauvegarde est ams.rkf plutôt que TAPE13 dans les versions ADF précédentes.
Pour plus d'information, consultez SCM Support.

==AMS-GUI==
===Nibi===

Sur un nœud de calcul de Nibi (durée limite de 8 heures), AMS peut être utilisée interactivement en mode graphique via OnDemand en suivant ces étapes :

# Connectez-vous à ondemand.sharcnet.ca.
# Sélectionnez Nibi Desktop dans Compute' (dans le haut).
# Sélectionnez vos options (sélectionnez 1 cœur pour une visualisation; ne sélectionnez pas Enable VirtualGL) et cliquez sur Launcher.
# Sélectionnez Launcher Nibi Desktop une fois que la tâche est lancée.
# Faites un clic droit sur le bureau et sélectionnez Open it Terminal.
# Sélectionnez MATE Terminal dans le menu System Tools sous Applications.
# module unload openmpi
# module load ams
# amsinput &  (créer des fichers d'entrée)
# amsview & (visualiser les résultats)

Si vous devez sélectionner Enable VirtualGL pour un autre programme que vous utilisez, vous devez d'abord le désactiver pour AMS en le lançant avec LD_PRELOAD= amsinput.

Sur OnDemand, Nibi Desktop est utilisé pour exécuter des applications AMS-GUI pour, par exemple, créer les fichiers d'entrée et visualiser les résultats. Veuillez ne pas y exécuter de tâches régulières ou des tâches interactives de longue durée. Sélectionnez un seul cœur ainsi qu'une quantité de mémoire et un temps d'exécution raisonnables.