---
title: "Using GPUs with Slurm/fr"
url: "https://docs.alliancecan.ca/wiki/Using_GPUs_with_Slurm/fr"
category: "General"
last_modified: "2026-02-04T12:27:11Z"
page_id: 5046
display_title: "Ordonnancement Slurm des tâches exécutées avec GPU"
language: "fr"
---

= Introduction =

Pour demander un ou plusieurs GPU pour une tâche Slurm, utilisez
  --gpus-per-node=:

Par exemple
  --gpus-per-node=a100:1

Ceci est pour un seul GPU A100, à moins que vous ajoutiez --nodes pour demander plusieurs nœuds.
Pour les modèles valides, voir la section GPU disponibles ci-dessous.

Vous pouvez aussi utiliser
  --gres=gpu::
Cependant, il est possible que ce format ne soit plus pris en charge. Nous vous recommandons de le remplacer par  --gpus-per-node.

Slurm prend en charge plusieurs autres directives que vous pouvez utiliser pour demander des GPU, par exemple --gpus, --gpus-per-socket, --gpus-per-task, --mem-per-gpu et --ntasks-per-gpu. Voyez la documentation de Slurm sur sbatch.  Notre équipe ne teste pas toutes les combinaisons; si vous n'obtenez pas le résultat voulu, contactez le soutien technique.

Pour l'information générale sur l'ordonnancement des tâches, consultez Exécuter des tâches.

= GPU disponibles =

Grappe   	Modèle de GPU	Identifiant de modèlepour Slurm	Remarques
Fir      	H100-80gb    	h100
Fir      	H100-80gb    	nvidia_h100_80gb_hbm3_1g.10gb  	MIG
Fir      	H100-80gb    	nvidia_h100_80gb_hbm3_2g.20gb  	MIG
Fir      	H100-80gb    	nvidia_h100_80gb_hbm3_3g.40gb  	MIG
Narval   	A100-40gb    	a100
Narval   	A100-40gb    	a100_1g.5gb                    	MIG
Narval   	A100-40gb    	a100_2g.10gb                   	MIG
Narval   	A100-40gb    	a100_3g.20gb                   	MIG
Narval   	A100-40gb    	a100_4g.20gb                   	MIG
Nibi     	H100-80gb    	h100
Nibi     	H100-80gb    	nvidia_h100_80gb_hbm3_1g.10gb  	MIG
Nibi     	H100-80gb    	nvidia_h100_80gb_hbm3_2g.20gb  	MIG
Nibi     	H100-80gb    	nvidia_h100_80gb_hbm3_3g.40gb  	MIG
Nibi     	MI300A-128gb 	(aucun; voir Nibi)
Rorqual  	H100-80gb    	h100
Rorqual  	H100-80gb    	nvidia_h100_80gb_hbm3_1g.10gb  	MIG; synonymes h100_1g.10gb, h100_1.10, h100_10gb
Rorqual  	H100-80gb    	nvidia_h100_80gb_hbm3_2g.20gb  	MIG; synonymes h100_2g.20gb, h100_2.20, h100_20gb
Rorqual  	H100-80gb    	nvidia_h100_80gb_hbm3_3g.40gb  	MIG; synonymes h100_3g.40gb, h100_3.40, h100_40gb
Trillium 	H100-80gb    	h100
Killarney	H100-80gb    	h100                           	 
Killarney	L40S-48gb    	l40s                           	 
tamIA    	H100-80gb    	h100                           	 
tamIA    	H200         	h200                           	 
Vulcan   	L40S-48gb    	l40s                           	 
Vulcan

La commande ci-dessous présente les identifiants de GPU (et MIG) disponibles sur chacune des grappes. Cette commande est utile si le tableau plus haut n'a pas été mis à jour récemment.

grep gpused 's/gpu://g'sed 's/),/\n/g'cut -d: -f1sortuniq}}

Sur certaines grappes, il existe des identifiants courts pour quelques modèles de MIG; cette commande ne les fournit pas.

De plus, la présence d'un modèle de GPU ne garantit pas que vous pouvez utiliser un identifiant de modèle correspondant; des restrictions supplémentaires peuvent s'appliquer, dépendant notamment de votre groupe de recherche.

Pour plus d'information, cliquez sur le nom de la grappe dans le tableau ci-dessus, ou contactez le soutien technique.

Si vous ne spécifiez pas de modèle, votre tâche risque d'être rejetée ou acheminée vers une instance de GPU arbitraire.

Très peu de programmes sont capables d'utiliser efficacement un modèle arbitraire, c'est pourquoi nous recommandons vivement de toujours spécifier un identifiant de modèle dans vos scripts de tâches.

Des GPU sont disponibles sur Arbutus, mais comme c'est le cas pour les autres ressources infonuagiques, il n'est pas possible de soumettre des tâches via Slurm.
Pour plus d'information, voir Ressources infonuagiques.

== GPU multi-instances (MIG) ==
La technologie MIG permet de partitionner un GPU en plusieurs instances. Vos tâches pourraient utiliser un MIG plutôt qu'un GPU entier. Pour plus d'information, voir GPU multi-instances.

= Demander des cœurs CPU et la mémoire système =

Avec chaque instance GPU, une tâche doit avoir un nombre de cœurs CPU (1 par défaut) et une certaine quantité de mémoire système. Pour le maximum de cœurs CPU et de mémoire système, voir le tableau des ratios.

= Exemples =

== Tâches avec un seul cœur ==
Pour une tâche qui nécessite un seul cœur CPU et un GPU,

== Tâches multifils ==
Pour une tâche GPU qui nécessite plusieurs CPU dans un seul nœud,

Pour chaque GPU demandé, nous recommandons
* sur Fir, un maximum de 12 cœurs CPU
* sur Narval, un maximum de 12 cœurs CPU
* sur Nibi, un maximum de 14 cœurs CPU
* sur Rorqual, un maximum de 16 cœurs CPU

== Tâches MPI ==

== Nœuds entiers  ==
Si votre application peut utiliser efficacement un nœud entier et ses GPU associés, vous pouvez probablement réduire le temps d'attente si vous demandez un nœud entier. Utilisez les scripts suivants comme modèle.

===Regroupement de tâches pour un seul GPU===

Pour exécuter pendant plus de 24 heures quatre programmes qui utilisent un seul GPU ou deux programmes qui utilisent deux GPU, nous recommandons GNU Parallel. Voici un exemple simple :

cat params.input | parallel -j4 'CUDA_VISIBLE_DEVICES=$(({%} - 1)) python {} &> {#}.out'

L'identifiant du GPU est calculé en soustrayant 1 de l'identifiant de la fente (slot), représenté par {%}. L'identifiant de la tâche est représenté par {#}, avec des valeurs partant de 1.

Le fichier params.input devrait contenir les paramètres sur des lignes distinctes, comme suit :

code1.py
code2.py
code3.py
code4.py
...

Vous pouvez ainsi soumettre plusieurs tâches. Le paramètre -j4 fait en sorte que GNU Parallel exécutera quatre tâches concurremment en lançant une tâche aussitôt que la précédente est terminée. Pour éviter que deux tâches se disputent le même GPU, utilisez CUDA_VISIBLE_DEVICES.

== Profilage des tâches avec GPU ==

Sur Narval et Rorqual le profilage est possible, mais
DCGM (NVIDIA Data Center GPU Manager)
doit être désactivé. Ceci doit se faire lorsque vous soumettez la tâche en configurant la variable d'environnement DISABLE_DCGM.

1 salloc --accountdef-someuser --gpus-per-nodea100:1 --mem4000M --time03:00}}

Ensuite, dans votre tâche interactive, attendez que DCGM soit désactivé sur le nœud.
 grep 'Hostengine build info:')" ]; do  sleep 5; done}}

Enfin, lancez le profileur (voir débogage et profilage pour les détails).

Sur Fir et Nibi, le profilage de GPU décrit ci-dessus n'est pas encore disponible.

= Voir aussi =
CUDA
GPU multi-instances
Exécuter des tâches