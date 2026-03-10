---
title: "Weights & Biases (wandb)/fr"
url: "https://docs.alliancecan.ca/wiki/Weights_%26_Biases_(wandb)/fr"
category: "General"
last_modified: "2025-09-02T19:53:32Z"
page_id: 15991
display_title: "Weights & Biases (wandb)"
language: "fr"
---

Weights & Biases (wandb) est une plateforme de méta-apprentissage machine qui permet de construire des modèles pour des applications concrètes. La plateforme permet de suivre, comparer, décrire et reproduire les expériences d'apprentissage machine.

== Utilisation sur nos grappes ==

=== Disponibilité sur les nœuds de calcul ===

L'utilisation de toutes les fonctionnalités de wandb sur les nœuds de calcul nécessite un accès Internet ainsi qu'un accès à Google Cloud Storage, qui peuvent tous deux ne pas être disponibles selon la grappe :

Grappe   	Wandb disponible	Commentaire
Narval   	limité ❌        	réservées aux membres de Mila et autres groupes admissibles via httpproxy
Rorqual  	limité ❌        	réservées aux membres de Mila et autres groupes admissibles via httpproxy
TamIA    	limité ❌        	réservées aux membres de Mila et autres groupes admissibles via httpproxy
Fir      	oui ✅           	httpproxy non requis
Nibi     	oui ✅           	httpproxy non requis
Trillium 	non ❌           	accès internet désactivé pour les nœuds de calcul
Vulcan   	oui ✅           	httpproxy non requis
Killarney	oui ✅           	httpproxy non requis

== Membres de Mila et autres groupes admissibles ==

Les membres de l'Institut québécois d’intelligence artificielle Mila peuvent utiliser wandb sur n'importe laquelle de nos grappes offrant un accès Internet, à condition d'utiliser un compte Mila-org valide pour se connecter à la plateforme. Veuillez consulter le tableau ci-dessus pour plus d'informations sur les modules requis pour utiliser wandb sur chaque grappe.

D'autres groupes ont pris des dispositions avec Weights & Biases pour contourner les appels à l'API Google Cloud Storage. Contactez votre chercheuse principale ou votre chercheur principal pour savoir si c'est le cas pour votre groupe.

== Narval, Rorqual et TamIA ==

Bien qu'il soit possible de télécharger des métriques de base vers Weights&Biases lors d'une tâche sur Narval, Rorqual et TamIA, le package wandb tentera automatiquement de télécharger les informations relatives à votre environnement vers un bucket Google Cloud Storage, ce qui n'est pas autorisé sur les nœuds de calcul de ces grappes et qui causera un plantage pendant ou à la toute fin d'une tâche. Votre tâche peut également se bloquer jusqu'à ce qu'elle atteigne son temps mort, gaspillant ainsi des ressources. Il n'est actuellement pas possible de désactiver ce comportement. Notez que le téléchargement d'artefacts vers W&B avec wandb.save() nécessite également un accès à Google Cloud Storage et bloquera ou fera planter votre tâche.

Vous pouvez quand même utiliser wandb en activant les modes offline ou dryrun. Avec ces modes, wandb écrit tous les métriques, journalisations et artefacts sur le disque local, sans synchronisation avec le service internet Weights&Biases. Une fois les tâches terminées, vous pouvez faire la synchronisation avec la commande wandb sync sur le nœud de connexion.

Remarquez que le produit Comet.ml est très semblable à Weights & Biases et qu'il fonctionne sur Narval, Rorqual et TamIA.

== Exemple ==

Voici un exemple d'utilisation de wandb pour faire le suivi d'expériences en mode hors ligne. Pour exécuter en ligne, chargez le module httpproxy sur les grappes et tenez compte des commentaires dans le script ci-dessous.

Le script wandb-test.py est un exemple simple de journalisation des métriques. Pour d'autres options, voyez la documentation complète de W&B.

Après que l'entraînement a été effectué en mode hors ligne, vous aurez le nouveau répertoire ./wandb/offline-run*. Pour envoyer les métriques au serveur, utilisez la commande wandb sync ./wandb/offline-run* où l'astérisque synchronise toutes les exécutions.