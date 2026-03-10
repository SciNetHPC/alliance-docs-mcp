---
title: "ABINIT/fr"
url: "https://docs.alliancecan.ca/wiki/ABINIT/fr"
category: "General"
last_modified: "2026-03-05T22:24:07Z"
page_id: 10073
display_title: "ABINIT"
language: "fr"
---

ABINIT est une suite logicielle pour le calcul des propriétés optiques, mécaniques, vibrationnelles et autres propriétés observables des matériaux. Avec les équations de la théorie de la fonctionnelle de la densité (DFT), il est possible d’évoluer vers des applications plus avancées avec les théories des perturbations basées sur la DGT et plusieurs fonctions N-corps de Green (GW et DMFT). ABINIT peut calculer les molécules, les nanostructures et les solides, peu importe leur composition chimique. La suite offre plusieurs tables complètes et fiables de potentiels atomiques.

Pour connaître les versions disponibles, utilisez la commande module spider abinit. Exécutez ensuite la même commande avec un numéro de version (par exemple module spider abinit/8.4.4) pour savoir si d’autres modules doivent être chargés au préalable. Pour plus d’information, consultez Utiliser des modules.

== Données atomiques ==

Nous ne disposons pas de collection de données atomiques pour ABINIT. Pour obtenir les fichiers dont vous avez besoin, référez-vous à Atomic data files.

Puisque ces fichiers sont habituellement de moins de 1Mo, ils peuvent être directement téléchargés vers un nœud de connexion avec leur URL et wget. L’exemple suivant sert à télécharger le fichier des pseudopotentiels de l’hydrogène.

== Exemples de scripts ==

Vous trouverez des fichiers de données pour effectuer des tests et pour suivre les tutoriels ABINIT  à  $EBROOTABINIT/share/abinit-test/Psps_for_tests/
et$EBROOTABINIT/share/abinit-test/tutorial.

== Exemple de script ==

Les calculs plus substantiels que les tests ou les exercices du tutoriel devraient être soumis à l’ordonnanceur Slurm. Le script suivant est un exemple d’une tâche qui utilise 64 cœurs CPU dans deux nœuds pendant 48 heures, nécessitant 1024Mo de mémoire par cœur. Cet exemple peut être adapté selon vos cas particuliers.