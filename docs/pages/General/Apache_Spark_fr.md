---
title: "Apache Spark/fr"
url: "https://docs.alliancecan.ca/wiki/Apache_Spark/fr"
category: "General"
last_modified: "2023-07-21T20:23:34Z"
page_id: 10928
display_title: "Apache Spark"
language: "fr"
---

= Introduction =

Apache Spark est une framework de calcul distribuée open source initialement développé par l'AMPLab de l'Université Berkeley, et maintenant un projet de la fondation Apache. Contrairement à l'algorithme MapReduce implémenté par Hadoop qui utilise le stockage sur disque, Spark utilise des primitives conservées en mémoire lui permettant d'atteindre des performances jusqu'à 100 fois plus rapides pour certaines applications. Le chargement des données en mémoire permet de les interroger fréquemment ce qui fait de Spark une framework particulièrement approprié pour l'apprentissage automatique et l'analyse de données interactive.

= Utilisation =

== PySpark ==

== Java Jars  ==

= Monitoring =

Les journaux d'activités de l'application Spark qui a été exécuté peuvent être sauvegardés et consultés par la suite à l'aide d'une application web fournie avec Spark. Les instructions suivantes montrent comment activer la sauvegarde des journaux et le démarrage de l'application web.

== Configuration ==
Créer d'abord un répertoire qui contiendra les journaux d'application :

S'il n'existe pas déjà, créer ensuite un répertoire qui contiendra les paramètres de configuration de Spark :

Dans ce répertoire, créer le fichier suivant ou ajouter le contenu présenté au fichier spark-defaults.conf si ce dernier existe déjà.

== Visualisation ==

Créer un tunnel entre votre ordinateur et la grappe de calcul.

Charger le module Spark :

Lancer l'application web de visualisation des journaux :
1 start-history-server.sh
|result=
starting org.apache.spark.deploy.history.HistoryServer, logging to /home//.spark//log/spark--org.apache.spark.deploy.history.HistoryServer-1-.computecanada.ca.out
Spark Command: /cvmfs/soft.computecanada.ca/easybuild/software/2017/Core/java/1.8.0_121/bin/java -cp /home//.spark//conf/:/cvmfs/soft.computecanada.ca/easybuild/software/2017/Core/spark/2.2.0/jars/* -Xmx1g org.apache.spark.deploy.history.HistoryServer
========================================
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
17/10/13 04:28:56 INFO HistoryServer: Started daemon with process name: 71616@.computecanada.ca
17/10/13 04:28:56 INFO SignalUtils: Registered signal handler for TERM
17/10/13 04:28:56 INFO SignalUtils: Registered signal handler for HUP
17/10/13 04:28:56 INFO SignalUtils: Registered signal handler for INT
17/10/13 04:28:56 INFO SecurityManager: Changing view acls to:
17/10/13 04:28:56 INFO SecurityManager: Changing modify acls to:
17/10/13 04:28:56 INFO SecurityManager: Changing view acls groups to:
17/10/13 04:28:56 INFO SecurityManager: Changing modify acls groups to:
17/10/13 04:28:56 INFO SecurityManager: SecurityManager: authentication disabled; ui acls disabled; users  with view permissions: Set(); groups with view permissions: Set(); users  with modify permissions: Set(); groups with modify permissions: Set()
17/10/13 04:28:56 INFO FsHistoryProvider: History server ui acls disabled; users with admin permissions: ; groups with admin permissions
17/10/13 04:29:01 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
17/10/13 04:29:02 INFO FsHistoryProvider: Replaying log path: file:/home//.spark//eventlog/app-20171013040359-0000
17/10/13 04:29:02 INFO Utils: Successfully started service on port 18080.
17/10/13 04:29:02 INFO HistoryServer: Bound HistoryServer to 0.0.0.0, and started at http://:18080

}}
Copier l'URL afficher dans le terminal et coller dans votre fureteur web.

Pour stopper l'application de visualisation, entrer la combinaison de touche Ctrl-C dans le terminal ayant servi à lancer l'application.