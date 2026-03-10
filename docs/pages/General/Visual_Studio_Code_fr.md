---
title: "Visual Studio Code/fr"
url: "https://docs.alliancecan.ca/wiki/Visual_Studio_Code/fr"
category: "General"
last_modified: "2026-03-03T18:46:34Z"
page_id: 24904
display_title: "Visual Studio Code"
language: "fr"
---

TRADUCTION EN COURS
Visual Studio Code est un environnement de développement local intégré (IDE) personnalisable de Microsoft qui offre plusieurs extensions.

* Utilisez VS Code en mode local et évitez de le connecter aux systèmes. Enregistrez vos modifications dans les fichiers de /project avec Git, puis appliquez-les sur les systèmes pour faire les tests.
* Utilisez nano ou vim pour modifier les fichiers directement sur les systèmes.
* Pour le débogage et les tests rapides, vous pouvez charger le module code-server.
* Si aucune de ces solutions n'est possible, vous pouvez configurer VS Code pour les connexions à distance.

__FORCETOC__

=Utiliser VS Code localement=

Les avantages sont les suivants :

* vitesse et stabilité : l'exécution locale de VS Code réduit les interruptions réseau et améliore la performance, ce qui est idéal pour le développement itératif;
* accès direct : vous pouvez interagir avec les fichiers, les extensions et le terminal directement sur votre ordinateur, sans latence;
* possibilité de travailler hors ligne : aucune contrainte de connexion Internet ou de serveur distant, ce qui vous permet de coder n'importe où et n'importe quand.

Il est recommandé de développer localement avec VS Code. Vous pouvez ainsi personnaliser et ajouoter vos extensions et langages préférés avec VS Code.

Une fois votre projet prêt à être testé sur les systèmes, vous pouvez enregistrer vos modifications dans un dépôt Git, les envoyer vers un hébergeur distant comme GitHub ou GitLab, puis vous connecter au système et récupérer vos modifications pour effectuer le test.

Pour plus d'information sur la gestion de versions, voir VS Code Source Control.

Après avoir enregistré et envoyé vos modifications vers le dépôt distant, connectez-vous au système via le terminal.

Clonez le dépôt (s'il n'existe pas)

ou changez le répertoire pour votre dépôt et incorporez les modifications avec

Then test your changes in a short interactive job using minimal resources.

= Editing files on the systems=
While VS Code is great for local development, sometimes you need direct access to files on a remote system. In such cases, terminal-based editors like nano or vim offer a lightweight and efficient way to edit files directly from the command line.

Si vous préférez une interface graphique, l'éditeur de texte JupyterLab est une option commode; les scripts au formats Markdown, Python et autres sont pris en charge.

= Debugging and testing =
If you need to debug or test your code on the systems, you can start a code-server instance from Jupyter Lab.

# Access one of the options to launch JupyterLab.
# Select minimal resources and start an interactive JupyterLab job.
# On the Launcher tab, click on the VS Code launcher button.

The code-server module has several common extensions already available, but we can add more upon request.

== Installation personnalisée des extensions ==
[en préparation ]

= Configuration of VS Code for remote connection =
If none of the above works for your case, one can configure VS Code to connect to a remote host with the Remote SSH extension.

== SSH configuration ==
If not done already, generate your SSH key and add your public SSH key on the CCDB.

Ensuite, créez ou ajoutez un fichier de configuration SSH sur votre ordinateur local.

== Configuration locale ==
1. Dans VS Code, ouvrez la palette Command Palette et appuyez Ctrl+Shift+P (Windows/Linux) ou Cmd+Shift+P (macOS).

2. Dans les paramètres d'utilisation, sélectionnez (Preferences: Open User Settings (JSON)) et collez ou intégrez la configuration suivante :

3. Enregistrez et relancez VS Code.

== Remote configuration ==
1. Log in to the system via an external terminal.

2. Créez le répertoire.

3. Créez la configuration settings.json.

4. Copiez la configuration ci-dessous. Vous pourriez avoir besoin de fusionner manuellement les paramètres avec les vôtres, s'il y a lieu.

== Connecting ==
# Open the Command Palette in VS Code: Press Ctrl+Shift+P (Windows/Linux) or Cmd+Shift+P (macOS).
# Type remote and then select Connect to Host...
# Choose the host (remote system) and confirm.

La connexion se fait avec un nœud de connexion.

== Closing your connection ==
# Open the Command Palette in VS Code: Press Ctrl+Shift+P (Windows/Linux) or Cmd+Shift+P (macOS).
# Type remote and then select Remote-SSH: Kill VS Code Server on Host...
# Choose the host (remote system) and confirm.
# Open the File menu, and select Close Remote Connection.

== Fonction avancée : Se connecter à un nœud de calcul interactif ==

Mettez à jour votre configuration en ajoutant les lignes suivantes :

# In an external terminal, connected to the system via an ssh connection, start a new interactive job (with salloc) with at least 2000M of memory.
## Note the allocated compute node name.
## If you need to work with SLURM_* environment variables in VS Code, save them all in a source file:  grep SLURM_  sed -e 's/^\(.*\)\(.*\)$/export \1"\2"/g' > slurm_var.sh}}
# In VS Code, start a new remote session with the name of the allocated compute node.
## Press F1 or Ctrl+Shift+P to start the command prompt > in the Command Palette.
## Start typing Remote and select Remote-SSH: Connect to Host... > Remote-SSH: Connect to Host...
## Enter the noted compute node name.
### If you get prompted for the type of operating system, select Linux.
# If you need to work with SLURM_* environment variables, navigate to the working directory in a VS Code terminal and source the slurm_var.sh file.

= Remarques importantes =
* VS Code ne peut pas être utilisé sur les nœuds de connexion de tamIA.