---
title: "BLAST/fr"
url: "https://docs.alliancecan.ca/wiki/BLAST/fr"
category: "General"
last_modified: "2024-01-30T20:51:44Z"
page_id: 9876
display_title: "BLAST"
language: "fr"
---

BLAST (pour Basic Local Alignment Search Tool) permet de trouver les régions similaires entre deux ou plusieurs séquences de nucléotides ou d'acides aminés, et de réaliser un alignement de ces régions homologues.

== Manuel de l'utilisateur ==
Vous trouverez plus d'information sur les arguments dans le  manuel de l'utilisateur ou en lançant la commande.

== Bases de données ==
Certaines bases de données de séquences fréquemment utilisées se trouvent sur nos grappes dans /cvmfs/bio.data.computecanada.ca/content/databases/Core/blast_dbs/2022_03_23/. Voyez le contenu de ce répertoire et de ses sous-répertoires avec, par exemple,

== Accélérer la recherche ==
Dans les exemples qui suivent, le fichier ref.fa est utilisé comme base de référence au format FASTA et le fichier seq.fa pour les requêtes à faire.

=== makeblastdb ===
Avant d'exécuter une recherche, il faut préparer la base de données. Ceci peut se faire par une tâche de prétraitement, avec les autres tâches dépendantes du résultat de la tâche makeblastdb.
Voici un exemple d'un script de soumission :

=== Vecteur de tâches ===
Le parallélisme des données peut grandement améliorer la recherche; il s'agit de diviser le fichier de requêtes en plusieurs requêtes qui se feront à la base de données.

==== Prétraitement ====
Pour accélérer la recherche, le fichier seq.fa doit être divisé en plusieurs petites parts. Ces parts devraient être d'au moins 1Mo; des parts plus petites pourraient nuire au système de fichiers parallèles.

Avec l'utilitaire faSplit, la commande

crée 10 fichiers nommés seqN.fa où N représente [0..9] pour 10 requêtes (séquences).

==== Soumettre une tâche ====
Une fois que les requêtes sont séparées vous pouvez créer une tâche pour chaque fichier seq.fa.N avec un vecteur de tâches. L'identifiant de la tâche contenu dans le vecteur correspondra au nom du fichier où se trouvent les requêtes à exécuter.

Avec cette solution, l'ordonnanceur peut utiliser les ressources de la grappe qui sont disponibles pour exécuter les plus petites tâches.

Avec le script ci-dessus, vous pouvez soumettre votre requête BLAST et elle sera exécutée après que la base de données aura été créée.
afterok:$(sbatch makeblastdb.sh) blastn_array.sh}}

Quand toutes les tâches du vecteur sont terminées, concaténez les résultats avec

où les 10 fichiers sont concaténés dans seq.ref.
Ceci peut s'effectuer à partir du nœud de connexion ou comme tâche indépendante une fois que toutes les tâches du vecteur sont complétées.

=== GNU Parallel ===
GNU Parallel est un bon outil pour grouper plusieurs petites tâches en une et la paralléliser. Cette solution réduit les problèmes qui se produisent avec plusieurs petits fichiers dans un système de fichiers parallèles avec des requêtes sur des blocs de taille fixe dans seq.fa avec un cœur et plusieurs nœuds.

Par exemple, pour le fichier seq.fa de 100Mo, vous pourriez lire des blocs de 10Mo et GNU Parallel créerait 3 tâches, utilisant ainsi 3 cœurs; en demandant 10 cœurs, ce sont 7 cœurs qui auraient été gaspillés. La taille des blocs est donc importante. On peut aussi laisser GNU Parallel décider, comme dans l'exemple ci-dessous.

Voir aussi Travailler avec des fichiers volumineux dans la page sur GNU Parallel.

==== Utiliser plusieurs cœurs dans un nœud====

Note : Le fichier ne doit pas être compressé.

===== Soumettre une tâche =====
Avec le script ci-dessus, vous pouvez soumettre votre requête BLAST et elle sera exécutée après que la base de données aura été créée.
afterok:$(sbatch makeblastdb.sh) blastn_gnu.sh}}

=== Additional tips ===
* Si le stockage local du nœud le permet, copiez votre base de données FASTA dans l'espace local /scratch ($SLURM_TMPDIR).
* Si votre recherche s'y prête, réduisez le nombre de réponses (-max_target_seqs, -max_hsps).
* Si votre recherche s'y prête, limitez la liste des réponses avec des filtres -evalue pour ne conserver que les réponses quasi identiques.