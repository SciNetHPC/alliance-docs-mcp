---
title: "NVTOP/fr"
url: "https://docs.alliancecan.ca/wiki/NVTOP/fr"
category: "General"
last_modified: "2024-12-10T18:50:20Z"
page_id: 27037
display_title: "NVTOP"
language: "fr"
---

NVTOP (pour  Neat Videocard TOP) est un outil de type htop et top servant à surveiller l'utilisation de GPU et d'accélérateurs.

Une image qui vaut mille mots

__FORCETOC__

= Utilisation de GPU =
NVTOP peut gérer un ou plusieurs GPU et montrer leur utilisation et leur mémoire.
Vous pouvez aussi sélectionner un autre accélérateur à partir du menu (F2 -> GPU Select).

NVTOP est utile pour suivre et vérifier que votre tâche fait la meilleure utilisation des GPU.

== Tâches non interactives ==
Si vous avez soumis une tâche qui n'est pas interactive et voulez voir son utilisation du GPU,

1. Depuis un nœud de connexion, trouvez l'ID de la tâche.

2. Visionnez l'utilisation.

== Tâches interactives ==
1. Lancez votre tâche interactive avec le moins de ressources possible.

2. Dans un autre terminal, connectez-vous au nœud de connexion et trouvez l'ID de la tâche.

3. Visionnez l'utilisation.

Vous pouvez voir l'utilisation en temps réel au fur et à mesure que vos commandes sont exécutées dans le premier terminal.

== Utilisation de GPU sur un nœud particulier ==
Avec les tâches qui utilisent plusieurs nœuds, vous pouvez vérifier qu'un ou plusieurs GPU sont utilisés le plus efficacement possible.

1. Depuis un nœud de connexion, trouvez l'ID de la tâche et trouvez le nom des nœuds.

2. Visionnez l'utilisation.