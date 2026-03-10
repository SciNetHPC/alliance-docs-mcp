---
title: "JupyterHub/fr"
url: "https://docs.alliancecan.ca/wiki/JupyterHub/fr"
category: "General"
last_modified: "2026-02-09T14:35:25Z"
page_id: 4861
display_title: "JupyterHub"
language: "fr"
---

JupyterHub est le meilleur système pour que plusieurs personnes puissent utiliser simultanément Jupyter Notebook, qu'il s'agisse d'un groupe dans un contexte d'enseignement ou de recherche, ou dans une entreprise de science des données.
http://jupyterhub.readthedocs.io/en/latest/index.html

JupyterHub offre une version préconfigurée de JupyterLab et/ou Jupyter Notebook; pour plus d'information sur les options de configuration, consultez la  page Jupyter.

=Initiatives de l'Alliance=

Notre réseau comprend quelques hubs qui permettent l'accès aux ressources de calcul de pointe.

== JupyterHub sur une grappe ==

Utilisez votre nom d'utilisateur et votre mot de passe de de votre compte avec l'Alliance pour vous connecter aux grappes suivantes :‡.

JupyterHub	Commentaires
Fir       	Donne accès aux serveurs JupyterLab générés par des tâches interactives qui sont lancées à même l’interface web.
Narval    	Donne accès aux serveurs JupyterLab générés par des tâches interactives qui sont lancées à même l’interface web.
Rorqual   	Donne accès aux serveurs JupyterLab générés par des tâches interactives qui sont lancées à même l’interface web.

Certaines grappes permettent l'accès à JupyterLab via Open OnDemand. Ppour plus d'information, voir JupyterLab.

‡  Les nœuds de calcul sur lesquels les noyaux (kernels) Jupyter sont activés n'ont pas accès à l'internet. En conséquence, vous pouvez seulement copier des fichiers vers et à partir de votre propre ordinateur. Vous ne pouvez pas télécharger du code ou des données de l'internet par exemple avec git clone ou pip install si le wheel ne se trouve pas dans notre wheelhouse. Aussi, des problèmes pourraient survenir si votre code effectue des téléchargements ou des téléversements, dans le cas par exemple de l'apprentissage machine où les données sont souvent téléchargées à partir du code.

== JupyterHub pour les universités et les écoles ==

* En collaboration avec l'Alliance et Cybera, le Pacific Institute for the Mathematical Sciences offre des hubs infonuagiques aux établissements d'enseignement. Chacun peut avoir son propre hub auquel les utilisateurs accèdent via leur compte d'établissement. Les hubs sont hébergés par notre service infonuagique et servent essentiellement à des fins de formation. Les établissements souhaitant obtenir un hub peuvent consulter Syzygy.

= Options pour le serveur =

Une fois la connexion établie et selon la configuration de JupyterHub, le navigateur web est redirigé vers
a) un serveur Jupyter précédemment lancé,
b) un nouveau serveur Jupyter possédant des options par défaut, ou
c) un formulaire permettant de configurer les options du serveur Jupyter avant d'appuyer sur le bouton Start.
Dans tous les cas, c'est l'équivalent d'accéder aux ressources demandées via  une tâche interactive sur la grappe correspondante.

Important : Sur chaque grappe, une seule tâche interactive à la fois obtient une plus haute priorité pour commencer à l'intérieur de quelques secondes ou quelques minutes. Ceci inclut les tâches exécutées via salloc, srun et les tâches JupyterHub. Si vous avez une autre tâche interactive en exécution sur la grappe où se trouve JupyterHub, votre nouvelle session Jupyter pourrait ne pas commencer avant la limite de 5 minutes.

== Ressources de calcul==

Par exemple, les options pour JupyterHub sur Béluga sont :
* Account : vous pouvez utiliser un compte de calcul de type def-*, rrg-*, rpp-* ou ctb-* auquel vous avez accès;
* Time (hours) : nombre d'heures requises pour la session;
* Number of cores : nombre de CPU réservés sur un seul nœud;
* Memory (MB) : limite de mémoire vive totale pour toute la session;
* GPU configuration (optionnel) : au moins un GPU;
*  Interface utilisateur (voir ci-dessous).

== Interface utilisateur ==

JupyterHub permet d'avoir accès à un serveur à la fois, mais plusieurs interfaces peuvent être offertes sous User Interface :
*  JupyterLab (interface moderne) : cette interface Jupyter est la plus recommandée pour le prototypage interactif et la visualisation des données;
* Jupyter Notebook (interface classique) : cette interface offre beaucoup de fonctionnalités, mais la plupart des utilisateurs choisissent désormais JupyterLab qui est une meilleure plateforme et qui possède beaucoup plus de caractéristiques;
* Terminal (pour un terminal unique) : cette interface donne accès à un terminal connecté à un compte à distance, ce qui se compare à se connecter à un serveur via SSH.

Remarque : JupiterHub peut aussi être configuré pour afficher une interface spécifique, par exemple dans le cas d'un événement spécial.

= JupyterLab =

La description de l'interface JupyterLab se trouve maintenant à la page JupyterLab.

= Messages d'erreur =

== Spawn failed: Timeout ==

Les erreurs avec JupyterHub sont généralement causées par l'ordonnanceur de tâches sous-jacent qui ne répond pas ou qui est incapable de trouver les ressources appropriées pour votre session, par exemple

* Au lancement d'une nouvelle session, JupyterHub soumet automatiquement à la grappe une nouvelle  tâche interactive. Si la tâche ne démarre pas dans les cinq prochaines minutes, ce message est affiché et la session est annulée.
** Comme c'est le cas pour toutes les tâches interactives sur une grappe, le fait de demander plus de temps d'exécution peut entraîner une attente plus longue avant que la tâche puisse démarrer, ce qui peut aussi se produire quand vous demandez un GPU ou trop de cœurs CPU. Assurez-vous de demander uniquement les ressources dont vous avez besoin.
** Si vous avez une autre tâche interactive sur la même grappe, votre session Jupyter sera placée en file d'attente avec les autres tâches en lots. Si c'est possible, arrêtez ou annulez les autres tâches interactives avant d'utiliser JupyterHub.
** Il est possible qu'aucune ressource ne soit disponible à ce moment. Vérifiez si un problème est rapporté dans la page de l'État des systèmes et essayez de nouveau plus tard.

== Erreur d'authentification 403 ==

Cette erreur survient quand votre compte ou votre accès à la grappe n'est plus valide.
# Vérifiez si votre compte a été renouvelé et qu'il est actif
# Assurez-vous  d'avoir activé votre accès à la grappe.

= Références =