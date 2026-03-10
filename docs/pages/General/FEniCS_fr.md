---
title: "FEniCS/fr"
url: "https://docs.alliancecan.ca/wiki/FEniCS/fr"
category: "General"
last_modified: "2026-01-13T12:17:52Z"
page_id: 3994
display_title: "FEniCS/fr"
language: "fr"
---

FEniCS est une plateforme de logiciels libres pour la résolution d'équations aux dérivées partielles.

Puisque la plateforme peut être construite avec diverses extensions, nous n'offrons pas une installation centrale unique, mais vous avez le choix entre
*l'installation dans un environnement virtuel,
*l'utilisation d'un conteneur Singularity.

= Installation dans un environnement virtuel=
Les instructions suivantes installent la version 2019.1.0 dans StdENv/2020 avec OpenMPI et GCC 9.3.0.

Vous pouvez exécuter le script ci-dessous en le copiant sur la grappe que vous utilisez et en lançant bash fenics-install.sh.

À l'installation, une notification vous informe qu'un nouveau répertoire sera créé pour l'application, ou que le répertoire sera remplacé si l'application s'y trouve déjà. Les directives d'utilisation seront affichées quand l'installation sera terminée. Le script peut être modifié pour spécifier un répertoire différent.

==Plugiciels==

Cette section n'est pas à jour pour StdEnv/2020.

Installez d'abord FEniCS en suivant les directives ci-dessus.

===mshr===

Ensuite, exécutez

= Utilisation d'un conteneur Singularity  =

Le pilote Singularity Recipe télécharge l'image FEniCS Docker, l'installe et télécharge des paquets additionnels comme Python, par exemple. Exécutez le pilote sur votre ordinateur où Singularity est installé sous Linux et où vous avez toutes les permissions.

Utilisez la commande

  sudo singularity build FEniCS.simg FEniCS-ComputeCanada-Singularity-Recipe

Téléversez ensuite FEniCS.simg dans votre compte. L'image FEniCS Docker place plusieurs fichiers dans /home/fenics.

= Installation de FEniCS Legacy (2019) sur Trillium =

Dans votre répertoire /home, configurez et testez le conteneur pour la version FEniCS Legacy 2019 avec les instructions suivantes.

== 1. Téléchargez l'image Docker au format Apptainer SIF ==

apttainer pull fenics-legacy.sif docker://ghcr.io/scientificcomputing/fenics-gmsh:2024-05-30

== 2.  Créez un bac à sable (sandbox) avec permission d’écriture ==
Créez l’arborescence fenics-legacy.sandbox à partir du fichier SIF afin de modifier ou installer d’autres paquets.

apptainer build --sandbox fenics-legacy.sandbox fenics-legacy.sif

Remarques :
* la commande crée le répertoire fenics-legacy.sandbox
* Yvous pouvez modifier le nom du répertoire (par exemple fenics-dev/ ou my_rw_image/)
* le suffixe .sandbox est optionnel

== 3. Certificat pip ==
Inside the sandbox, create a certs folder and symlink the CA bundle so pip/SSL trusts HTTPS:

apptainer exec --writable fenics-legacy.sandbox sh -c "mkdir -p /etc/pki/tls/certs && ln -s /etc/ssl/certs/ca-certificates.crt /etc/pki/tls/certs/ca-bundle.crt"

== 4. Créez un nouveau SIF à partir du bac à sable ==
Une fois les modifications du bac à sable terminées, utilisez-le pour créer une nouvelle image en lecture seulement. Cette image est portable et permet des calculs reproductibles.

apptainer build fenics-legacy-updated.sif fenics-legacy.sandbox

== 5. Effectuez ces courts tests ==

apptainer exec --bind $PWD:/root/shared --pwd /root/shared fenics-legacy-updated.sif python3 -c "import ufl_legacy; print('ufl_legacy ok. version:', ufl_legacy.__version__)"

Remarques :
* --bind $PWD:/root/shared monte le répertoire hôte actif dans le conteneur
* --pwd définit le répertoire de travail

== Remarques importantes ==
* FEniCS Legacy (2019.1.x) nécessite UFL Legacy, already bundled.
* Le paquet Python se nomme ufl_legacy, et non ufl.
* La version UFL compatible UFL version is 2022.3.0 (provided by ufl_legacy).
* L'instruction import ufl devrait échouer, mais non import ufl_legacy.

== Créez un alias ulf ==
Certains logiciels (tels que Oasis) assument que le nom du module à charger est ufl. Plutôt que de modifier ces logiciels, vous pouvez créer un shim qui réexporte ufl_legacy sous le nom ufl.

Créez le fichier  /pyshims/ufl/__init__.py qui contient

import sys
import ufl_legacy as ufl

api = [k for k in ufl.__dict__.keys() if not k.startswith('__') and not k.endswith('__')]
for key in api:
    sys.modules['ufl.{}'.format(key)] = getattr(ufl, key)
del api

== Testez l'alias ==
Ajoutez le chemin de l’alias (shim) à PYTHONPATH lors de l’utilisation du conteneur.

APPTAINERENV_PYTHONPATH=:$PYTHONPATH apptainer exec --bind /scratch:/scratch ~/fenics-legacy-updated.sif python3 -c "from ufl.tensors import ListTensor; print('UFL tensors ok')"