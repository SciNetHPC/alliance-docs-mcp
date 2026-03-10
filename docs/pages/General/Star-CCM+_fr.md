---
title: "Star-CCM+/fr"
url: "https://docs.alliancecan.ca/wiki/Star-CCM%2B/fr"
category: "General"
last_modified: "2026-02-26T07:24:58Z"
page_id: 5482
display_title: "Star-CCM+"
language: "fr"
---

STAR-CCM+ est une suite logicielle de simulation utilisée dans plusieurs spécialités de génie. Elle permet la modélisation dans des domaines variés dont l'acoustique, la dynamique des fluides, le transfert thermique, la rhéologie, l'écoulement polyphasique, le flux de particules, la mécanique des solides, les fluides réactifs, l'électrochimie et l'électromagnétisme.

= Limites de la licence =
Les binaires STAR-CCM+ sont installés sur nos serveurs, mais nous n'avons pas de licence pour utilisation générale; vous devez donc posséder votre propre licence.
Vous pouvez acheter une licence POD (Power On Demand) directement de Siemens. Autrement, vous pouvez utiliser une licence locale hébergée par votre établissement pourvu que le pare-feu permette à la grappe où les tâches seront exécutées d'y accéder.

== Configurer votre compte ==
Afin de configurer votre compte pour utiliser un serveur de licence avec le module Star-CCM+, créez le fichier  $HOME/.licenses/starccm.lic comme suit :

où server et port sont remplacés respectivement par le nom de l'hôte (ou l'adresse IP) et le port statique du fournisseur du serveur de licence. Il n'est pas nécessaire de définir manuellement CDLMD_LICENSE_FILE comme étant égal à @ dans votre script slurm; au lieu de cela, lorsqu'un module Star-CCM+ est chargé, cette variable est automatiquement spécifiée dans votre fichier $HOME/.licenses/starccm.lic.

=== Fichier pour une licence POD ===

Si vous avez acheté une licence POD de Siemens votre variable d'environnement LM_PROJECT doit être manuellement configurée comme étant égale à YOUR CD-ADAPCO PROJECT ID dans votre script slurm. De plus, le fichier ~/.licenses/starccm.lic doit être configuré comme suit sur toutes les grappes.

= Soumettre des tâches en lot sur nos grappes =

Quand vous soumettez des tâches sur une grappe pour la première fois, vous devrez configurer votre environnement pour l’utilisation de votre licence. Si vous utilisez le serveur de licences distant pay-on-usage de Siemens, créez le fichier ~/.licenses/starccm.lic  comme décrit ci-dessus dans Fichier pour une licence POD; ceci devrait fonctionner immédiatement. Par contre, si vous utilisez un serveur de licence de votre établissement, créez d'abord le fichier ~/.licenses/starccm.lic et soumettez une demande d'assistance au soutien technique.  Nous vous aiderons à coordonner les modifications du pare-feu réseau nécessaires pour y accéder (en supposant que le serveur n'a jamais été configuré pour communiquer via la grappe de l'Alliance que vous voulez utiliser). Si vous rencontrez toujours des problèmes pour faire fonctionner la licence, essayez de supprimer ou de renommer le fichier ~/.flexlmrc car les chemins de recherche et/ou les paramètres précédents du serveur de licence pourraient y être stockés. Notez que des fichiers de sortie de tâches déjà exécutées peuvent s'accumuler dans des répertoires cachés nommés .star-version_number et consommer ainsi votre quota. Ceux-ci peuvent être supprimés périodiquement en exécutant périodiquement rm -ri ~/.starccm* et en répondant oui à l'affichage de l'invite.

== Scripts pour l'ordonnanceur ==

 awk '{print $2}')
port=$(cat $CDLMD_LICENSE_FILE  grep -Eo '[0-9]+$')
nmap $server -Pn -p $port  grep -v '^$'; echo

export FLEXIBLAS=NETLIB
STAR_MPI="-mpi openmpi"
if [ "$RSNT_CPU_VENDOR_ID" == intel ]; then
  export FLEXIBLAS=StarMKL
  STAR_MPI="-mpi intel"
elif [ "$RSNT_CPU_VENDOR_ID" == amd ]; then
  export FLEXIBLAS=StarAOCL
fi
echo "FLEXIBLAS=$FLEXIBLAS"

if [ "${EBVERSIONSTARCCM:0:2}" -lt 20 ]; then
  STAR_FABRIC="-fabric ofi"
else
  STAR_FABRIC="-fabric ucx"
fi

if [ -n "$LM_PROJECT" ]; then
   echo "Siemens PoD license server ..."
   starccm+ -jvmargs -Xmx4G -jvmargs -Djava.io.tmpdir=$SLURM_TMPDIR -batch -power -podkey $LM_PROJECT -np $NCORE -nbuserdir $SLURM_TMPDIR -machinefile $SLURM_TMPDIR/machinefile $JAVA_FILE $SIM_FILE $STAR_MPI $STAR_FABRIC
else
   echo "Institutional license server ..."
   [ $(command -v lmutil) ] && lmutil lmstat -c ~/.licenses/starccm.lic -a  egrep "license1UPuse$USER"; echo
   starccm+ -jvmargs -Xmx4G -jvmargs -Djava.io.tmpdir=$SLURM_TMPDIR -batch -np $NCORE -nbuserdir $SLURM_TMPDIR -machinefile $SLURM_TMPDIR/machinefile $JAVA_FILE $SIM_FILE $STAR_MPI $STAR_FABRIC
fi
}}

 sleep 5
          echo "Attempt number: "$I
          if [ -n "$LM_PROJECT" ]; then
          echo "Siemens PoD license server ..."
          starccm+ -jvmargs -Xmx4G -jvmargs -Djava.io.tmpdir=$SLURM_TMPDIR -batch -power -podkey $LM_PROJECT -np $NCORE -nbuserdir $SLURM_TMPDIR -machinefile $SLURM_TMPDIR/machinefile $JAVA_FILE $SIM_FILE $STAR_MPI
        else
          echo "Institutional license server ..."
          starccm+ -jvmargs -Xmx4G -jvmargs -Djava.io.tmpdir=$SLURM_TMPDIR -batch -np $NCORE -nbuserdir $SLURM_TMPDIR -machinefile $SLURM_TMPDIR/machinefile $JAVA_FILE $SIM_FILE $STAR_MPI
        fi
        RET=$?
        i=$((i+1))
done
exit $RET
}}

= Mode graphique =

Pour travailler en mode graphique, nous recommandons d'utiliser un système OnDemand ou JupyterLab pour démarrer un bureau distant. En plus de configurer ~/.licenses/starccm.lic, les groupes qui possèdent une licence POD devraient aussi exécuter export LM_PROJECT='CD-ADAPCO PROJECT ID' avant starccm+, comme dans les exemples ci-dessous; selon le type de licence, il faut aussi ajouter d'autres options comme -power.  La commande module avail starccm-mixed affiche les versions de starccm qui sont disponibles dans l'environnement standard (StdEnv) que vous avez chargé. Autrement, la commande module spider starccm-mixed affiche toutes les versions de modules qui sont disponibles dans toutes les versions de modules StdEnv.

== OnDemand ==
1. Sur votre ordinateur, connectez-vous à un système OnDemand en entrant une des adresses URL dans le navigateur :
 NIBI: https://ondemand.sharcnet.ca
  FIR: https://jupyterhub.fir.alliancecan.ca
 NARVAL:  https://portail.narval.calculquebec.ca/
 RORQUAL: https://jupyterhub.rorqual.alliancecan.ca
 TRILLIUM: https://ondemand.scinet.utoronto.ca
2. Sur votre ordinateur, ouvrez une fenêtre de terminal avec une des commandes suivantes :
: STAR-CCM+ 18.04.008 (ou versions plus récentes)
:: module load StdEnv/2023  (par défault)
:: module load starccm-mixed/20.04.007 **OU** starccm/20.04.007-R8
:: starccm+ -rr server
: STAR-CCM+ 15.04.010 --> 18.02.008 (plages de versions)
:: module load StdEnv/2020 (non pris en charge)
:: module load starccm-mixed/15.04.010 **OU** starccm/15.04.010-R8
:: starccm+ -mesa

== VncViewer ==

1. Connectez-vous à un nœud de connexion ou un nœud de calcul avec TigerVNC.
2. Sur votre ordinateur, ouvrez une nouvelle fenêtre de terminal et lancez une des commandes suivantes :
: STAR-CCM+ 18.04.008 (ou versions plus récentes)
:: module load StdEnv/2023  (par défaut)
:: module load starccm-mixed/20.04.007 **OU** starccm/20.04.007-R8
:: starccm+ -rr server
: STAR-CCM+ 15.04.010 --> 18.02.008 (plage de versions)
:: module load StdEnv/2020 (non pris en charge)
:: module load starccm-mixed/17.02.007 **OU** starccm/17.02.007-R8
::  starccm+