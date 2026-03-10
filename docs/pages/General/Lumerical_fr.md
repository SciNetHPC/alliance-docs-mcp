---
title: "Lumerical/fr"
url: "https://docs.alliancecan.ca/wiki/Lumerical/fr"
category: "General"
last_modified: "2022-12-12T17:53:27Z"
page_id: 6303
display_title: "Lumerical"
language: "fr"
---

Lumerical est une suite logicielle pour la modélisation d'appareils nanophotoniques; elle contient les applications FDTD Solutions.

= Installation =
Les applications FDTD Solutions sont maintenant intégrées au paquet Lumerical. Il n'y a pas d'installation centrale de ces logiciels sur l'infrastructure nationale, mais si vous disposez d'une licence, vous pouvez les installer en suivant les directives ci-dessous.

Si vous avez téléchargé la suite Lumerical au complet (fichier Lumerical-2020a-r1-d316eeda68.tar.gz), voyez les sections Installer Lumerical et Utiliser le module Lumerical.
Si vous avez téléchargé seulement Lumerical (fichier FDTD_Solutions-8.19.1438.tar.gz), voyez les sections Installer FDTD Solutions et Utiliser le module fdtd_solutions.

== Installer Lumerical ==
=== Si la version de l'installateur correspond exactement à celle de la recette  ===
Pour installer la suite Lumerical, lancez
  --disable-enforce-checksums}}
où path est le chemin vers le répertoire qui contient le fichier .tar.gz pour installer Lumerical sous Linux.

=== Si la version de l'installateur ne correspond pas exactement à celle de la recette ===
Avec une version 2020a différente de 2020a-r1-d316eeda68, lancez
 --sourcepath --disable-enforce-checksums}}
Par exemple, si Lumerical-2020a-r1-d316eeda68.eb.tar.gz a été téléchargé dans $HOME/scratch, la commande suivante installera Lumerical dans votre répertoire  $HOME/.local.
2020a-r6-aabbccdd --sourcepath$HOME/scratch --disable-enforce-checksums}}

La version de la recette (année suivie de "a" ou "b") doit être la même que celle de l'installateur.
Si l'année ou la lettre est différente (par exemple recette 2020a et installateur 2020b), nous devrons adapter le script d'installation.

En date du 1er avril 2020, les recettes suivantes sont disponibles :

Recette pour l'installation           	Version de l'installeur                   	Version compatible
Lumerical-2019b-r6-1db3676.eb         	Lumerical-2019b-r6-1db3676.tar.gz         	Lumerical-2019b-*.tar.gz
Lumerical-2020a-r1-d316eeda68.eb      	Lumerical-2020a-r1-d316eeda68.tar.gz      	Lumerical-2020a-*.tar.gz
Lumerical-2021-R2.5-2885-27742aa972.eb	Lumerical-2021-R2.5-2885-27742aa972.tar.gz	Lumerical-2021-*.tar.gz
Lumerical-2022-R1.3-3016-2c0580a.eb   	Lumerical-2022-R1.3-3016-2c0580a.tar.gz   	Lumerical-2022-*.tar.gz

Si ces directives ne fonctionnent pas, contactez le soutien technique et nous adapterons une recette pour l'installation de votre version.

Une fois l'installation terminée, vous devez vous déconnecter et vous reconnecter au serveur. Chargez ensuite le module Lumerical avec

=== Configurer le fichier de licence ===

Le module Lumerical cherchera le fichier $HOME/.licenses/lumerical.lic pour savoir comment contacter le serveur de licence.
Créez le fichier avec le contenu suivant, en ajustant 27011@license01.example.com en fonction du port et du hostname de votre serveur de licence.

Copiez la commande suivante dans $HOME/.licenses/lumerical.lic
 setenv("LUMERICAL_LICENSE_FILE", "27011@license01.example.com")

== Installer FDTD Solutions ==
Pour installer FDTD Solutions, lancez la commande  --disable-enforce-checksums}}
où path est le chemin vers le répertoire qui contient le fichier .tar.gz pour l'installation sous Linux.

Si vous avez une autre version que 8.19.1438, utilisez
 --sourcepath --disable-enforce-checksums}}
Par exemple, si FDTD_Solutions-8.19.1466.tar a été téléchargé dans  $HOME/Downloads, la commande suivante installera FDTD Solutions
à l'intérieur du dossier $HOME/.local.
8.19.1466 --sourcepath$HOME/Downloads --disable-enforce-checksums}}

En cas de difficulté, communiquez avec le soutien technique qui adaptera un script d'installation pour votre version.

Après l'installation, il faut se déconnecter du serveur et se connecter de nouveau. Pour charger le module, utilisez

Vous devez aussi configurer votre installation pour utiliser votre serveur de licence. Démarrez l'application d'abord sur un nœud de connexion et on vous demandera l'information sur votre serveur de licence; il ne sera pas nécessaire de répéter cette opération.

= Utilisation =
Le module lumerical possède plus d'outils que le module fdtd_solutions, mais la principale différence est que la variable d'environnement qui contient l'emplacement pour l'installation se nomme EBROOTLUMERICAL dans un cas et EBROOTFDTD_SOLUTIONS dans l'autre. Les scripts doivent donc être ajustés en fonction du module utilisé en indiquant le nom correspondant du module dans la ligne module load ... et EBROOTFDTD_SOLUTIONS par EBROOTLUMERICAL, selon le cas.

== Utiliser le module Lumerical ==

L'implémentation MPI fournie par Lumerical ne fonctionne pas étroitement avec notre ordonnanceur; pour cette raison, utilisez les options --ntasks-per-node=1 et --cpus-per-task=32 quand vous soumettrez une tâche.

L'exemple suivant est un script qui demande 2 nœuds pour une durée de 30 minutes; vous pouvez adapter ce script selon vos besoins.

== Utiliser le module fdtd_solutions ==
L'implémentation MPI fournie par FDTD ne fonctionne pas étroitement avec notre ordonnanceur; pour cette raison, utilisez les options --ntasks-per-node=1 et --cpus-per-task=32 quand vous soumettez une tâche.

Dans notre exemple de script, on demande deux nœuds pour une heure; vous pouvez utiliser ce script en remplaçant les valeurs de durée et de nombre de nœuds, selon vos besoins.

== Gabarits de scripts ==

Note : Cette section a été rédigée pour le module fdtd_solutions et n'est pas adaptée au module lumerical.

Si vous avez plusieurs simulations à faire exécuter, il ne serait pas très efficace d'avoir à modifier le script pour chacune d'elles. Vous pouvez alors utiliser un gabarit de script.

Par exemple,
* Créez le répertoire $HOME/bin et enregistrez-y le script fdtd-run.sh (voir ci-dessous).
* Créez le répertoire $HOME/bin/templates et enregistrez-y les gabarits de scripts  fdtd-mpi-template.sh et fdtd-process-template.sh.

fdtd-mpi-template.sh est en fait un interpréteur (shell) du script  fdtd_solutions.sh présenté ci-dessus et fdtd-process-template.sh indique les ressources de calcul dont vous avez besoin.

Pour soumettre une tâche, utilisez

Ceci utilisera 32 cœurs sur un seul nœud standard. Pour utiliser plus de cœurs, demandez plusieurs nœuds comme suit

 sed 's/^.*=//'`

#Total memory required
TOTALMEM=$(( ESTMEM * MEMORY_SAFETY / 100 ))

#Memory required per process
PROCMEM=$((TOTALMEM / PROCS))
if [ "$PROCMEM" -lt "$MEMORY_MIN" ]; then
    PROCMEM=$MEMORY_MIN
fi

#Gridpoints
GRIDPTS=`grep gridpoints $1.tmp  sed 's/^.*=//'`

#Timesteps
TIMESTEPS=`grep time_steps $1.tmp  sed 's/^.*=//'`

#Estimated time
TIME=$(( GRIDPTS * TIMESTEPS / PROCS / RATE / 10000000 ))
if [ "$TIME" -lt "$TIME_MIN" ]; then
    TIME=$TIME_MIN
fi

HOUR=$((TIME / 3600))
MINSEC=$((TIME - HOUR * 3600))
MIN=$((MINSEC / 60))
SEC=$((MINSEC - MIN * 60))

echo $TOTALMEM

#The replacements
sed -e "s##$TOTALMEM#g" \
    -e "s##$PROCMEM#g" \
    -e "s##$HOUR#g" \
    -e "s##$MIN#g" \
    -e "s##$SEC#g" \
    -e "s##$PROCS#g" \
    -e "s##$DIRFSP#g" \
    -e "s##$FILENAME#g" \
    $2
}}