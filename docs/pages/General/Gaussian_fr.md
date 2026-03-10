---
title: "Gaussian/fr"
url: "https://docs.alliancecan.ca/wiki/Gaussian/fr"
category: "General"
last_modified: "2026-02-17T08:04:13Z"
page_id: 3791
display_title: "Gaussian"
language: "fr"
---

Voir aussi la page sur les messages d'erreur de Gaussian.

Gaussian est une application de chimie computationnelle produite par Gaussian, Inc.

==Limites==

Gaussian est présentement disponible uniquement sur Nibi et Fir.

Nos systèmes nationaux ne prennent pas en charge l'exécution en parallèle grappe/réseau (parallélisme Linda), mais uniquement
l'exécution en parallèle avec multiprocesseur à mémoire partagée.
Ainsi, une tâche Gaussian ne peut pas utiliser plus d'un nœud de calcul.

==Licence==

Pour utiliser l'application, vous devez accepter certaines conditions. Copiez les énoncés suivants dans un courriel et faites-le parvenir au  soutien technique .
#Je ne fais pas partie d'un groupe de recherche qui développe une application concurrente.
#Je ne copierai pas Gaussian ni ne rendrai l'application disponible à un tiers.
#Je reconnaîtrai la collaboration de l'Alliance dans toute publication.
#J'informerai l'Alliance de tout changement concernant les précédentes conditions.
Si vous êtes un utilisateur parrainé par un chercheur principal, celui-ci doit aussi nous avoir fait parvenir une copie des mêmes énoncés.

Nous pourrons alors vous donner accès à Gaussian.

==Utiliser Gaussian sur Fir et Nibi==
Le module gaussian est installé sur Nibi et Fir. Pour connaître les versions disponibles. utiliser la commande module spider comme suit :

 [name@server $] module spider gaussian

Pour les commandes qui s'appliquent aux modules, voir Utiliser des modules.

===Soumettre des tâches===
Les grappes nationales utilisent l'ordonnanceur Slurm; pour des renseignements sur la soumission d'une tâche, consultez Exécuter des tâches.

Puisque seule la version avec multiprocesseur à mémoire partagée de Gaussian est prise en charge, vos tâches ne peuvent utiliser qu'un seul nœud et jusqu'au maximum de cœurs par nœud. Cependant, en raison de la scalabilité de Gaussian, nous vous recommandons de ne pas utiliser plus de 32 CPU par tâche si vous ne pouvez pas prouver qu'ils seront bien utilisés.  Nibi et Fir ont 192 CPU par nœud; nous vous demandons de ne pas utiliser des nœuds entiers puisque ce ne serait pas efficace. Si vos tâches nécessitent plus de mémoire que ce que vous pouvez obtenir sur un seul nœud, sachez que chacune des grappes offre quelques nœuds avec plus de mémoire. Pour connaître le nombre de nœuds sur une grappe et leur capacité, consultez  Fir et Nibi.

En plus du fichier d'entrée name.com, vous devez préparer un script décrivant les ressources de calcul pour la tâche; ce script doit être dans le même répertoire que le fichier d'entrée.

Il y a deux options pour les tâches Gaussian, selon la localisation des fichiers d'exécution par défaut et la taille de la tâche :

====Option 1 : G16 (G09, G03)====

Avec cette option, les fichiers d'exécution par défaut (unnamed .rwf, .inp, .d2e, .int, .skr) sont enregistrés dans /scratch/username/jobid/ et demeurent dans ce répertoire si la tâche n'est pas terminée ou si elle a échoué. Le fichier .rwf peut y être récupéré pour redémarrer la tâche plus tard.

Voici un exemple d'un script G16.

Remarquez que pour assurer la cohérence, les fichiers portent le même nom avec des extensions différentes (name.sh, name.com, name.log).

Pour utiliser Gaussian 09 ou Gaussian 03, remplacez gaussian/g16.c01 par gaussian/g09.e01 ou gaussian/g03.d01 et remplacez G16 par G09 ou G03. Modifiez --mem, --time, --cpus-per-task selon vos besoins en ressources de calcul.

====Option 2 : g16 (g09, g03)====

Avec cette option, les fichiers d'exécution par défaut (unnamed .rwf, .inp, .d2e, .int, .skr) sont enregistrés temporairement dans $SLURM_TMPDIR (/localscratch/username.jobid.0/) dans le nœud de calcul où la tâche devait être exécutée. Les tâches seront exécutées plus rapidement si vous utilisez  /localscratch. L'ordonnanceur supprime les fichiers quand la tâche est terminée que ce soit avec ou sans succès. Si vous voulez utiliser le fichier .rwf pour redémarrer la tâche plus tard, vous devez spécifier et nommer votre propre fichier .rwf explicitement dans le fichier d'entrée de Gaussian.

/localscratch est d'environ 3To, partagés par toutes les tâches exécutées sur le même nœud. Si la taille de vos fichiers est semblable ou plus grande, utilisez plutôt l'option G16 (G09, G03).

Voici un exemple d'un script g16.

====Soumettez la tâche====
 sbatch mysub.sh

===Tâches interactives===
Il est possible d'exécuter une tâche Gaussian interactive à des fins de test. Il n'est cependant pas indiqué d'exécuter une tâche Gaussian interactive sur un nœud de connexion. Ouvrez plutôt une session interactive sur un nœud de calcul avec salloc pour une durée d'une heure, avec 8 CPUs et 10Go de mémoire.
1:0:0 --cpus-per-task8 --mem10g}}

Puis, utilisez

ou

=== Redémarrer une tâche ===
Une tâche Gaussian peut être redémarrée à partir du fichier rwf précédent.

Comme d'habitude, l'optimisation géométrique peut être redémarrée à partir du fichier chk.
Avec le fichier rwf, vous pouvez redémarrer les calculs qui se font en une étape, par exemple les calculs de fréquence analytique incluant des propriétés comme ROA et VCD avec ONIOM; les calculs CCSD et EOM-CCSD; NMR; Polar=OptRot; et les énergies CID, CISD, CCD, QCISD et BD.

Pour redémarrer une tâche à partir du fichier rwf, vous devez connaître l'endroit où se situe ce fichier rwf de la tâche précédente.

Il suffit d'indiquer d'abord le chemin %rwf vers le fichier rwf précédent et modifier la ligne des mots-clés pour qu'elle se lise #p restart, puis laisser une ligne vide à la fin.

Voici un exemple :

===Exemples===

Un exemple de fichier d'entrée et de scripts *.sh se trouve dans
/opt/software/gaussian/version/examples/
où la version est g03.d10, g09.e01, g16.a03 ou g16.b01.

== Remarques ==
# NBO7 est inclus uniquement dans la version g16.c01 avec l'emploi des mots-clés nbo6 et nbo7.
# NBO6 est inclus dans les versions g09.e01 et g16.b01.
# Voir les diapositives du webinaire  Running Gaussian16 and NBO7 effectively on Nibi and Fir (2026).

== Erreurs ==
Vous trouverez la solution à plusieurs erreurs dans Gaussian – Messages d’erreur.