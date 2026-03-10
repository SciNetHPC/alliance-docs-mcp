---
title: "Jupyter/fr"
url: "https://docs.alliancecan.ca/wiki/Jupyter/fr"
category: "General"
last_modified: "2022-07-19T12:51:56Z"
page_id: 18600
display_title: "Jupyter"
language: "fr"
---

= Vocabulaire Jupyter et pages wiki reliées =
* Jupyter : une implémentation d'applications Web et d’interfaces interactives de notebooks.
** Google Colab serait une autre implémentation du même type d'environnement.
* Application Jupyter : semblable à toute autre application, mais est affichée dans un onglet Web distinct. L'application peut accéder aux données stockées à distance sur le serveur; les calculs intensifs sont pris en charge par le serveur distant.
* JupyterHub : un serveur Web hébergeant des portails et noyaux (kernels) Jupyter.

== JupyterLab ==
Portail Web avec interface moderne pour la gestion et l'exécution d'applications et la création de fichiers Notebook avec des noyaux (kernels) variés. Pour plus d'information, voyez :
* JupyterLab via JupyterHub, un environnement JupyterLab préinstallé avec un noyau Python par défaut et accès aux modules logiciels;
* JupyterLab à partir d'un environnement virtuel:  environnement personnalisé qui est lancé par une tâche Slurm.

== Jupyter Notebook ==
Portail Web moins récent pour la gestion et l'exécution d'applications et la création de fichiers Notebook avec des noyaux (kernels) variés. Pour plus d'information, voyez :
* Jupyter Notebook via JupyterHub, un environnement Jupyter Notebook préinstallé avec un noyau Python par défaut et accès aux modules logiciels;
* Jupyter Notebook à partir d'un environnement virtuel: un environnement personnalisé qui est lancé par une tâche Slurm.

== Noyau (kernel) ==
Service actif derrière l'interface Web :
* Noyaux Notebook (par exemple Python, R, Julia)
* Noyaux d'application (par exemple RStudio, VSCode)

== Notebook ==
Une page de cellules exécutables contenant du code et du texte formaté :
* Notebook IPython : un notebook exécuté par un noyau (kernel) Python et qui dispose de commandes interactives IPython particulières qui ne sont pas prises en charge par un interpréteur Python régulier.