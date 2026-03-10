---
title: "Prolonging terminal sessions/fr"
url: "https://docs.alliancecan.ca/wiki/Prolonging_terminal_sessions/fr"
category: "General"
last_modified: "2023-07-04T16:40:47Z"
page_id: 22809
display_title: "Prolonger une session dans un terminal"
language: "fr"
---

Pour soumettre et faire le suivi des tâches, modifier des fichiers et plusieurs autres opérations, vous aurez sans doute besoin de vous connecter à une grappe via SSH. Il est parfois nécessaire de garder la connexion active pendant plusieurs heures, même plusieurs jours. Nous décrivons ici certaines techniques pour ce faire.

=Configuration de SSH=

Une solution simple pour prolonger une connexion est de modifier la configuration de votre client SSH. Avec macOS et Linux, cette configuration se trouve dans $HOME/.ssh/config alors qu'avec Windows elle est dans C:\Users\\.ssh\config. Si le fichier n'existe pas, vous devez le créer at ajouter les lignes

Host *
    ServerAliveInterval 240

Ceci transmet un signe de vie au serveur distant (comme une grappe de l'Alliance) à toutes les 240 secondes (4 minutes), ce qui devrait garder la connexion vivante même si elle est inactive pendant quelques heures.

=Multiplexeur de terminal=

Les programmes tmux et screen sont des exemples de multiplexeurs de terminal qui vous permettent de complètement détacher une session de terminal qui restera active jusqu'à ce que vous vous y rattachiez.  Vous pouvez donc vous déconnecter de la grappe, fermer le poste de travail ou le mettre en veille, puis reprendre le travail le lendemain en vous rattachant à la même session.

==tmux==

Le logiciel tmux est un multiplexeur de terminal qui permet plusieurs sessions virtuelles à l'intérieur d'une même session de terminal. Vous pouvez donc vous déconnecter d'une session SSH sans que les processus soient affectés.

Pour une introduction à tmux (en anglais) :
* The Tao of tmux
* Getting Started With TMUX, vidéo de 24 minutes
* Turbo boost your interactive experience on the cluster with tmux, vidéo de 58 minutes

===Aide-mémoire===

Voyez la documentation complète (en anglais).

Commande	Description
tmux    	Démarrer le serveur
Ctrl+B D	Se déconnecter du serveur
tmux a  	Se reconnecter au serveur
Ctrl+B C	Créer une nouvelle fenêtre
Ctrl+B N	Aller à la prochaine fenêtre
Ctrl+B [	Activer le mode copie pour défilement avec la souris et les touches page suivante et page précédente
Esc     	Désactiver le mode copie

===Utiliser tmux dans une tâche soumise par tmux===

Si vous utilisez tmux pour soumettre une tâche et que vous tentez de lancer tmux à l’intérieur de cette tâche, vous obtiendrez le message d'erreur lost server. Ceci est dû au fait que la variable d'environnement $TMUX, qui pointe vers le serveur tmux sur le nœud de connexion, est propagée à la tâche. La valeur de la variable n'est donc pas valide. Vous pouvez la réinitialiser avec

Cependant, l'usage de deux (ou plus) niveaux de tmux n'est pas recommandé. Pour envoyer des commandes à un tmux imbriqué, il faut taper deux fois les touches Ctrl+B; par exemple, pour créer une fenêtre, il faut taper Ctrl+B Ctrl+B C. Considérez plutôt d'utiliser GNU Screen (ci-dessous) à l'intérieur de vos tâches (si vous utilisez tmux sur un nœud de connexion).

==GNU Screen==

Le programme GNU Screen est un autre multiplexeur de terminal souvent utilisé. Créez une session de terminal détachée avec

Donnez à vos sessions des noms faciles à retenir. Pour voir la liste des sessions détachées sur un nœud, utilisez la commande screen -list.

Pour vous attacher à une de vos sessions, utilisez screen -d -r .