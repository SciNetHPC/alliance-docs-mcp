---
title: "MPI-IO/fr"
url: "https://docs.alliancecan.ca/wiki/MPI-IO/fr"
category: "General"
last_modified: "2021-07-06T15:57:21Z"
page_id: 13003
display_title: "MPI-IO"
language: "fr"
---

== Description ==
Partie du standard MPI-2, MPI-IO est une famille de routines MPI qui permet l'enregistrement d'opérations parallèles de lecture et d'écriture. Le principal avantage de MPI-IO est de pouvoir, de manière simple et efficace, lire et écrire des données réparties sur plusieurs processus en un seul fichier commun à tous les processus. Ceci s'avère particulièrement utile lorsque les données manipulées sont des vecteurs ou des matrices découpés de manière structurée entre les différents processus. Vous trouverez ici quelques indications à propos de l'utilisation de MPI-IO et des références vers des documents plus complets

== Utilisation==

=== Opérations par déplacements ===

La manière la plus simple de faire des opérations de lecture et écriture en parallèle est d'utiliser des déplacements (offsets).  Chaque processus peut ainsi lire ou écrire dans le fichier avec un déplacement défini. Cela peut se faire en deux opérations (MPI_File_seek suivie de MPI_File_read ou de MPI_File_write), ou bien en une seule opération (MPI_File_read_at ou MPI_File_write_at). On calcule habituellement le déplacement en fonction du rang du processus.
 MPI_MODE_CREATE), MPI_INFO_NULL, &f);

    /* Write data alternating between the processes: aabbccddaabbccdd... */
    MPI_File_seek(f, rank*BLOCKSIZE, MPI_SEEK_SET); /* Go to position rank * BLOCKSIZE */
    for (i=0; i<NBRBLOCKS; ++i) {
        MPI_File_write(f, buffer, BLOCKSIZE, MPI_CHAR, MPI_STATUS_IGNORE);
        /* Advance (size-1)*BLOCKSIZE bytes */
        MPI_File_seek(f, (size-1)*BLOCKSIZE, MPI_SEEK_CUR);
    }

    MPI_File_close(&f);

    MPI_File_open(MPI_COMM_WORLD, filename, MPI_MODE_RDONLY, MPI_INFO_NULL, &f);

    /* Read data in a serial fashion for each process. Each process reads: aabbccdd */
    for (i=0; i<NBRBLOCKS; ++i) {
        MPI_File_read_at(f, rank*i*NBRBLOCKS*BLOCKSIZE, buffer, BLOCKSIZE, MPI_CHAR, MPI_STATUS_IGNORE);
    }

    MPI_File_close(&f);
    MPI_Finalize();

    return 0;
}
}}
=== Utiliser les vues ===

En utilisant les vues, chaque processus peut voir une section du fichier, comme si c'était le fichier en entier. De cette manière, il n'est plus nécessaire de calculer les déplacements dans le fichier en fonction du rang du processus. Une fois la vue définie, il est beaucoup plus simple d'effectuer des opérations sur le fichier sans craindre d'entrer en conflit avec les opérations effectuées par les autres processus. On définit une vue à l'aide de la fonction MPI_File_set_view. Voici un programme identique à celui de l'exemple précédent, mais en utilisant les vues.
 MPI_MODE_CREATE),
        MPI_INFO_NULL,
        &f);

    /* Write data alternating between the processes: aabbccddaabbccdd... */
    MPI_Type_contiguous(BLOCKSIZE, MPI_CHAR, &type_intercomp);
    MPI_Type_commit(&type_intercomp);
    for (i=0; i<NBRBLOCKS; ++i) {
        MPI_File_set_view(f, rank*BLOCKSIZE+i*size*BLOCKSIZE, MPI_CHAR, type_intercomp, "native", MPI_INFO_NULL);
        MPI_File_write(f, buffer, BLOCKSIZE, MPI_CHAR, MPI_STATUS_IGNORE);
    }

    MPI_File_close(&f);

    MPI_File_open(MPI_COMM_WORLD,
        filename,
        MPI_MODE_RDONLY,
        MPI_INFO_NULL,
        &f);

    /* Read data in a serial fashion for each process. Each process reads: aabbccdd */
    MPI_Type_contiguous(NBRBLOCKS*BLOCKSIZE, MPI_CHAR, &type_contiguous);
    MPI_Type_commit(&type_contiguous);
    MPI_File_set_view(f, rank*NBRBLOCKS*BLOCKSIZE, MPI_CHAR, type_contiguous, "native", MPI_INFO_NULL);
    for (i=0; i<NBRBLOCKS; ++i) {
        MPI_File_read(f,  buffer, BLOCKSIZE, MPI_CHAR, MPI_STATUS_IGNORE);
    }

    MPI_File_close(&f);
    MPI_Finalize();

    return 0;
}
}}
Attention! Certains systèmes de fichiers ne supportent pas les verrous sur les fichiers (file locks). Par conséquent, certaines opérations ne sont pas possibles, notamment l'utilisation de vues sur des sections disjointes d'un fichier.

== Références ==

* Documentation OpenMPI
* Course on parallel I/O