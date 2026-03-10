---
title: "Pthreads/fr"
url: "https://docs.alliancecan.ca/wiki/Pthreads/fr"
category: "General"
last_modified: "2023-05-08T18:09:31Z"
page_id: 1860
display_title: "Parallélisation pthreads"
language: "fr"
---

=Introduction=
Le terme pthreads provient de POSIX threads, l'une des premières techniques de parallélisation. Tout comme les autres outils faisant usage de fils d'exécution, pthreads s'emploie dans un contexte de mémoire partagée et donc habituellement sur un seul nœud où le nombre de fils actifs est limité aux cœurs CPU disponibles sur ce nœud. On utilise pthreads dans plusieurs langages de programmation, mais surtout en C. En Fortran, la parallélisation de fils d'exécution se fait préférablement avec OpenMP. En C++, les outils de la bibliothèque Boost issus de la norme C11 sont mieux adaptés à la programmation orientée-objet.

La bibliothèque pthreads a servi de base aux approches de parallélisation qui ont suivi, dont OpenMP. On peut voir pthreads comme étant un ensemble d'outils primitifs offrant des fonctionnalités élémentaires de parallélisation, contrairement aux APIs conviviales et de haut niveau comme OpenMP. Dans le modèle pthreads, les fils sont générés dynamiquement pour exécuter des sous-procédures dites légères qui exécutent les opérations de façon asynchrone; ces fils sont ensuite détruits après avoir réintégré le processus principal. Puisque tous les fils d'un même programme résident dans le même espace mémoire, il est facile de partager les données à l'aide de variables globales, contrairement à une approche distribuée comme MPI; toute modification aux données partagées risque cependant de créer des situations de compétition  (race conditions).

Pour paralléliser un programme avec pthreads ou toute autre technique, il importe de considérer la capacité du programme à s'exécuter en parallèle, ce que nous appellerons sa scalabilité. Après avoir parallélisé votre locigiel et que sa qualité vous satisfait, nous vous recommandons d'effectuer une analyse de sa scalabilité pour en comprendre la performance.

=Compilation=
Pour utiliser les fonctions et structures de données associées à pthreads dans votre programme C, il faut y inclure le fichier d'en-tête (header file) pthread.h et compiler le programme avec un indicateur (flag) pour faire le lien avec la bibliothèque pthreads.

Le nombre de fils pour le programme est défini par une des méthodes suivantes :
* utilisé comme argument dans une ligne de commande;
* entré via une variable d'environnement;
* encodé dans le fichier source (ceci ne permet toutefois pas d'ajuster le nombre de fils à l'exécution).

=Création et destruction des pthreads=
Pour paralléliser avec pthreads un programme séquentiel existant, nous utilisons un modèle de programmation où les fils sont créés par un parent, exécutent une partie du travail, puis sont réintégrés au parent. Le parent est soit le fil maître séquentiel ou un des autres fils esclaves.

La fonction pthread_create crée des nouveaux fils avec ces quatre arguments :
*l'identifiant unique pour le nouveau fil;
*l'ensemble des attributs du fil;
*la fonction C que le fil exécute lorsqu'il est amorcé (la routine de lancement);
*l'argument de la routine de lancement.

Dans cet exemple, l'index du fil (de 0 à 11) est passé en argument;  la fonction task est donc exécutée par chacun des 12 fils. Remarquez que  la fonction pthread_create ne bloque pas le  fil maître, qui continue à exécuter la fonction main après la création de chacun des fils. Une fois les 12 fils créés, le fil maître entre dans la deuxième boucle for et appelle la fonction bloquante pthread_join : le fil maître attend alors que les 12 fils esclaves terminent l'exécution de la fonction task et qu'ils réintègrent ensuite le fil maître. Cet exemple simple illustre bien le fonctionnement de base d'un fil POSIX : le fil maître crée un fil en lui assignant une fonction à exécuter et attend ensuite que le fil créé termine cette fonction,  puis réintégre le fil maître.

En exécutant ce code plusieurs fois de suite, vous noterez probablement une variation dans l'ordre dans lequel les fils esclaves disent hello, ce qui est prévisible puisqu'ils s'exécutent en mode asynchrone. Chaque fois que le programme est exécuté, les 12 fils répondent en même temps à la fonction printf et ce n'est jamais le même fil qui remporte la course.

=Synchronisation de l'accès aux données=
Dans un programme réel, les fils esclaves doivent lire et dans certains cas modifier les données afin d'accomplir leurs tâches. Ces données sont habituellement un ensemble de variables globales de divers types et dimensions; l'accès concurrent en lecture et en écriture par plusieurs fils doit donc être synchronisé afin d'éviter les situations de compétition, c'est-à-dire les cas où le résultat du programme dépend de l'ordre dans lequel les fils esclaves accèdent aux données. Si un programme en parallèle doit donner le même résultat que sa version en série, les situations de compétition ne doivent pas se produire.

Le moyen le plus simple et le plus utilisé pour contrôler l'accès concurrent est le verrou; dans le contexte de pthreads,  le mécanisme de verrouillage est le mutex (pour mutual exclusion). Les variables de ce type sont assignées à un seul fil à la fois. Après la lecture ou la modification, le fil désactive le verrou. Le code entre l'appel de la variable et le moment où elle est désactivée est exécuté exclusivement par ce fil. Pour créer un mutex, il faut déclarer une variable globale de type pthread_mutex_t. Cette variable est initialisée par la fonction pthread_mutex_init. À la fin du programme, les ressources sont déverrouillées par la fonction pthread_mutex_destroy.

Dans cet exemple basé sur le contenu du fichier thread.c plus haut, l'accès au canal de sortie standard est sérialisé comme il se doit avec un mutex. L'appel de pthread_mutex_lock effectue le blocage, c'est-à-dire que le fil attendra indéfiniment que le mutex devienne disponible. Il faut s'assurer que le code ne provoque pas d'autre blocage puisque le mutex doit éventuellement devenir disponible. Ceci pose problème surtout dans un programme réel qui comporte plusieurs variables mutex contrôlant l'accès à différentes structures de données globales.

Dans le cas de l'alternative non bloquante pthread_mutex_trylock, la valeur non nulle est immédiatement produite si le mutex n'est pas accompli, indiquant ainsi que le mutex est occupé. Il faut aussi s'assurer qu'il n'y a pas de code superflu à l'intérieur du bloc sérialisé; puisque ce code est exécuté en série, il doit être le plus concis possible afin de ne pas nuire au parallélisme dans l'exécution du programme.

Une synchronisation plus subtile est possible avec le verrou lecture/écriture pthread_rwlock_t. Cet outil permet la lecture simultanée d'une variable par plusieurs fils, mais se comporte comme un mutex standard, c'est-à-dire qu'aucun autre fil n'a accès à cette variable (en lecture ou en écriture). Comme pour le mutex, le verrou pthread_rwlock_t doit être initialisé avant son utilisation et détruit quand il n'est plus nécessaire. Un fil obtient un verrou en lecture avec  pthread_rwlock_rdlock et un verrou en écriture avec pthread_rwlock_wrlock. Dans les deux cas, le verrou est détruit avec pthread_rwlock_unlock.

Un autre outil permet à plusieurs fils d'agir sur la même condition, par exemple d'attendre que les fils esclaves soient sollicités pour une tâche. Il s'agit d'une variable-condition, exprimée comme suit : pthread_cond_t. Comme le mutex ou le verrou lecture/écriture, la variable-condition doit être initialisée avant son utilisation et détruite lorsqu'elle n'est plus nécessaire. Pour utiliser cette variable-condition, un mutex doit contrôler l'accès aux variables qui ont une incidence sur la condition. Un fil en attente d'une condition verrouille le mutex et fait appel à la fonction pthread_cond_wait avec la variable-condition et le mutex en arguments. Le mutex est détruit atomiquement avec la création de la variable-condition dont le résultat est attendu par le fil; les autres fils peuvent alors verrouiller le mutex, soit pour attendre la condition ou pour modifier une ou plusieurs de variables, ce qui modifiera la condition.

Dans cet exemple, deux fils esclaves modifient la valeur de l'entier workload dont la valeur initiale doit être plus petite ou égale à 25. Le premier fil verrouille le mutex et attend parce que workload <= 25; la variable-condition ticker est créée et le mutex est détruit. Le deuxième fil peut alors exécuter la boucle, qui elle incrémente de trois la valeur de workload à chaque itération. À chaque incrémentation, le deuxième fil vérifie si la valeur de workload est plus grande que 25; si c'est le cas, le fil appelle pthread_cond_signal pour signaler au fil en attente que la condition est satisfaite. Une fois que le signal est reçu par le premier fil, le deuxième fil fixe la condition de sortie de la boucle, amorce le mutex et disparait avec pthread_join. Entretemps, le premier fil étant réveillé, celui-ci incrémente de 15 la valeur de workload et quitte la fonction task. Quand tous les fils esclaves sont réintégrés, le fil maître imprime la valeur finale de workload et le programme se termine.

De façon générale, dans un programme réel où plusieurs fils sont en attente d'une variable-condition, la fonction pthread_cond_broadcast signale à tous les fils en attente que la condition est satisfaite. Dans ce contexte, pthread_cond_signal avertirait un seul fil au hasard et les autres fils demeureraient en attente.

=Pour en savoir plus=
Pour plus d'information sur pthreads, sur les arguments optionnels pour les diverses fonctions (les paramètres utilisés dans cette page utilisent l'argument par défaut NULL) et sur les sujets de niveau avancé, nous recommandons l'ouvrage de David Butenhof,  Programming with POSIX Threads ou l'excellent tutoriel du Lawrence Livermore National Laboratory.