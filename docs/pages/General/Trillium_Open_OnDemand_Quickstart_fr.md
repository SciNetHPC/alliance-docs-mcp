---
title: "Trillium Open OnDemand Quickstart/fr"
url: "https://docs.alliancecan.ca/wiki/Trillium_Open_OnDemand_Quickstart/fr"
category: "General"
last_modified: "2026-03-05T22:25:05Z"
page_id: 32173
display_title: "Trillium : Guide de démarrage Open OnDemand"
language: "fr"
---

Cette page décrit le service Open OnDemand sur Trillium.  Pour l'information générale sur les instances Open OnDemand sur nos grappes, voir  Open OnDemand.

= Porter un environnement virtuel sur Open OnDemand =

IMPORTANT : En raison du changement de système d’exploitation et de pile logicielle, vos noyaux d’environnement virtuel Python existants risquent de ne pas fonctionner immédiatement sur le site OnDemand. Vous devriez pouvoir activer vos environnements Python dans un terminal Trillium (voir ci-dessous Accès via terminal), avec tous les modules Trillium requis chargés, puis exécuter la commande venv2jup pour les rendre fonctionnels.

=Introduction=

Ce guide décrit les étapes de base pour démarrer avec le portail Open OnDemand de SciNet.

Open OnDemand (OOD) est une plateforme web qui donne accès à un large éventail d'applications scientifiques et de ressources informatiques, telles que Jupyter Lab, R Studio et Visual Studio Code. Elle vous permet d'interagir avec Trillium via un navigateur web, sans avoir à installer de logiciel sur votre ordinateur. Vous pourrez gérer des fichiers, soumettre et suivre des tâches, et exécuter des applications de manière interactive. Pour plus d'information, consultez le site web .

=Se connecter au portail Open OnDemand=

Pour accéder au portail Open OnDemand, ouvrez un navigateur web et allez à la page https://ondemand.scinet.utoronto.ca. Saisissez votre nom d'utilisateur et votre mot de passe avec l'Alliance, puis effectuez l'authentification multifacteur via Duo ou YubiKey. La connexion étant établie, le tableau de bord Open OnDemand sera affiché. Vous pourrez alors accéder aux différents outils et applications disponibles.

=Gestion des fichers=

La plateforme Open OnDemand propose un explorateur permettant de gérer vos fichiers et répertoires dans le système de fichiers. Pour y accéder, cliquez sur l'onglet Files et sélectionnez le répertoire à gérer dans le menu déroulant (HOME, SCRATCH ou PROJECT). L'explorateur de fichiers sera affiché et vous pourrez :

* Naviguer dans vos répertoires
* Téléverser et télécharger des fichiers
* Créer des fichiers et répertoires
* Supprimer des fichiers et répertoires
* Modifier des fichiers existants

Storage quotas can also be displayed by clicking on the Storage Quotas link in the Files tab.

==Téléverser des fichiers==

La taille maximale des fichiers à téléverser est présentement de 10Go. Pour téléverser un fichier plus volumineux ou si vous rencontrez des problèmes de téléversement (par exemple, en raison d'une mauvaise connexion Internet), utilisez Globus Globus. Un bouton Globus est disponible en haut à droite de l'explorateur de fichiers . Cliquez sur ce bouton pour faire afficher l'interface web de Globus où vous pourrez vous connecter avec votre nom d'utilisateur et votre mot de passe avec l'Alliance. Le chemin d'accès affiché dans le navigateur Open OnDemand sera identique à celui ouvert dans Globus.

=Soumettre une tâche=

Open OnDemand offre également une interface permettant de soumettre des tâches en lots à Trillium. Ceci peut s'avérer utile lorsque vous avez besoin de plus de ressources que celles fournies par les tâches interactives, par exemple un accès exclusif à 192 cœurs et 755Go de mémoire sur un nœud de calcul Trillium.

The Open Composer app provides a suite of Slurm job template scripts that can be submitted directly to the Trillium scheduler. It also provides an interface to monitor your submitted jobs, via the History tab. You can access Open Composer by navigating to the Jobs drop-down menu and selecting Open Composer or by clicking on one of the Slurm job templates e.g. MPI Slurm Job, OpenMP Slurm Job and Hybrid MPI/OpenMP Slurm Job.

Once you have selected a job template, you will be taken to the job submission page. This is split between the job parameters on the left and the job script itself on the right. The job parameters let you control how many resources your job will use, such as the number of nodes, number of tasks per node, wall clock time and output file name. The job script section displays the script that will be submitted to the scheduler. Any changes made to the job parameters will be reflected in the job script automatically. You may also edit the job script directly if you wish.

The extra fields at the top of the page allow you to change how your job is submitted:

* Script Location: specifies the directory where the job script will be saved and where your job will be run from.
* Script Name: specifies the name of the job script file.
* Job Name: specifies the name of the job that will appear in the job queue.
* Cluster: allows you to change which cluster to submit your job to, e.g. Trillium (default) or Trillium-GPU. Selecting Trillium-GPU will provide an additional job parameter to request GPU resources .

Once you are happy with your job script, click on the Submit button to submit the job to the scheduler and save your script to the Script Location. If your job was submitted successfully, you will see a confirmation message at the top of the page with your job ID: .

Note: The template scripts provided in Open Composer are basic examples to get you started. You will need to modify the job script further to suit your specific needs, such as loading your required modules and specifying input/output files.

==Suivi des tâches dans Open Composer==

To monitor your submitted jobs in Open Composer, navigate to the History tab. This will display a list of all your submitted jobs, along with their status: Queued, Running, Completed, Failed. You can filter the jobs by using the Filter text box at the top right or by using the checkboxes below. Clicking on different column fields will give different information about the job:

* Job ID: opens the job in my.SciNet, which displays performance statistics and more detailed Slurm information about the job. Note: my.SciNet may show 'Not found or not permitted' if the job hasn't started yet or was cancelled.
* Application: opens the job script editor of the template you used.
* Script Location: opens an OOD file browser window at the location of the job script. Clicking on the small terminal icon will open a terminal in the job script location.
* Script Name: displays the job script that was submitted to the scheduler.

To resubmit or modify a previously run job click on the job script under the Script Name column and click Load Parameters. This will take you back to the job submission page where further modifications can be made to the job.

==Applications prises en charge==

Présentement, Open Composer prend en charge les applications suivantes :

* MPI Slurm Job
* OpenMP Slurm Job
* Hybrid MPI/OpenMP Slurm Job
* Python Slurm Job
* R Slurm Job
* VASP Slurm Job

=Suivi des tâches=

To get an overview of all your jobs in the queue you can use the job monitoring interface. Navigate to the Jobs tab and select Active Jobs. You can filter the jobs by using the Filter text box at the top right. Columns can also be sorted by clicking on the column headers, for example you can sort by job status (running, completed, failed, etc.). Clicking on > to the left of a job will show you more details about the job, such as the start/end time, node list and account charged etc. You might also want to show all jobs in the queue, you can do this by clicking on the drop-down menu at the top right and selecting All Jobs. A more detailed view of your jobs can still be found using the myscinet portal.

== Applications interactives ==

Open OnDemand also features interactive applications that can be run directly from your web browser. To access the applications, navigate to the Interactive Apps tab and select the application you want to run from the drop-down. This will then bring you to the job submission page where you can choose job parameters such as:

* Length of job in hours
* Number of cores
* Amount of memory to allocate (GB)
* GPU resources (Note: only the h100_1.10 MIG profile is currently available, which provides 10GB of memory and 1/8 of the compute resources of a full NVIDIA H100 GPU.)
* Notify me by email when the job starts

When you have chosen your job parameters click on the Launch button to submit your job to the queue. You will be taken to the My Interactive Sessions page where you can see the status of your job, i.e. queued, running or completed. Once the job has been assigned a node and is running, you can click on the Connect to ... button to launch the application. The application will open in a new tab in your browser, and you can interact with it as if it was running locally.

If you would like terminal access to the node where the application is running, to monitor the performance for example you can click on the button beside Host starting with >_. This will open a terminal window in your browser where you can run commands on the node directly.

If for whatever reason you would like to kill the job, you can do so by clicking on the red Delete button in the job panel in the My Interactive Sessions page.

    Figure 7: Interactive app submission form.

    Figure 8: Interactive sessions page.

    Figure 9: Interactive app session.

==Applications installées==

Les appplications suivantes sont prises en charge :

* Jupyter Lab/Notebook
* Rstudio
* VSCode
* Trillium Desktop
* ParaView
* Forge DDT/MAP
* MATLAB
* SAS4
* Stata4
* Open Composer

Pour faire installer d'autres applications, écrivez à support@scinet.utoronto.ca.

=Interfaces graphiques=

If you would like to run software that has a graphical user interface (GUI) and is not yet installed as an interactive application, such as Octave or Blender, you can do so using the Trillium Desktop application. This app provides a remote desktop environment that you can access through your web browser. In the following example, we will run Octave's GUI:

# Navigate to the Interactive Apps tab and select Trillium Desktop from the drop-down.
# You will be taken to the job submission page. Choose how many cores and amount of memory you would like to allocate for your session in addition to your job length in hours. Then click on the Launch button to submit your job to the queue.
# This will take you to the My Interactive Sessions page. Once your job is running, you have the option to improve the Image Quality and Image Compression of the desktop session. Depending on the speed of your internet connection, you may want to set these lower to improve performance and responsiveness of the desktop. Click on the Connect to Trillium Desktop button to launch the remote desktop environment in a new tab.
# Once the desktop environment has loaded, open a terminal window using the desktop shortcut and load the required modules for Octave:
#: $ module load StdEnv/2023 gcc/12.3 openmpi/4.1.5 octave/7.2.0
# Now launch Octave's GUI by typing octave --gui in the terminal window.

You should now see Octave's GUI appear in the remote desktop environment. You can use this method to run other GUI applications as well, just make sure to load the appropriate modules before launching the application. Applications may have different ways to launch their GUI, so please refer to the application's documentation for more information. You can see the list of binaries installed for a given application by looking at its environment variable, e.g. run ls $EBROOTOCTAVE/bin to see the list of Octave binaries.

=Accès via terminal=

Sometimes you might prefer to use a terminal to interact with Trillium, Open OnDemand provides a web-based terminal that you can use to access the command-line interface. To access the terminal, navigate to the Clusters tab and select Trillium Shell Access. This will open a new tab in your browser with a terminal window where you can run commands as you would in a regular terminal session.

= Debogage =

If you encounter any errors while using an interactive Open OnDemand job, you can check the logs for more information. To access the logs, navigate to the My Interactive Sessions tab and find your active session. Click on the output.log link (see Figure. 12) to open a separate tab which displays the output of your job. This file contains the standard output and error messages generated by the job, which can help you identify any issues that may have occurred during the session. When submitting a ticket to SciNet support, please include the output.log file, your Session ID, which is displayed as a long string of characters, e.g.  8feb45fa-bc65-4846-8398-2a73c1bf8e5a, and any other relevant information to help us assist you more effectively.

= Comparaison avec Jupyter Hub=

feature                 	Jupyter Hub (decommissioned)                                 	Open OnDemand
authentication          	password                                                     	password + MFA
first installed         	2017                                                         	2025
last update             	2021                                                         	2025
supports                	Jupyter Notebook, JupyterLab (R, Python, Julia)              	Jupyter Notebook, JupyterLab (R, Python), Rstudio, VSCode, Desktop, SAS4, Stata4, ParaView, Forge DDT/MAP, MATLAB
start and continue later	Yes                                                          	Yes1
command terminal        	No                                                           	Yes
file management         	Yes (limited)                                                	Yes
monitor jobs            	No                                                           	Yes
submit jobs             	No                                                           	Yes
core limit              	8 cores2                                                     	20 cores (8 for high memory)3
memory limit            	48 GB2                                                       	85 GB (500 GB for high memory)3
time limits             	3 days2                                                      	3 days3
operating system        	CentOS 7                                                     	RockyLinux 9
software stack          	NiaEnv, CCEnv                                                	CCEnv
system issue alerts     	No                                                           	Yes
user quota alerts       	No                                                           	Yes
error logs              	No                                                           	Yes
hardware                	1 x CPU with 40 Intel "CascadeLake" cores at 2.5 GHz, 1TB RAM	62 x CPU with 40 Intel "CascadeLake" cores at 2.5 GHz, 180GB RAM (default)3 x CPU with 40 Intel "CascadeLake" cores at 2.5 GHz, 1TB RAM (high memory) 4 x NVIDIA H100 80GB GPUs, with 96-core AMD EPYC 9654 CPU at 2.4 GHz, 810GB RAM

1 À l'intérieur des limites demandées.

2 Parce que les limites pour JupyterHub n'ont pas été implémentées très scrictement, elles peuvent temporairement être surpassées.

3 Demander les limites avant de lancer une application.

4 Vous devez détenir une licence.