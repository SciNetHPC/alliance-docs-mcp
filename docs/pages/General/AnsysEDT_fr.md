---
title: "AnsysEDT/fr"
url: "https://docs.alliancecan.ca/wiki/AnsysEDT/fr"
category: "General"
last_modified: "2026-02-17T08:08:52Z"
page_id: 28886
display_title: "AnsysEDT"
language: "fr"
---

AnsysEDT regroupe des solutions de simulation électromagnétique telles qu'Ansys HFSS, Ansys Maxwell, Ansys Q3D Extractor, Ansys SIwave et Ansys Icepak, utilisant des flux de travail de CAO électriques (ECAD) et mécaniques (MCAD). AnsysEDT s'intègre également à l'ensemble de la gamme Ansys de solveurs thermiques, fluides et mécaniques, permettant une analyse multiphysique complète.

= Licence=

AnsysEDT est hébergé sur nos grappes, mais nous n'avons pas une licence qui permet un accès généralisé. Toutefois, plusieurs établissements, facultés et départements possèdent des serveurs de licences qui peuvent être utilisés dépendant de l'aspect légal de leur utilisation. En ce qui a trait à l'aspect technique, nos nœuds de calcul doivent pouvoir communiquer avec votre serveur de licence.  Quand tout sera en place, vous pourrez charger le module ansysEDT qui localisera de lui-même la licence. En cas de difficulté, communiquez avec le soutien technique.

== Configurer votre propre fichier de licence ==
 Pour indiquer votre licence ansysedt, créez un fichier nommé $HOME/.licenses/ansys.lic qui contient deux lignes. Pour les détails, voir Configurez votre propre fichier de licence.

= Soumettre des tâches en lots =

Ansys EDT peut être exécuté de manière interactive en mode batch (sans interface graphique) en démarrant d'abord une session salloc avec les options salloc --time=3:00:00 --tasks=8 --mem=16G --account=def- compte; copiez-collez ensuite la commande ansysedt complète donnée à la dernière ligne de script-local-cmd.sh en vous assurant de spécifier manuellement $YOUR_AEDT_FILE.

=== Scripts pour l'ordonnanceur Slurm ===

Les tâches peuvent être soumises à la file d'attente d'une grappe avec la commande sbatch script-name.sh en utilisant l'un des scripts Slurm pour nœud simple ci-dessous. En date de janvier 2023, ces scripts n'ont été testés que sur Graham et pourraient donc être mis à jour à l'avenir si nécessaire pour fonctionner avec d'autres grappes. Avant de les utiliser, spécifiez le temps de simulation, la mémoire, le nombre de cœurs et remplacez YOUR_AEDT_FILE par le nom de votre fichier d'entrée. Une liste complète des options de ligne de commande peut être obtenue en démarrant AnsysEDT en mode graphique avec les commandes ansysedt -help ou ansysedt -Batchoptionhelp pour obtenir des fenêtres graphiques contextuelles déroulantes.

= Mode graphique =

Les programmes Ansys fonctionnent interactivement en mode graphique sur les nœuds de calcul des grappes ou sur les nœuds VDI de Graham.

== Nœuds VDI ==

1. Connect to an OnDemand system using one of the following URLs in your laptop browser :
 NIBI: https://ondemand.sharcnet.ca
 FIR: https://jupyterhub.fir.alliancecan.ca
 NARVAL:  https://portail.narval.calculquebec.ca/
 RORQUAL: https://jupyterhub.rorqual.alliancecan.ca
 TRILLIUM: https://ondemand.scinet.utoronto.ca
2. Open a new terminal window in your desktop and run:
::: module load StdEnv/2023  (default)
::: module load ansysedt/2024R2.1 **OR** ansysedt/2023R2
::: Type ansysedt in the terminal and wait for the gui to start
3. Verify the following settings
::: The following only needs to be done once:
:::: click Tools -> Options -> HPC and Analysis Options -> Edit
:::: When the Analysis Configuration panel appears untic Use Automatic Settings
:::: Ensure the settings in the Machine tab correspond requested desktop resources such as:
::::  | Tasks 1 | Cores 4 | Allocated_Cores | GPUs 0 | RAM 90 | tic Enabled |
:::: Click the OK button to save any changes and close the Analysis Configuration panel
:::: Click the OK button close the HPC and Analysis Options panel
4.  To retrieve the 2024R2.1 Antennas examples, copy its directory under your account as follows:
:::: module load ansysedt/2024R2.1
:::: mkdir -p ~/Ansoft/$EBVERSIONANSYSEDT; rm -rf ~/Ansoft/$EBVERSIONANSYSEDT/Antennas
:::: cp -a $EBROOTANSYSEDT/v242/Linux64/Examples/HFSS/Antennas ~/Ansoft/$EBVERSIONANSYSEDT
5. Now to run the example:
:::: Open one of the Antennas examples .aedt files then click HFSS -> Validation Check
:::: Click simulation -> setup -> advanced -> Mesh/Solution options -> Use Defaults
:::: Start simulation running by clicking Simulation -> Analyze All
:::: To quit without saving the converged solution click File -> Close -> No
6. If ansysedt crashes and won't restart try running the following commands:
:::: pkill -9 -u $USER -f "ansys*|mono|mwrpcss|apip-standalone-service"
:::: rm -rf ~/.mw (ansysedt will re-run first-time configuration on startup)

= Particularités selon le site =

== Licence SHARCNET ==

Les conditions d'utilisation de la licence ANSYS de SHARCNET (qui inclut AnsysEDT) se trouvent sur la page wiki pour Ansys, ainsi que les autres informations; elles ne sont pas répétées ici.

==== Fichier de licence ====

La licence Ansys de SHARCNET peut être utilisée sans frais avec les modules AnsysEDT pour les traaux de recherche sur nos grappes. Pour ce faire, configurez votre fichier ansys.lic comme suit :

[username@cluster:~] cat ~/.licenses/ansys.lic
setenv("ANSYSLMD_LICENSE_FILE", "1055@license3.sharcnet.ca")
setenv("ANSYSLI_SERVERS", "2325@license3.sharcnet.ca")