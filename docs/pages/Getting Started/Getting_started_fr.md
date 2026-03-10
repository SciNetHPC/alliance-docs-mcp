---
title: "Getting started/fr"
url: "https://docs.alliancecan.ca/wiki/Getting_started/fr"
category: "Getting Started"
last_modified: "2026-02-12T12:40:00Z"
page_id: 637
display_title: "Guide de démarrage"
language: "fr"
---

==Que voulez-vous faire?==
* Si vous ne possédez pas de compte, voyez
** Demander un compte CCDB;
** Authentification multifacteur
** voyez la page Foire aux questions sur le portail CCDB.
* Si vous avez de l'expérience en CHP et que vous voulez vous connecter à une grappe, vous voudrez savoir
** quels sont les systèmes disponibles;
** quels sont les logiciels disponibles et comment utiliser les modules;
** comment soumettre une tâche;
** comment les systèmes de fichiers sont organisés.
* Pour vous initier au CHP
** apprenez comment vous connecter par SSH à nos grappes de CHP;
** lisez cette introduction à Linux;
** voyez comment transférer des données soit vers nos systèmes, soit en provenance de ceux-ci.
* Pour connaître les ressources qui sont disponibles pour une discipline particulière, consultez les guides spécialisés :
** Intelligence artificielle et apprentissage machine
** Bio-informatique
** Simulation biomoléculaire
** Chimie computationnelle
** Mécanique des fluides numérique
** Systèmes d'information géographique
** Visualisation
* Si vous avez des centaines de gigaoctets de données à transférer entre les serveurs, lisez à propos du service de transfert Globus.
* Apprenez à installer des modules Python dans un environnement virtuel en lisant la page Python, sections Créer et utiliser un environnement virtuel et suivantes.
* Apprenez à installer des paquets R.
* Pour utiliser des logiciels qui ne sont pas conçus pour fonctionner sur nos systèmes traditionnels de CHP, vous pourriez utiliser l'environnement infonuagique.

Pour toute autre question, vous pouvez utiliser le champ de recherche dans le coin supérieur droit de la présente page, consulter notre documentation technique ou encore nous joindre par courriel.

=Quels sont les systèmes disponibles?=

Vous pouvez demander la permission d'accès à un ou plusieurs des systèmes suivants : Arbutus, Fir, Narval, Nibi, Rorqual et Trillium.
Quatre d'entre eux ont été installés en 2015 et un a été mis à jour; pour plus d'information, voir Renouvellement de l'infrastructure.

* Arbutus est un nuage pour configurer et exécuter des instances virtuelles. Pour savoir comment y accéder, voyez Service infonuagique.

Les grappes d'usage général (superordinateurs) Fir, Narval, Nibi et Rorqual comportant divers types de nœuds dont certains à large mémoire et d'autres avec accélérateurs comme des GPU.
Pour vous y connecter, utilisez SSH. Un répertoire personnel (/home) est automatiquement créé quand vous vous connectez pour la première fois.

Pour désigner un superordinateur, nous privilégions le terme grappe qui représente mieux l'architecture de nos systèmes; plusieurs ordinateurs distincts (nœuds) forment une unité semblable à une grappe.

* Trillium est une grappe homogène conçue pour les tâches massivement parallèles (plus de 1000 cœurs).

Votre mot de passe pour vous connecter aux nouvelles grappes est celui que vous utilisez pour  vous connecter à CCDB. Votre nom d'utilisateur est affiché au haut de votre page d'accueil CCDB.

=Quels sont les systèmes qui répondent à mes besoins?=
Répondre à cette question n'est pas facile puisqu'ils peuvent subvenir à un large éventail de besoins. Si vous avez besoin de clarifications, n'hésitez pas à communiquer avec le soutien technique.

Les questions suivantes nous aideront à identifier les ressources pertinentes :
* Quels sont les logiciels que vous voulez utiliser?
** Les logiciels doivent-ils être sous licence commerciale?
** Les logiciels peuvent-ils opérer sans l'intervention d'un utilisateur? Peuvent-ils être contrôlés par un fichier de commandes ou faut-il passer par l'interface utilisateur?
** Les logiciels peuvent-ils fonctionner sous Linux?
* Pour une tâche type, quels sont les besoins en termes de mémoire, temps, puissance de traitement, accélérateurs, espace de stockage, bande passante sur le réseau, etc.? (fournir une estimation)
* À quelle fréquence ce type de tâche sera-t-il exécuté?

Si vous ne connaissez pas les réponses à ces questions, notre équipe technique peut vous guider et vous indiquer les ressources appropriées.

=Quelles sont les activités de formation?=

La plupart des ateliers sont organisés par nos partenaires régionaux; ils sont offerts en ligne ou en personne et pour tous les niveaux d'expertise.

* WestDRI (Colombie-Britannique et provinces des Prairies)
** site web Training Materials, cliquez sur l'image pour Upcoming sessions ou explorez le menu de navigation dans le haut de la page
** UAlberta ARC Bootcamp
* SHARCNET (Ontario)
** Calendar
** YouTube
** Online Workshops
* SciNet (Ontario)
** Education Site
** YouTube
* Calcul Québec (Québec)
** Événements
** Formation
* ACENET (provinces de l'Atlantique)
** Training
** YouTube
Voir aussi la liste des événements de formation sur Explora.