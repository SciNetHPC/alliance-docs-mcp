---
title: "Comet.ml/fr"
url: "https://docs.alliancecan.ca/wiki/Comet.ml/fr"
category: "General"
last_modified: "2025-08-29T20:28:01Z"
page_id: 12623
display_title: "Comet.ml"
language: "fr"
---

Comet est une plateforme de méta-apprentissage machine qui permet de construire des modèles pour des applications concrètes et d’en faciliter le développement et la maintenance. La plateforme permet de suivre, comparer, décrire et reproduire les expériences, et accélère grandement la recherche d’hyperparamètres grâce à son module d’exploration bayésienne.

== Utilisation sur nos grappes ==

=== Disponibilité ===

Puisqu’une connexion internet est requise, l’utilisation de Comet est restreinte à certaines grappes.

Grappe   	Wandb disponible	Commentaire
Narval   	oui ✅           	module load httpproxy requis
Rorqual  	oui ✅           	module load httpproxy requis
TamIA    	oui ✅           	module load httpproxy requis
Fir      	oui ✅           	httpproxy non requis
Nibi     	oui ✅           	httpproxy non requis
Trillium 	non ❌           	accès internet désactivé sur les nœuds de calcul
Vulcan   	oui ✅           	httpproxy non requis
Killarney	oui ✅           	httpproxy non requis

=== Meilleures pratiques ===

* Évitez de faire des requêtes au serveur de Comet à trop haute fréquence, car Comet pourrait limiter le débit, et rendre imprévisible la durée de la tâche. Interagissez avec Comet à des intervalles de >= 1 minute.