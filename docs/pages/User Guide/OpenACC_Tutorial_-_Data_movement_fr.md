---
title: "OpenACC Tutorial - Data movement/fr"
url: "https://docs.alliancecan.ca/wiki/OpenACC_Tutorial_-_Data_movement/fr"
category: "User Guide"
last_modified: "2017-05-12T23:35:29Z"
page_id: 3272
display_title: "Tutoriel OpenACC : Mouvement des données"
language: "fr"
---

== Gestion explicite des données  ==

Nous avons utilisé CUDA Unified Memory pour simplifier les premières étapes d'accélération de notre code.
Si le processus est plus simple, le code n'est cependant pas portable :
* pour PGI seulement, indicateur –ta=tesla:managed
* pour NVIDIA seulement, CUDA Unified Memory
La gestion explicite des données rend le code portable et peut améliorer la performance.

== Zones de données structurées ==
La directive data délimite la zone du code où les tableaux GPU restent sur le GPU et sont partagés par tous les noyaux de la zone.
Voici un exemple de comment se définit la zone de données structurées :

#pragma acc data
{
#pragma acc parallel loop ...
#pragma acc parallel loop
...
}

Un autre exemple :

!$acc data
!$acc parallel loop
...
!$acc parallel loop
...
!$acc end data

==Données non structurées==
Dans certains cas, la délimitation d'une zone ne permet pas l'utilisation de zones de données normales, par exemple quand on utilise des constructeurs ou des destructeurs.
=== Directives ===
Dans ces cas, on utilise des directives de données non structurées.
* enter data, définit le début de la durée de vie des données non structurées
** clauses : copyin(list), create(list)
* exit data, définit la fin de la durée de vie des données non structurées
** clauses : copyout(list), delete(list)'
Voici un exemple :

#pragma acc enter data copyin(a)
...
#pragma acc exit data delete(a)

=== Classes C++ ===
Quel est l'avantage des clauses de données non structurées? Elles permettent l'utilisation d'OpenACC dans les classes C++.
De plus, ces clauses peuvent être utilisées quand les données sont allouées et initialisées dans une portion du code différente de celle où les données sont libérées, par exemple dans les modules Fortran.

class Matrix { Matrix(int n) {
len = n;
v = new double[len];
#pragma acc enter data
                     create(v[0:len])
}
~Matrix() {
#pragma acc exit data
                     delete(v[0:len])
};

===Clauses de la directive data ===

* copyin(list ), pour allouer de la mémoire du GPU et copier des données de la mémoire de départ vers le GPU, à l'entrée de la zone
* copyout(list), pour allouer de la mémoire du GPU et copier des données vers la mémoire de départ, à la sortie de la zone
* copy(list), pour allouer de la mémoire du GPU et copier des données de la mémoire de départ vers le GPU à l'entrée de la zone et copier des données vers la mémoire de départ, à la sortie de la zone (données structurées seulement)
* create(list), pour allouer de la mémoire du GPU, sans copier
* delete(list), pour désallouer de la mémoire du GPU, sans copier (données non structurées seulement)
* present(list), le GPU contient déjà des données en provenance d'une autre région

=== Format des tableaux ===
Le compilateur ne peut pas toujours déterminer la taille d'un tableau; il faut donc en spécifier la taille et le format.
Voici un exemple en C

#pragma acc data copyin(a[0:nelem]) copyout(b[s/4:3*s/4])

et un exemple en Fortran.

!$acc data copyin(a(1:end)) copyout(b(s/4:3*s/4))

== Mouvement explicite des données ==
=== Copier dans la matrice ===
Dans cet exemple, nous commençons par allouer et initialiser la matrice. La matrice est ensuite copiée dans la mémoire. La copie se fait en deux étapes :
# Copier la structure de la matrice.
# Copier les membres de la matrice.

void allocate_3d_poisson_matrix(matrix &A, int N) {
   int num_rows=(N+1)*(N+1)*(N+1);
   int nnz=27*num_rows;
   A.num_rows=num_rows;
   A.row_offsets = (unsigned int*) \ malloc((num_rows+1)*sizeof(unsigned int));
   A.cols = (unsigned int*)malloc(nnz*sizeof(unsigned int));
   A.coefs = (double*)malloc(nnz*sizeof(double)); // Initialize Matrix
   A.row_offsets[num_rows]=nnz;
   A.nnz=nnz;
   #pragma acc enter data copyin(A)
   #pragma acc enter data copyin(A.row_offsets[:num_rows+1],A.cols[:nnz],A.coefs[:nnz])
}

=== Supprimer la matrice ===
Pour libérer la mémoire, il faut d'abord sortir la matrice puis énoncer la commande free. Ceci se fait en deux étapes, mais en sens inverse :
# Supprimer les membres.
# Supprimer la structure.

void free_matrix(matrix &A) {
   unsigned int *row_offsets=A.row_offsets;
   unsigned int * cols=A.cols;
   double * coefs=A.coefs;
   #pragma acc exit data delete(A.row_offsets,A.cols,A.coefs)
   #pragma acc exit data delete(A)
   free(row_offsets);
   free(cols);
   free(coefs);
}

=== La clause present ===
Pour une gestion de haut niveau, il faut dire au compilateur que les données se trouvent déjà en mémoire.
La déclaration des variables locales devraient cependant se faire à l'intérieur de la fonction dans laquelle elles sont utilisées.

function main(int argc, char **argv) {
#pragma acc data copy(A) {
    laplace2D(A,n,m);
}
}
...
function laplace2D(double[N][M] A,n,m){
   #pragma acc data present(A[n][m]) create(Anew)
   while ( err > tol && iter < iter_max ) {
      err=0.0;
      ...
   }
}

Dans le prochain exemple, la zone de calcul dans le code contient l'information qui indique au compilateur que les données sont déjà présentes.

#pragma acc kernels \
present(row_offsets,cols,Acoefs,xcoefs,ycoefs)
{
   for(int i=0;i<num_rows;i++) {
      double sum=0;
      int row_start=row_offsets[i];
      int row_end=row_offsets[i+1];
      for(int j=row_start;j<row_end;j++) {
         unsigned int Acol=cols[j];
         double Acoef=Acoefs[j];
         double xcoef=xcoefs[Acol];
         sum+=Acoef*xcoef;
      }
   ycoefs[i]=sum;
   }
}

=== Compiler et exécuter avec une gestion explicite de la mémoire ===
Pour faire un nouveau build sans mémoire autogérée, remplacez -ta=tesla:managed par -ta-tesla dans le Makefile.

=== La directive update  ===
Cette directive permet d'actualiser un tableau ou une partie d'un tableau.

do_something_on_device()
!$acc update self(a)   //  Copy "a" from GPU to CPU
do_something_on_host()
!$acc update device(a)  // Copy "a" from CPU to GPU

Dans cet autre exemple,  nous modifions d'abord un vecteur dans la mémoire du CPU de départ, puis nous le copions dans la mémoire du GPU.

void initialize_vector(vector &v,double val) {
   for(int i=0;i<v.n;i++)
      v.coefs[i]=val;   // Updating the vector on the CPU
   #pragma acc update
      device(v.coefs[:v.n])    // Updating the vector on the GPU
}

=== Développer et exécuter sans mémoire autogérée ===
Nous voyons ici la performance du code avec et sans mémoire autogérée.

Dans cet exemple, des essais ont été faits avec et sans l'option -ta=tesla:managed .

Les résultats démontrent que certains tests avec mémoire autogérée améliorent la vitesse; ceci est probablement dû à la mémoire immobilisée (pinned memory). De façon générale, il semble que la localité fonctionne : quand la plupart des opérations sont effectuées sur le GPU et que les données y demeurent longtemps, le mouvement des données n'a pas d'incidence majeure sur la performance.

Page suivante, Optimisation des boucles
Retour au début du tutoriel