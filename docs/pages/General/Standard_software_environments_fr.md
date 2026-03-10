---
title: "Standard software environments/fr"
url: "https://docs.alliancecan.ca/wiki/Standard_software_environments/fr"
category: "General"
last_modified: "2025-10-15T15:19:14Z"
page_id: 14699
display_title: "Environnements logiciels standards"
language: "fr"
---

Pour plus d'information sur la migration vers les environnements standards, voyez la page  Migration vers le nouvel environnement standard.

== Description ==
Nos environnements logiciels sont rendus disponibles par un ensemble de  modules qui vous permettent d'alterner entre différentes versions d'un paquet logiciel. Ces modules sont organisés selon une structure en arbre dont le tronc est composé des mêmes utilitaires que ceux offerts dans les environnements Linux. Les branches principales de ce tronc sont les versions des compilateurs auxquelles sont rattachées des sous-branches pour chaque version de MPI ou CUDA.

Un environnement logiciel standard est composé d’une combinaison particulière de modules de compilation et de modules MPI groupés dans un module appelé StdEnv. Ces environnements sont communément utilisés par notre équipe technique pour construire d’autres logiciels.

En date de février 2023, les quatre versions des environnements standards étaient 2023, 2020, 2018.3 et 2016.4, chacune comportant des améliorations importantes. Nous supportons seulement les versions 2023 et 2020.

Nous décrivons ici les différences entre les versions et expliquons pourquoi il est préférable d’installer la plus récente.

Les plus récentes versions des paquets logiciels sont habituellement installées dans le plus récent environnement logiciel.

=== StdEnv/2023 ===
Cette dernière itération de notre environnement logiciel utilise par défaut GCC 12.3.0, Intel 2023.1, et Open MPI 4.1.5.

Pour activer cet environnement, lancez la commande

==== Amélioration de la performance ====
L'ensemble minimal des instructions CPU supporté est AVX2, de façon générale x86-64-v3. Même la couche de compatibilité qui offre les commandes Linux de base est compilé avec des optimisations pour ces commandes.

==== Changements aux modules par défaut ====
Le compilateur par défaut GCC plutôt que Intel. Nous compilons avec Intel uniquement les applications qui démontrent une meilleure performance avec Intel. CUDA devient une extension de OpenMPI plutôt que le contraire, c'est-à-dire que MPI pour CUDA est chargé au lancement si CUDA est chargé. Ceci permet de partager plusieurs bibliothèques MPI sur toutes les branches (CUDA ou non).

Les versions par défaut des modules suivants ont été mises à jour :
* GCC 9.3 => GCC 12.3
* OpenMPI 4.0.3 => OpenMPI 4.1.5
* Compilateurs Intel 2020 => 2023
* Intel MKL 2020 => Flexiblas 3.3.1 (avec MKL 2023 ou BLIS 0.9.0)
* CUDA 11 => CUDA 12

=== StdEnv/2020 ===

Cette troisième version de notre environnement logiciel est devenue la version par défaut en avril 2021. Les compilateurs par défaut sont passés à GCC 9.3.0 et Intel 2020.1. MPI par défaut est passée à Open MPI 4.0.3.

Activez cet environnement avec la commande

====Amélioration de la performance====
Les binaires générés avec le compilateur Intel supportent automatiquement les jeux d’instructions AVX2 et AVX512. Techniquement, ce sont des binaires multiarchitecture, aussi appelés fat binaries. Ceci signifie que quand vous utilisez une grappe comme Cedar ou Graham qui ont connu plusieurs générations de processeurs, vous n’avez plus besoin de charger manuellement un des modules arch si vous utilisez des paquets logiciels générés avec le compilateur Intel.

Certains paquets logiciels installés auparavant avec GCC ou Intel se trouvent maintenant à un niveau plus bas de la hiérarchie, ce qui fait que le même module est visible peu importe le compilateur qui est chargé; c’est le cas par exemple pour les modules R et pour plusieurs paquets en bio-informatique pour lesquels le module gcc devait auparavant être chargé. Ceci a été rendu possible par des optimisations spécifiques aux architectures CPU que nous avons effectuées sous le niveau du compilateur.

Nous avons aussi installé une version plus récente de GNU C Library qui offre des fonctions mathématiques optimisées. Ceci a nécessité une plus récente version du noyau Linux (voir ci-dessous).

==== Couche de compatibilité ====
La couche de compatibilité est un niveau de la hiérarchie en dessous de celui des compilateurs et des paquets logiciels pour que ces derniers soient indépendants du système d’exploitation sous-jacent et qu’ils fonctionnent autant sous CentOS que sous Ubuntu ou Fedora.Un changement majeur dans la version 2020 a été de changer d'outil pour la couche de compatibilité en passant de Nix package manager à Gentoo Prefix.

====Noyau Linux====
Les versions 2016.4 et 2018.3 nécessitent une version du noyau Linux  2.6.32 ou plus, ce qui est supporté à partir de CentOS 6. La version 2020 demande un noyau Linux 3.10 ou plus, ce qui est supporté à partir de CentOS 7. Les autres distributions Linux ont habituellement un noyau beaucoup plus récent et vous n’aurez donc pas à changer votre distribution Linux si vous utilisez cet environnement standard sous une autre que CentOS.

==== Extensions de modules ====
Avec l'environnement 2020, nous avons commencé à installer plusieurs extensions Python dans les modules principaux correspondants. Par exemple,  PyQt5 a été installé dans le module qt/5.12.8 pour supporter plusieurs versions de Python. Le système des modules a été modifié pour vous permettre de trouver facilement ce type d'extensions. Par exemple, avec

vous saurez que vous pouvez obtenir le module qt/5.12.8.

=== StdEnv/2018.3 ===

Cette deuxième version de notre environnement logiciel a été installée en 2018, avec la mise en service de la grappe Béluga, peu après le déploiement de [Niagara/fr|Niagara]. Les compilateurs par défaut sont passés à GCC  7.3.0 et Intel 2018.3. L’implémentation MPI par défaut est passée à Open MPI 3.1.2. Il s’agit de la première version à offrir le support des instructions AVX512.

Activez cet environnement avec la commande

=== StdEnv/2016.4 ===

Cette première version de notre environnement logiciel a été installée en 2016 avec la mise en service des grappes [Cedar/fr|Cedar] et [Graham/fr|Graham]. Les compilateurs par défaut sont GCC 5.4.0 et Intel 2016.4. L’implémentation MPI par défaut est Open MPI 2.1.1. La plupart des logiciels compilés dans cet environnement ne supportent pas les instructions AVX512, contrairement aux processeurs Skylake de Béluga, [Niagara/fr|Niagara] et aux récents ajouts à Cedar et Graham.

Activez cet environnement avec la commande