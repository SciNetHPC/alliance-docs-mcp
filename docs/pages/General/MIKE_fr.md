---
title: "MIKE/fr"
url: "https://docs.alliancecan.ca/wiki/MIKE/fr"
category: "General"
last_modified: "2025-09-03T12:25:02Z"
page_id: 21998
display_title: "MIKE"
language: "fr"
---

Le progiciel MIKE du groupe DHI sert à la modélisation hydraulique et hydrologique.

== Licence ==
MIKE est un produit commercial et vous devez avoir votre propre licence.

Pour pouvoir l'utiliser sur nos grappes de calcul, vous devez contacter le service à la clientèle de MIKE à mike@dhigroup.com et confirmer que vous avez
* une licence internet et
* un lien téléchargé pour la version Linux de MIKE.

== Installation ==

Vous devez télécharger les archives d'installation pour Linux.

Les directives suivantes supposent que les archives d'installation sont dans un fichier Zip (MIKE 2025 et plus récentes) ou dans trois fichiers  *.tgz (MIKE 2024 et moins récentes).

* MIKE_Zero_2025_rhel9.zip

* MIKE_Zero_2024_rhel9_Update_1.tgz
* MIKE_Zero_2024_Tools_rhel9_Update_1.tgz
* MIKE_Zero_2024_Examples_Update_1.tgz

* MIKE_Zero_2023_rhel7_22.11.05.tgz
* MIKE_Zero_2023_Tools_rhel7_22.11.05.tgz
* MIKE_Zero_2023_Examples.tgz

* MIKE_Zero_2022_rhel7_Update_1.tgz
* MIKE_Zero_2022_Tools_rhel7_Update_1.tgz
* MIKE_Zero_2022_Examples_Update_1.tgz

1. Créez le répertoire ~/scratch/MIKE_TGZ pour y téléverser les archives d'installation.

2. MIKE a été compilé avec la bibliothèque Intel MPI, donc chargez le module apparié

 module load StdEnv/2023 intel/2023.2.1 intelmpi/2021.9.0

 module load StdEnv/2020  intel/2021.2.0  intelmpi/2021.2.0

 module load StdEnv/2020  intel/2020.1.217  intelmpi/2019.7.217

3. Lancez les commandes suivantes, selon la version de MIKE.
Ceci fera l'extraction des archives. Exécutez les scripts d'installation  install.sh pour chaque composant,
ensuite modifiez les binaires pour qu'ils trouvent les bibliothèques dynamiques d'Intel MPI.

 export MIKE_TGZ="$HOME/scratch/MIKE_TGZ"
 export MIKE_HOME="$HOME/MIKE/2025"

 cd $MIKE_TGZ
 unzip -j  MIKE_Zero_2025_rhel9.zip
 tar -xzf MIKE_Common_2025_rhel9.tgz
 tar -xzf MIKE_Zero_2025_rhel9.tgz
 tar -xzf MIKE_Zero_2025_Tools_rhel9.tgz
 tar -xzf MIKE_Zero_2025_Examples.tgz

 cd $MIKE_TGZ/MIKE_Common_2025_rhel9
 sed -i 's/ cp -rp / cp -r /' install.sh
 sh install.sh --eula --install-path "$MIKE_HOME" --license-server 127.0.0.1
 cd $MIKE_TGZ/MIKE_Zero_2025_rhel9
 sed -i 's/ cp -rp / cp -r /' install.sh
 sh install.sh --eula --install-path "$MIKE_HOME"
 cd $MIKE_TGZ/MIKE_Zero_2025_Tools_rhel9
 sed -i 's/ cp -rp / cp -r /' install.sh
 sh install.sh --eula --install-path "$MIKE_HOME"
 cd $MIKE_TGZ/MIKE_Zero_2025_Examples
 sed -i 's/ cp -rp / cp -r /' install.sh
 sh install.sh --eula --install-path "$MIKE_HOME"

 module load StdEnv/2023 intel/2023.2.1 intelmpi/2021.9.0
 setrpaths.sh --path "$MIKE_HOME/bin"  --add_origin  \
     --add_path="$EBROOTIMPI/mpi/latest/lib/release:$EBROOTIMPI/mpi/latest/lib"

 export MIKE_TGZ="$HOME/scratch/MIKE_TGZ"
 export MIKE_HOME="$HOME/MIKE/2024"

 cd $MIKE_TGZ
 tar -xzf MIKE_Zero_2024_rhel9_Update_1.tgz
 tar -xzf MIKE_Zero_2024_Tools_rhel9_Update_1.tgz
 tar -xzf MIKE_Zero_2024_Examples_Update_1.tgz

 cd $MIKE_TGZ/MIKE_Zero_2024_rhel9_Update_1
 sed -i 's/ cp -rp / cp -r /' install.sh
 sh install.sh --eula --install-path "$MIKE_HOME" --license-server 127.0.0.1
 cd $MIKE_TGZ/MIKE_Zero_2024_Tools_rhel9_Update_1
 sed -i 's/ cp -rp / cp -r /' install.sh
 sh install.sh --eula --install-path "$MIKE_HOME"
 cd $MIKE_TGZ/MIKE_Zero_2024_Examples_Update_1
 sed -i 's/ cp -rp / cp -r /' install.sh
 sh install.sh --eula --install-path "$MIKE_HOME"

 module load StdEnv/2023 intel/2023.2.1 intelmpi/2021.9.0
 setrpaths.sh --path "$MIKE_HOME/bin"  --add_origin  \
     --add_path="$EBROOTIMPI/mpi/latest/lib/release:$EBROOTIMPI/mpi/latest/lib"

 export MIKE_TGZ="$HOME/scratch/MIKE_TGZ"
 export MIKE_HOME="$HOME/MIKE/2023"

 cd $MIKE_TGZ
 tar -xzf MIKE_Zero_2023_rhel7_22.11.05.tgz
 tar -xzf MIKE_Zero_2023_Tools_rhel7_22.11.05.tgz
 tar -xzf MIKE_Zero_2023_Examples.tgz

 cd $MIKE_TGZ/MIKE_Zero_2023_rhel7_22.11.05
 sh install.sh --eula --install-path "$MIKE_HOME" --license-server 127.0.0.1
 cd $MIKE_TGZ/MIKE_Zero_2023_Tools_rhel7_22.11.05
 sh install.sh --eula --install-path "$MIKE_HOME"
 cd $MIKE_TGZ/MIKE_Zero_2023_Examples
 sh install.sh --eula --install-path "$MIKE_HOME"

 module load StdEnv/2020  intel/2021.2.0  intelmpi/2021.2.0
 setrpaths.sh --path "$MIKE_HOME/bin"  --add_origin  \
     --add_path="$EBROOTIMPI/mpi/latest/lib/release:$EBROOTIMPI/mpi/latest/lib"

 MIKE_TGZ_DIR="$HOME/MIKE_TGZ"
 MIKE_INST_DIR="$HOME/MIKE/2022"

 cd $MIKE_TGZ_DIR
 tar -xzf MIKE_Zero_2022_rhel7_Update_1.tgz
 tar -xzf MIKE_Zero_2022_Tools_rhel7_Update_1.tgz
 tar -xzf MIKE_Zero_2022_Examples_Update_1.tgz

 cd $MIKE_TGZ_DIR/MIKE_Zero_2022_rhel7_Update_1
 sh install.sh --eula --install-path "$MIKE_INST_DIR" --license-server 127.0.0.1
 cd $MIKE_TGZ_DIR/MIKE_Zero_2022_Tools_rhel7_Update_1
 sh install.sh --eula --install-path "$MIKE_INST_DIR"
 cd $MIKE_TGZ_DIR/MIKE_Zero_2022_Examples_Update_1
 sh install.sh --eula --install-path "$MIKE_INST_DIR"

 module load StdEnv/2020 intel/2020.1.217 intelmpi/2019.7.217
 setrpaths.sh --path "$MIKE_INST_DIR/bin"  --add_origin  \
     --add_path="$EBROOTIMPI/intel64/lib/release:$EBROOTIMPI/intel64/lib"

=== Autres versions ===

Les instructions ci-dessus supposent que les archives d'installation ont des noms de fichiers spécifiques. À l'installation de versions mineures publiées la même année, les noms des fichiers pour les archives (par exemple dans tar -xzf MIKE_Zero_2023_rhel7_22.11.05.tgz),
et les noms des répertoires (par exemple dans cd $MIKE_TGZ/MIKE_Zero_2023_rhel7_22.11.05) ont besoin d'être ajustés en conséquence.
Les prochaines versions majeures de MIKE pourraient utiliser des versions différentes de MPI et il faudrait alors modifier les directives en conséquence. Essayez un module de bibliothèque Intel MPI avec une version majeure correspondante (année).

Les directives ci-dessus suivent la procédure d'installation officielle, à l'exception du fait que l'installation de MIKE_Zero_*_Prerequisites.tgz (bibliothèque Intel MPI) est omise et qu'un module correspondant est chargé à sa place. De plus, le script setrpaths.sh est utilisé pour installer des paquets binaires afin de les rendre compatibles avec notre pile logicielle.

Si vous avez des difficultés à adapter la recette pour les versions plus récentes de MIKE, écrivez au soutien technique.

=== Créer un module ===

Collez ces commandes dans votre terminal pour créer un module d'environnement pour MIKE.
Assurez-vous de modifier la version (par exemple 2025) pour correspondre à la version que vous avez installée.
Modifiez aussi la version des modules intelmpi et intel pour correspondre à ce que vous avez chargé pendant l'installation.
Après avoir exécuté les commandes ci-dessous, connectez-vous de nouveau pour que le nouveau module d'environnement soit visible par les commandes de modules; vous pouvez aussi exécuter module use $HOME/modulefiles.

 export MIKE_VERSION=2025
 mkdir -p $HOME/modulefiles/mike
 cat > $HOME/modulefiles/mike/${MIKE_VERSION}.lua <

 export MIKE_VERSION=2024
 mkdir -p $HOME/modulefiles/mike
 cat > $HOME/modulefiles/mike/${MIKE_VERSION}.lua <

 export MIKE_VERSION=2023
 mkdir -p $HOME/modulefiles/mike
 cat > $HOME/modulefiles/mike/${MIKE_VERSION}.lua <

 export MIKE_VERSION=2022
 mkdir -p $HOME/modulefiles/mike
 cat > $HOME/modulefiles/mike/${MIKE_VERSION}.lua <

Activez ce module dans chaque tâche ou dans chaque session de connexion avec

=== Configurer la licence ===

Le service à la clientèle de MIKE vous enverra des directives comme ceci pour configurer votre licence.
internet --iuseruser@example.com --ipasswordmy-password}}
Ceci se fait habituellement une seule fois quand vous recevez une nouvelle licence ou un nouveau code de licence.
Les renseignements sur la licence sont enregistrés dans le fichier ~/.config/DHI/license/NetLmLcwConfig.xml.

== Exemple de script pour une tâche ==