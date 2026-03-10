---
title: "OpenACC Tutorial - Adding directives/fr"
url: "https://docs.alliancecan.ca/wiki/OpenACC_Tutorial_-_Adding_directives/fr"
category: "User Guide"
last_modified: "2023-06-08T19:37:03Z"
page_id: 3228
display_title: "Tutoriel OpenACC : Ajouter des directives"
language: "fr"
---

== Transfert vers un processeur graphique (GPU) ==
Avant de porter du code sur un GPU, il faut savoir que ceux-ci ne partagent pas la même mémoire que le CPU de l'hôte.
* la mémoire de l'hôte est en général plus grande, mais plus lente que la mémoire du GPU;
* un GPU n'a pas d'accès direct à la mémoire de l'hôte;
* pour pouvoir utiliser un GPU, les données doivent passer par le bus PCI, dont la bande passante est moins grande que celles du CPU et du GPU;
* il est donc de la plus haute importance de bien gérer les transferts entre la mémoire de départ et le GPU.  En anglais, ce processus s'appelle offloading.

==Directives OpenACC==
Les directives OpenAcc sont semblables aux directives OpenMP. En C/C++, ce sont des énoncés pragmas  et en Fortran, des commentaires. L'emploi de directives comporte plusieurs avantages :
* Premièrement, puisque le code est peu affecté, les modifications peuvent se faire de manière incrémentale, un pragma à la fois;  ceci est particulièrement utile pour le débogage puisqu'il est ainsi facile d'identifier le changement précis qui crée le bogue.
* Deuxièmement, OpenACC peut être désactivé au moment de la compilation;  les pragmas sont alors vus comme étant des commentaires et ne sont pas considérés par le compilateur, ce qui permet de compiler une version accélérée et une version normale à partir du même code source.
* Troisièmement, comme le compilateur fait tout le travail de transfert, le même code peut être compilé pour différents types d'accélérateurs, que ce soit un GPU ou des instructions SIMD sur un CPU;    ainsi, un changement du matériel exigera simplement la mise à jour du compilateur, sans modification au code.

Le code de notre exemple contient deux boucles : la première initialise deux vecteurs et la seconde effectue une opération de niveau 1 d'addition des vecteurs.

C/C++	FORTRAN

#pragma acc kernels
{
  for (int i=0; i prevents parallelization
              Loop carried backward dependence of ycoefs-> prevents vectorization
              Complex loop carried dependence of Acoefs->,xcoefs-> prevents parallelization
              Generating NVIDIA GPU code
              31, #pragma acc loop seq
              35, #pragma acc loop vector(128) /* threadIdx.x */
                  Generating implicit reduction(+:sum)
          35, Loop is parallelizable
}}

Le résultat montre que la boucle externe sur la ligne 31 n'a pas pu être parallélisée par le compilateur. Dans la prochaine section, nous expliquons comment traiter ces dépendances.

== Réparer les fausses dépendances de boucles ==
Même lorsque le programmeur sait qu'une boucle peut être parallélisée, il arrive que le compilateur ne le remarque pas. Un cas commun en C/C++ est connu sous le nom de pointer aliasing. Contrairement au Fortran, C/C++ ne possèdent pas comme tel de tableaux (arrays), mais plutôt des pointeurs. Le concept d'alias s'applique à deux pointeurs dirigés vers la même mémoire. Si le compilateur ne sait pas que des pointeurs ne sont pas des alias, il doit cependant le supposer. Dans l'exemple précédent, on voit clairement pourquoi le compilateur ne pouvait pas paralléliser la boucle. En supposant que les pointeurs sont identiques, il y a forcément dépendance des itérations de la boucle.

===Mot-clé restrict ===
Une des manières de dire au compilateur que les pointeurs ne sont pas des alias est d'utiliser  le mot-clé  restrict, introduit à cette fin dans C99.  Il n'y a toujours pas de manière standard pour ce faire en C++, mais chaque compilateur possède un mot-clé qui lui est propre. Dépendant du compilateur, on peut utiliser __restrict ou __restrict__. Les compilateurs du Portland Group et de NVidia utilisent __restrict. Pour savoir pourquoi il n'existe pas de standard en C++, consultez ce document. Ce concept est important pour OpenACC comme pour toute programmation C/C++, car les compilateurs peuvent effectuer plusieurs autres optimisations si les pointeurs ne sont pas des alias. Remarquez que le mot-clé se place après le pointeur puisque c'est à ce dernier qu'il se réfère, et non au type; autrement dit, la déclaration doit se lire float * __restrict A; plutôt que float __restrict * A;.

=== Boucle avec clause independent ===
Une autre façon de s'assurer que le compilateur traite les boucles de manière indépendante est de le spécifier explicitement  avec la clause independent. Comme toute autre directive prescriptive, le compilateur y est obligé et l'analyse qu'il pourrait faire ne sera pas considérée. En reprenant l'exemple de la section La directive kernels ci-dessus, nous avons :

#pragma acc kernels
{
#pragma acc loop independent
for (int i=0; i New Session ou cliquez sur le bouton correspondant dans la barre d'outils.
# Cliquez sur le bouton Browse à la droite du champ File pour le chemin.
## Changez le répertoire s'il y a lieu.
## Sélectionnez un exécutable construit avec des codes écrits avec des directives OpenACC et CUDA C/C++.
# Sous le champ Arguments, sélectionnez l'option Profile current process only.
# Cliquez sur Next > pour voir les autres options de profilage.
# Cliquez sur Finish pour lancer le profilage de l'exécutable.

Pour faire ceci, suivez ces étapes :
# Lancez nvvp avec la commande nvvp &   (le symbole & commande le lancement en arrière-plan).
# Sélectionnez  File -> New Session.
# Dans le champ File:, cherchez l'exécutable (nommé dans notre exemple challenge).
# Cliquez sur Next jusqu'à ce que vous puissiez cliquer sur Finish.

Le programme est exécuté et on obtient un tableau chronologique du déroulement (voir l'image). On remarque que le transfert de données entre le départ et l'arrivée occupe la plus grande partie du temps d'exécution, ce qui est fréquent quand du code est porté d'un CPU vers un GPU. Nous verrons comment ceci peut être amélioré dans la prochaine partie, Mouvement des données.

== La directive parallel loop ==
Avec la directive kernels, c'est le compilateur qui fait toute l'analyse; ceci est une approche descriptive pour porter du code. OpenACC offre aussi une approche prescriptive avec la directive parallel qui peut être combinée à la directive loop ainsi :

#pragma acc parallel loop
for (int i=0; i et -Minfoaccel aux indicateurs pour le compilateur.
}}

<- Page précédente, Profileurs | ^- Retour au début du tutoriel | Page suivante, Mouvement des données ->