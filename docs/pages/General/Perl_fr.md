---
title: "Perl/fr"
url: "https://docs.alliancecan.ca/wiki/Perl/fr"
category: "General"
last_modified: "2025-09-11T12:38:02Z"
page_id: 4227
display_title: "Perl"
language: "fr"
---

== Description ==
Perl est un langage de programmation libre interprété qui possède plusieurs paquets développés au fil de plus de 25 années d'existence. Selon cet article, ses forces sont la manipulation des chaînes de caractères, l'accès aux bases de données ainsi que sa portabilité. Ses faiblesses sont sa faible performance et la facilité avec laquelle on peut écrire du code illisible. En effet, par design, Perl offre plusieurs façons de réaliser la même tâche. Plusieurs programmeurs ont adopté ce langage et produisent du code très compact, mais souvent quasi illisible.

== Charger l'interpréteur ==
Perl est installé par défaut sur les serveurs de Calcul Canada. Voyez les versions disponibles avec

et chargez une version comme ceci

== Installer les paquets==
Plusieurs paquets Perl peuvent être installés via le site Comprehensive Perl Archive Network avec l'outil cpan.
Assurez-vous d'abord que l'initialisation est correcte afin de pouvoir installer les paquets dans votre répertoire personnel (home).

===Configuration initiale pour installer le module ===
Lors de la première exécution de la commande cpan, vous devez décider si la configuration doit se faire de façon automatique. Répondez yes.

L'utilitaire cpan demandera si vous voulez ajouter certaines variables d'environnement au fichier .bashrc; acceptez l'ajout. Entrez ensuite la commande quit via l'interface pour quitter cpan. Avant d'installer un module Perl, redémarrez l'interpréteur pour activer les nouveaux paramètres.

=== Installation de paquets ===
Lorsque la configuration initiale est terminée, vous pouvez installer n'importe lequel des 25 000 paquets et plus offerts par CPAN, par exemple :