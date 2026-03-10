---
title: "Job arrays/fr"
url: "https://docs.alliancecan.ca/wiki/Job_arrays/fr"
category: "General"
last_modified: "2024-02-13T20:15:55Z"
page_id: 8157
display_title: "Vecteurs de tâches"
language: "fr"
---

Page enfant de Exécuter des tâches

Si vous avez plusieurs tâches dont un paramètre diffère, vous pouvez utiliser un vecteur de tâches (job array, array job, task array). La variable d’environnement $SLURM_ARRAY_TASK_ID différencie chacune des tâches et l’ordonnanceur leur attribue une valeur différente. Les valeurs sont définies par le paramètre --array.

Pour plus d'information, voir documentation de SchedMD.

== Exemples du paramètre --array ==

 sbatch --array=0-7       # $SLURM_ARRAY_TASK_ID utilise les valeurs de 0 à 7 inclusivement
 sbatch --array=1,3,5,7   # $SLURM_ARRAY_TASK_ID utilise les valeurs de la liste
 sbatch --array=1-7:2     # le pas est égal à 2 comme dans l'exemple précédent
 sbatch --array=1-100%10  # limite à 10 le nombre de tâches exécutées simultanément

== Exemple simple ==

Ce script crée 10 tâches indépendantes. Chacune a une durée maximum de 3 heures et chacune peut commencer à des moments différents sur des nœuds différents.

Le script utilise $SLURM_ARRAY_TASK_ID pour indiquer le fichier pour les données en entrée (dans notre exemple program x) ou utiliser en commande de ligne l'argument (avec par exemple program y).

Le fait d’utiliser un vecteur de tâches plutôt que plusieurs tâches séquentielles est avantageux pour vous-même et pour les autres utilisateurs. Un vecteur de tâches en attente ne produit qu’une seule ligne dans squeue, ce qui vous permet de consulter son résultat plus facilement. De plus, l’ordonnanceur n’est pas appelé à analyser les besoins de chacune des tâches séparément, ce qui résulte en un gain de performance.

En excluant le recours à sbatch comme étape initiale, l’ordonnanceur subit la même charge avec un vecteur de tâches qu’avec un nombre équivalent de tâches soumises séparément. Il n’est pas recommandé d’utiliser un vecteur pour soumettre des tâches qui ont une durée de beaucoup moins d’une heure. Les tâches d’une durée de quelques minutes seulement devraient être groupées avec META, GLOST, GNU Parallel ou dans une boucle de l’interpréteur à l'intérieur d'une tâche.

== Exemple avec des répertoires multiples ==

Supposons que vous voulez exécuter le même script dans des répertoires multiples ayant une structure identique;
*si les noms des répertoires peuvent être des nombres séquentiels, il serait facile d’adapter l’exemple présenté plus haut;
*autrement, créez un fichier avec les noms des répertoires comme suit

 $ cat case_list
 pacific2016
 pacific2017
 atlantic2016
 atlantic2017

Il y a plusieurs manières de sélectionner une ligne en particulier dans un fichier; dans le prochain exemple, nous utilisons sed.

ATTENTION
*Le nombre de tâches que vous demandez doit être égal au nombre de lignes du fichier.
*Le fichier case_list ne doit pas être modifié tant que toutes les tâches du vecteur ne sont pas exécutées puisque le fichier sera lu au commencement de chaque nouvelle tâche.

== Exemple avec des paramètres multiples ==

Supposons que vous avez un script Python qui effectue des calculs avec certains paramètres définis dans une liste Python ou un tableau NumPy tel que

Le traitement de cette tâche peut se faire avec un vecteur de tâches pour que chaque valeur du paramètre beta soit traité en parallèle.
Il faut passer $SLURM_ARRAY_TASK_ID au script Python et obtenir le paramètre beta selon sa valeur.
Le script Python est maintenant

Le script pour soumettre la tâche est le suivant (remarquez que les paramètres vont de 0 à 99 tout comme les index du vecteur NumPy).