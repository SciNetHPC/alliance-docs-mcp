---
title: "SQLite/fr"
url: "https://docs.alliancecan.ca/wiki/SQLite/fr"
category: "General"
last_modified: "2020-03-12T19:58:17Z"
page_id: 13027
display_title: "SQLite"
language: "fr"
---

SQLite est un moteur de bases de données pour construire des BD dites de poche, car elles offrent toutes les fonctionnalités des BD relationnelles sans l'architecture client-serveur, avec en plus l'avantage que toutes les données résident sur un seul fichier disque qui peut être copié sur un autre ordinateur. Des applications écrites dans plusieurs langages bien connus peuvent lire et écrire dans un fichier SQLite par des requêtes SQL standard via leur API d'interaction avec les BD.

Les bases de données SQLite, comme toutes les autres, ne devraient pas être utilisées dans des systèmes de fichiers partagés comme /home, /scratch et /project. Au début d'une tâche, vous devriez en principe copier le fichier SQLite sur l'espace local /scratch $SLURM_TMPDIR où vous pourrez utiliser la base de données sans problème, tout en bénéficiant de la meilleure performance. Notez que SQLite ne prévoit pas l'emploi de plusieurs fils ou processus qui écrivent dans la base de données en même temps; pour ce faire, vous devriez utiliser une solution client-serveur.

==Utiliser SQLite directement==

Vous pouvez aussi accéder directement à une base de données SQLite en utilisant le client natif.

Si le fichier foo.sqlite n'existe pas, SQLite le crée et ce client démarre dans une base de données vide, autrement, vous êtes connecté à la base de données existante. Vous pouvez alors exécuter toutes les requêtes, par exemple lancer SELECT * FROM tablename; pour faire afficher à l'écran le contenu de tablename.

==Accéder à SQLite à partir d'une application==

Le moyen habituel d'interagir avec une BD SQLite (ou toute autre) est d'utiliser des appels de fonctions pour établir la connexion; exécuter les requêtes de lecture, d'écriture ou de mise à jour des données; et fermer la connexion pour que les modifications soient enregistrées dans le fichier disque SQLite. Dans l'exemple simple montré ci-dessous, nous supposons que la BD existe déjà et qu'elle contient la table employee de deux colonnes : la chaîne name et l'entier age.

#!/usr/bin/env python3

# For Python we can use the module sqlite3, installed in a virtual environment,
# to access an SQLite database
import sqlite3

age = 34

# Connect to the database...
dbase = sqlite3.connect("foo.sqlite")

dbase.execute("INSERT INTO employee(name,age) VALUES(\"John Smith\"," + str(age) + ");")

# Close the database connection
dbase.close()

# Using R, the first step is to install the RSQLite package in your R environment,
# after which you can use code like the following to interact with the SQLite database
library(DBI)

age <- 34

# Connect to the database...
dbase <- dbConnect(RSQLite::SQLite(),"foo.sqlite")

# A parameterized query
query <- paste(c("INSERT INTO employee(name,age) VALUES(\"John Smith\",",toString(age),");"),collapse=)
dbExecute(dbase,query)

# Close the database connection
dbDisconnect(dbase)

#include
#include
#include

int main(int argc,char** argv)
{
  int age = 34;
  std::string query;
  sqlite3* dbase;

  sqlite3_open("foo.sqlite",&dbase);

  query = "INSERT INTO employee(name,age) VALUES(\"John Smith\"," + std::to_string(age) + ");";
  sqlite3_exec(dbase,query.c_str(),nullptr,nullptr,nullptr);

  sqlite3_close(dbase);

  return 0;
}

==Limites==

Comme son nom le suggère, SQLite est facile d'utilisation et conçu pour les bases de données relativement simples dont la taille n’excède pas quelques centaines de Go et dont le modèle entités-associations n'est pas trop complexe. Au fur et à mesure que la taille et la complexité de votre BD augmentent, vous pourriez remarquer une baisse de performance; si c’est le cas, il serait temps de trouver un outil plus sophistiqué sur le modèle client-serveur. Vous trouverez sur le site web SQLite des critères de sélection entre SQLite et les SGBDR client-serveur.