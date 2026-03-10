---
title: "CFOUR/fr"
url: "https://docs.alliancecan.ca/wiki/CFOUR/fr"
category: "General"
last_modified: "2024-04-03T18:00:49Z"
page_id: 9144
display_title: "CFOUR"
language: "fr"
---

= Introduction =

CFOUR (pour coupled-cluster techniques for computational chemistry) est un paquet logiciel qui permet d’effectuer des calculs de chimie quantique de haut niveau sur les atomes et les molécules. Le principal intérêt de CFOUR réside en la quantité de méthodes ab-initio qu’il offre pour le calcul des propriétés atomiques et moléculaires. La plupart des approches basées sur la théorie des perturbations Møller-Plesset (MP) et sur l’approximation de clusters couplés (CC) sont disponibles et plusieurs de ces approches permettent la dérivation analytique.

CFOUR n’est pas un programme commercial et son développement se fait par l’apport constant d’améliorations et de nouvelles techniques. Voir le site web .

= Limites de la licence  =

L'Alliance a conclu un accord de licence avec le professeur Jürgen Gauss qui représente les développeurs de CFOUR.

Pour utiliser la version disponible sur nos grappes, vous devez faire parvenir les énoncés suivants au  soutien technique.

# J’utiliserai CFOUR pour la recherche académique uniquement.
# Je ne copierai pas le logiciel CFOUR, ni le rendrai disponible à une autre personne.
# Je citerai correctement l'Alliance et les articles de CFOUR dans mes publications (voir la licence pour les détails).
# J’accepte que l’entente d’utilisation de CFOUR puisse être en tout temps annulée par les développeurs de CFOUR ou par l'Alliance.
# J’informerai l'Alliance de toute dérogation aux énoncés précédents.

À la réception de cette déclaration, nous vous donnerons accès à l’application.

= Module =

Pour accéder la version MPI de CFOUR, chargez le module ainsi

module load intel/2023.2.1  openmpi/4.1.5 cfour-mpi/2.1

Pour la version séquentielle, le module est chargé ainsi

module load intel/2023.2.1 cfour/2.1

Les utilisateurs peuvent échanger de l’information dans un forum CFOUR. Voyez les renseignements pour vous inscrire à la [liste d’envoi].

== Exemples de scripts ==

Vous devez avoir au moins le fichier ZMAT contenant toute l'information sur la géométrie, la méthode à employer, les ensembles de données de base, etc. Le deuxième fichier est GENBAS; il contient l'information sur les ensembles de données de base qui sont disponibles. Si GENBAS ne se trouve pas dans le répertoire à partir duquel la tâche est lancée, CFOUR crée un symlink et utilise le fichier existant qui se trouve dans le module. Ce fichier se trouve dans $EBROOTCFOUR/basis/GENBAS.

= Références =

* Manual
* Features