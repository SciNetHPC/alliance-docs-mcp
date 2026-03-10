---
title: "Mounting /project space on a VM in the cloud/fr"
url: "https://docs.alliancecan.ca/wiki/Mounting_/project_space_on_a_VM_in_the_cloud/fr"
category: "General"
last_modified: "2025-08-29T15:34:46Z"
page_id: 11498
display_title: "Préparer l'espace projet dans une instance infonuagique"
language: "fr"
---

===Introduction===

Nous décrivons ici comment une instance virtuelle peut accéder aux systèmes de fichiers de l'espace projet. Avec nos nuages, les instances infonuagiques ne peuvent directement accéder les grappes de CHP. Pour avoir accès aux fichiers de l'espace projet à partir d'une instance virtuelle, utilisez SSHFS et respectez les exigences particulières.

===SSHFS===

SSHFS permet de configurer votre répertoire /project dans votre instance. Ces répertoires sont semblables aux autres types de répertoires et vous pouvez y accéder par les commandes Linux régulières.

Pour des informations, consultez la référence https://wiki.archlinux.org/index.php/SSHFS.

===Exigences particulières===

Pour éviter les problèmes de sécurité, respectez les exigences suivantes :

* NE CONSERVEZ PAS votre mot de passe en texte brut dans l'instance.
* Créez une clé SSH EXCLUSIVEMENT pour SSHFS. N'utilisez pas la clé SSH qui sert à vous connecter.
* Gardez votre instance à jour et n'ouvrez que les ports sécuritaires qui sont nécessaires.