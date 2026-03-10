---
title: "SAIGE/fr"
url: "https://docs.alliancecan.ca/wiki/SAIGE/fr"
category: "General"
last_modified: "2026-01-29T14:35:51Z"
page_id: 23147
display_title: "SAIGE"
language: "fr"
---

SAIGE est un paquet R développé avec Rcpp pour l’étude d'associations pangénomiques avec les grands ensembles de données et les biobanques.

Cette méthode

* tient compte de la parenté des échantillons sur la base des modèles mixtes généralisés;

* permet l'ajustement des modèles selon une matrice de relations génétiques complète ou clairsemée (GRM);

* fonctionne pour les traits quantitatifs et binaires;

* gère le déséquilibre des traits binaires dans les cas témoin;

* produit des calculs efficaces pour les grands ensembles de données;

* effectue des tests d'association à un seul variant;

* fournit une estimation de la taille de l'effet grâce à la régression logistique à biais réduit de Firth;

* effectue l'analyse d'association conditionnelle.

Cette page décrit l'installation du paquet 1.0.0 de SAIGE.

== Installation dans l'environnement StdEnv/2020 ==

1.  Chargez les modules nécessaires.

2. Créez le répertoire d'installation.
~/.local/R/$EBVERSIONR/
}}
3. Installez les dépendances de R. Il est important d'installer exactement ces versions.Si au cours de l'installation on vous demande d'installer la plus récente version d'une dépendance, appuyez sur Enter pour refuser.

[name@server ~]$ R -e 'install.packages("remotes", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("Rcpp", version="1.0.10", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("RcppParallel", version="5.1.6", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("data.table", version="1.17.8", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("RcppArmadillo", version="14.0.2-1", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("SPAtest", version="3.1.2", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("RcppEigen", version="0.3.3.9.3", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("BH", version="1.81.0-1", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("optparse", version="1.7.3", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("SKAT", version="2.2.5", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("MetaSKAT", version="0.82", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("qlcMatrix", version="0.9.5", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("RhpcBLASctl", version="0.23-42", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("RSQLite", version="2.3.8", repos="https://cloud.r-project.org/")'
[name@server ~]$ R -e 'remotes::install_version("dplyr", version="1.1.0", repos="https://cloud.r-project.org/")'

4. Téléchargez la version 1.0.0 de SAIGE.

5. Modifiez l'installation.

Supprimez d'abord le fichier configure pour éviter d'installer des dépendances qui sont déjà disponibles. Ensuite, modifiez le nom de la bibliothèque pour qu'elle utilise le fichier Makevars et que les options utilisent FlexiBLAS. Vous évitez ainsi d'obtenir le message d'erreur unable to find -llapack à l'installation. Pour plus d'information, lisez  BLAS et LAPACK.

6. Compilez et installez.

7. Effectuez un test.