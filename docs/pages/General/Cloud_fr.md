---
title: "Cloud/fr"
url: "https://docs.alliancecan.ca/wiki/Cloud/fr"
category: "General"
last_modified: "2025-09-18T22:56:34Z"
page_id: 521
display_title: "Service infonuagique"
language: "fr"
---

Nous offrons une infrastructure IaaS pour la création et l'exploitation d'environnements virtuels.

Dans un nuage, une ou plusieurs instances (ou VM pour virtual machine) sont habituellement créées. Avec les privilèges d'administrateur, vous pouvez installer et exécuter tous les programmes nécessaires à votre projet, qu'il s'agisse d'analyser des données en physique des particules ou encore opérer un service Web à l'intention de la recherche en littérature ou en sciences humaines. L'avantage est que vous disposez d'un contrôle total sur les applications installées (la pile logicielle). L'inconvénient par contre est que vous devez posséder une certaine expérience dans la gestion d'un ordinateur et dans l'installation des applications.

Il est facile de créer des instantanés (snapshots) de vos instances pour en faire des copies; ceci permet de disposer de versions avec différentes fonctionnalités ou de relancer la même instance en cas de panne de courant, par exemple.

Si vos tâches s'intègrent bien dans un environnement de traitement par lots géré par un ordonnanceur sur un superordinateur, il serait préférable d'utiliser les autres ressources qui sont plus disponibles et dont les logiciels sont déjà configurés pour les besoins courants. De plus, certains outils dont Apptainer peuvent aisément être utilisés pour exécuter des piles logicielles personnalisées dans des conteneurs sur nos grappes de calcul.

Si Apptainier ou le traitement par lots ne satisfont pas vos exigences, optez pour l'infonuagique.

==Obtenir un projet dans l'environnement infonuagique==
* Assurez-vous de bien comprendre vos responsabilités par rapport à la protection de votre projet et celle d'une infrastructure partagée par plusieurs.
* Si vous ne possédez pas de compte, voyez ces directives.
* Un projet est une allocation de ressources qui vous permet de créer des instances infonuagiques.
* Si vous êtes chercheuse ou chercheur principal et possédez une allocation de ressources infonuagiques (voir la page Concours pour l'allocation de ressources), vous devriez déjà avoir un projet; voyez les renseignements ci-dessous. Si ce n'est pas le cas ou que vous avez des doutes, contactez le soutien technique.
* Autrement, remplissez le formulaire Projets infonuagiques et allocations par le service d'accès rapide pour
** obtenir l'accès à un projet existant; pour connaître les renseignements que vous devrez fournir, voyez ci-dessous,
** dans le cas d'une chercheuse ou d'un chercheur principal,
*** demander la création d'un nouveau projet et une allocation de ressources par le service d'accès rapide,
*** demander une hausse du quota de ressources pour un projet existant.

* Les demandes sont généralement traitées dans les 48 heures ouvrables.

===Préparer votre demande===
* Pour accéder à un projet de calcul ou à un projet persistant, vous devez connaître le nom du projet et le nuage où il se trouve; voyez comment trouver le nom du projet et la liste de nos ressources infonuagiques. La chercheuse ou le chercheur principal doit confirmer son droit d'accéder au projet.
* Si vous demandez la création d'un nouveau projet ou une augmentation du quota des ressources pour un projet existant, vous devez ː
**expliquer pourquoi vous demandez des ressources infonuagiques,
**expliquer pourquoi les grappes de CHP ne conviennent pas à votre projet,
**décrire les méthodes de maintenance et de sécurité qui seront mises en place (voir cette page).
*Une chercheuse ou un chercheur principal peut être propriétaire d'au plus trois projets et la somme des quotas doit respecter les limites établies (voir les limites sur cette page). Elle ou il peut être propriétaire à la fois d'allocations pour des projets de calcul et des projets persistants.

==Créer une machine virtuelle==
* Comment créer manuellement votre première machine virtuelle
* Glossaire technique
* Choix du type de stockage
* Dépannage de problèmes communs

==Vos responsabilités==
Pour chacun de vos projets, vous êtes responsable de
*
 Créer de gérer vos instances
*Assurer la sécurité et la mise à jour des logiciels de vos instances
*Définir les groupes de sécurité pour l'accès à votre réseau
*Créer les comptes des utilisateurs
*Appliquer les meilleures pratiques
*Assurer la sécurité de vos instances
*Faire des copies de sécurité de vos instances

==Sujets avancés==
Si vous avez plus d'expérience, vous pouvez
*Créer automatiquement vos instances
*Coder votre infrastructure avec Terraform
==Cas d'utilisation==
*Configurer un serveur de données ou un serveur Web
*Utiliser les vGPU
*Utiliser les GPU
*Utiliser une interface graphique
*Utiliser IPv6 dans le nuage Arbutus

== Ressources infonuagiques ==

* Nuage Béluga
* Nuage Arbutus; voir la documentation
* Nibi
* Nuage Cedar

L'information sur le matériel et les versions OpenStack se trouve sur la page des ressources infonuagiques.
L'état des ressources et les activités planifiées de maintenance et de mise à jour sont décrits sur la page wiki État des ressources.

== Assistance==
Si vous avez des questions sur ce service, écrivez au soutien technique.