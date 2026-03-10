---
title: "Fortran/fr"
url: "https://docs.alliancecan.ca/wiki/Fortran/fr"
category: "General"
last_modified: "2019-03-05T21:25:31Z"
page_id: 3473
display_title: "Fortran"
language: "fr"
---

Fortran est un langage compilé disponible sur les ordinateurs de Calcul Canada où sont installés les compilateurs gfortran et ifort. En général, les langages compilés offrent une meilleure performance; nous vous encourageons donc à écrire vos programmes en Fortran, C ou C++.

== Options utiles de compilation ==

La plupart des compilateurs Fortran modernes offrent des options utiles pour le débogage.
* -fcheck=all pour le compilateur gfortran et -check pour le compilateur ifort vérifient les limites des tableaux et signalent les pointeurs sans cible et les variables non initialisées;
* -fpe0 (ifort) interrompt l'application dans des cas de virgule flottante (division par zéro ou racine carrée d'un nombre négatif) plutôt que de simplement générer NaN (not a number) et laisser l'application se poursuivre;
* pendant les tests, utilisez -O0 pour désactiver les optimisations et -g pour ajouter les symboles de débogage.

==Algèbre linéaire numérique==

À partir de Fortran 90, de nouvelles fonctions sont disponibles pour le traitement des opérations de base : matmul et dot_product pour les multiplications avec matrices et vecteurs; transpose pour la transposition de matrices. Utilisez toujours ces fonctions ou les librairies BLAS/LAPACK fournies et n'essayez jamais de créer vos propres méthodes, à moins que ce ne soit pour des motifs d'apprentissage. La routine BLAS pour la multiplication de matrices peut s'avérer 100 fois plus rapide que l'algorithme primaire avec trois boucles imbriquées.

==Erreurs de segmentation==

Une erreur fréquemment observée avec un exécutable Fortran provient de problèmes d'interface. Ces problèmes surviennent lorsque l'on transmet comme argument d'une sous-routine un pointeur, un tableau alloué dynamiquement ou encore un pointeur de fonctions. À la compilation il n'y a pas de problème, cependant à l'exécution vous obtiendrez par exemple le message suivant :
; forrtl: severe (174): SIGSEGV, segmentation fault occurred
Pour corriger le problème, il faut s'assurer que l'interface de la sous-routine est définie explicitement. Ceci peut se faire en Fortran avec la commande INTERFACE.  Ainsi, le compilateur arrivera à construire l'interface et les erreurs de segmentation seront réglées.

Dans le cas où l'argument est un tableau allouable, il s'agit de remplacer le code suivant

par le code

Le principe est le même dans le cas où l'argument est un pointeur de fonction. Considérons, par exemple, le code suivant :

Pour ne pas obtenir d'erreur de segmentation, il faut remplacer le code précédent par ce qui suit :