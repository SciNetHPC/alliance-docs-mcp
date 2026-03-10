---
title: "MATLAB/fr"
url: "https://docs.alliancecan.ca/wiki/MATLAB/fr"
category: "General"
last_modified: "2026-01-21T13:12:07Z"
page_id: 4003
display_title: "MATLAB"
language: "fr"
---

Il y a deux façons d'utiliser MATLAB sur nos grappes :

1. Exécuter directement MATLAB, mais vous devez avoir accès à une licence, soit :
* la licence fournie sur Fir, Narval ou Trillium ou pour les étudiants, professeurs et chercheurs;
* une licence externe détenue par votre établissement, faculté, département ou laboratoire (voir la section Utiliser une licence externe ci-dessous).

2. Compiler votre code MATLAB avec le compilateur mcc et utiliser le fichier exécutable généré sur une de nos grappes. Vous pouvez utiliser cet exécutable sans tenir compte de la licence.

Vous trouverez ci-dessous les détails pour ces approches.

=Utiliser une licence externe=
Nous sommes fournisseurs d'hébergement pour MATLAB. Dans ce contexte, MATLAB est installé sur nos grappes et vous pouvez avoir accès à une licence externe pour utiliser notre infrastructure; dans le cas de certains établissements, ceci s'effectue de façon automatique. Pour savoir si vous avez accès à une licence, faites le test suivant :

[name@cluster ~]$ module load matlab/2023b.2
[name@cluster ~]$ matlab -nojvm -nodisplay -batch license

987654
[name@cluster ~]$

Si tout est en ordre, un numéro de licence sera imprimé. Assurez-vous d'effectuer ce test sur chaque grappe avec laquelle vous voulez utiliser MATLAB puisque certaines licences ne sont pas disponibles partout.

Si vous obtenez le message This version is newer than the version of the license.dat file and/or network license manager on the server machine, essayez d'entrer une version moins récente de MATLAB dans la ligne module load.

Autrement, il se peut que votre établissement n'ait pas de licence, qu'il ne soit pas possible d'utiliser la licence de cette manière ou qu'aucune entente n'ait été conclue avec nous pour utiliser la licence. Pour savoir si vous pouvez utiliser une licence externe, contactez l'administrateur de la licence MATLAB de votre établissement ou votre gestionnaire de compte MATLAB.

Si vous pouvez utiliser une licence externe, certaines opérations de configuration sont requises. D'abord, vous devez créer un fichier semblable à

et placer ce fichier dans le répertoire $HOME/.licenses/ où l'adresse IP et le numéro du port correspondent aux valeurs du serveur de licence de votre établissement. Notre équipe technique devra alors contacter le personnel technique qui gère votre licence pour que votre serveur puisse se connecter à nos nœuds de calcul. Pour organiser ceci, contactez le soutien technique.

Consultez la documentation technique http://www.mathworks.com/support et l'information sur le produit http://www.mathworks.com.

= Préparer votre répertoire .matlab =
Puisque le répertoire /home de certains nœuds de calcul n'est accessible qu'en lecture, vous devez créer un lien symbolique .matlab pour que le profil et des données des tâches soient plutôt consignés dans /scratch.

[name@cluster ~]$ cd $HOME
[name@cluster ~]$ if [ -d ".matlab" ]; then
  mv .matlab scratch/
else
  mkdir -p scratch/.matlab
fi && ln -sn scratch/.matlab .matlab

= Boîtes à outils =
Pour la liste des boîtes à outils disponibles avec la licence et la grappe sur laquelle vous travaillez, utilisez

[name@cluster ~]$  module load matlab
[name@cluster ~]$  matlab -nojvm -batch "ver"

= Exécuter un programme séquentiel MATLAB =

Important : Pour tous les calculs d'envergure (durée de plus de cinq minutes ou mémoire d'un Go), la tâche doit être soumise à l'ordonnanceur comme démontré dans l'exemple suivant. Pour plus d'information, consultez Exécuter des tâches.

Voici un exemple de code ː

Voici un script pour l'ordonnanceur Slurm qui exécute cosplot.m :

Soumettez la tâche avec sbatch.

Chaque fois que MATLAB est lancé, un fichier comme java.log.12345 pourrait être créé. Pour économiser l'espace de stockage, ce fichier doit être supprimé une fois que MATLAB est fermé. La création de ce fichier peut cependant être évitée en utilisant l'option
-nojvm, mais ceci pourrait interférer avec certaines fonctions de traçage.

Pour plus d'information sur les options en ligne de commande dont -nodisplay, -nojvm,
-singleCompThread, -batch et autres, voir MATLAB (Linux) le site de MathWorks.

= Exécuter en parallèle =

MATLAB prend en charge plusieurs mode d'exécution en parallèle.
Pour la plupart d'entre vous, il suffira d'exécuter MATLAB dans un environnement parallèle Threads sur un nœud simple.
Voici un exemple inspiré de  la documentation de MathWorks au sujet de parfor.

Sauvegardez le code ci-dessus dans un fichier nommé timeparfor.m. Créez ensuite le script suivant et soumettez-le avec sbatch matlab_parallel.sh
pour exécuter la fonction en parallèle avec quatre cœurs.

Vous pouvez expérimenter en donnant à --cpus-per-task des valeurs plus petites (par exemple 1, 2, 6, 8) pour voir l'effet sur la performance.

= Lancer en simultané plusieurs tâches parallèles =
Si vous utilisez un environnement Cluster parallèle comme
ce qui est décrit ici, le problème suivant pourrait survenir. Quand deux ou plusieurs tâches parallèles initialisent parpool au même moment, chacune des tâches essait de lire et écrire dans le même fichier .dat du répertoire $HOME/.matlab/local_cluster_jobs/R*. Ceci corrompt le profil parallèle local utilisé par les autres tâches. Si ceci se produit, supprimez le répertoire local_cluster_jobs quand aucune tâche n’est en cours d’exécution.

Pour éviter ce problème, nous recommandons que chaque tâche crée son propre profil parallèle dans un endroit unique en spécifiant la propriété de l'objet
parallel.Cluster,
comme démontré ici.

Références :
* FAS Research Computing, MATLAB Parallel Computing Toolbox simultaneous job problem
* MathWorks, Why am I unable to start a local MATLABPOOL from multiple MATLAB sessions that use a shared preference directory using Parallel Computing Toolbox 4.0 (R2008b)?

= Utiliser les bibliothèques Compiler et Runtime =

Important : Comme pour toutes les tâches aux exigences élevées, le code MCR doit toujours être inclus dans une tâche soumise à l'ordonnanceur; consultez Exécuter des tâches.

Vous pouvez aussi compiler votre code avec MATLAB Compiler, un des modules dont nous sommes fournisseurs d'hébergement.
Consultez la documentation MATLAB Compiler.
Pour l'instant, mcc est disponible pour les versions 2014a, 2018a et suivantes.

Pour compiler l'exemple avec cosplot.m ci-dessus, vous utiliseriez la commande

Ceci produit le binaire cosplot et le script enveloppant run_cosplot.sh. Pour exécuter le binaire sur nos serveurs, vous n'avez besoin que du binaire. Le script enveloppant ne fonctionnera pas tel quel sur nos serveurs puisque MATLAB s'attend à ce que certaines bibliothèques se trouvent à des endroits spécifiques. Utilisez plutôt le script enveloppant run_mcr_binary.sh qui définit les bons chemins.

Chargez le module MCR correspondant à la version de MATLAB que vous utilisez pour créer votre exécutable :

Lancez la commande

ensuite, dans le script pour la tâche (et non dans les nœuds de connexion), utilisez le binaire comme suit :
run_mcr_binary.sh cosplot

La commande setrpaths.sh ne doit être exécutée qu'une seule fois pour chacun des binaires compilés; run_mcr_binary.sh vous demandera de l'exécuter si ce n'est pas fait.

= Utilisation de MATLAB Parallel Server =
MATLAB Parallel Server n’est utile que si votre tâche MATLAB parallèle possède plus de processus (appelés workers) que les cœurs CPU disponibles sur un nœud de calcul unique. L’installation régulière de MATLAB décrite ci-dessus permet d’exécuter des tâches parallèles avec un nœud (jusqu’à 64 workers par tâche selon la grappe et le nœud); pour utiliser plus d’un nœud.

Cette solution permet habituellement de soumettre des tâches MATLAB parallèles à partir de l’interface MATLAB locale de votre ordinateur. Certaines améliorations à la sécurité des nos grappes ont été apportées en mai 2023 et, étant donné que MATLAB utilise un mode SSH qui n'est plus autorisé, il n'est plus possible de soumettre une tâche à partir d'un ordinateur local aussi longtemps que MATLAB n'utilisera pas une nouvelle méthode pour se connecter. Il n'y a présentement aucune solution.

== Module d'extension pour Slurm ==
La procédure suivante ne fonctionne pas en raison de l'extension Slurm qui n'est plus disponible et aussi du problème avec SSH qui est mentionné à la section précédente. Toutefois, nous l'avons conservée pour lorsque la solution sera disponible.
# Installez MATLAB R2022a (ou une version plus récente), incluant le Parallel Computing Toolbox.
# De la page MathWorks Slurm Plugin, téléchargez et exécutez le fichier *.mlpkginstall (bouton Download à la droite de la page, sous l'onglet Overview).
# Entrez vos identifiants MathWorks. Si la configuration ne démarre pas automatiquement, lancez dans MATLAB la commande
#:parallel.cluster.generic.runProfileWizard()
# Entrez les renseignements suivants :
#* Sélectionnez Unix (habituellement la seule option offerte)
#* Shared location: No
#* Cluster host:
#** Pour Narval: narval.alliancecan.ca
#** Pour Rorqual: rorqual.alliancecan.ca
#* Username (optional):  (entrez votre nom d’utilisateur; au besoin, le fichier d’identité peut être défini plus tard)
#* Remote job storage: /scratch
#**Cochez Use unique subfolders.
#* Maximum number of workers: 960
#* Matlab installation folder for workers: (les versions locale et distante doivent correspondre)
#** Pour R2022a : /cvmfs/restricted.computecanada.ca/easybuild/software/2020/Core/matlab/2022a
#* License type: Network license manager
#* Profile Name: narval ou rorqual
# Cliquez sur Create et Finish pour compléter le profil.

== Modifier l'extension après son installation ==
Dans le terminal MATLAB, allez au répertoire nonshared en lançant la commande
 cd(fullfile(matlabshared.supportpkg.getSupportPackageRoot, 'parallel', 'slurm', 'nonshared'))

# Ouvrez le fichier independentSubmitFcn.m; aux environs de la ligne 117, remplacez  additionalSubmitArgs = sprintf('--ntasks=1 --cpus-per-task=%d', cluster.NumThreads);  par  additionalSubmitArgs = ccSBATCH().getSubmitArgs();
# Ouvrez le fichier communicatingSubmitFcn.m; aux environs de la ligne 126, remplacez  additionalSubmitArgs = sprintf('--ntasks=%d --cpus-per-task=%d', environmentProperties.NumberOfTasks, cluster.NumThreads);  par  additionalSubmitArgs = ccSBATCH().getSubmitArgs();
# Ouvrez le fichier communicatingJobWrapper.sh; aux environs de la ligne 20 (après la déclaration du copyright), ajoutez la commande suivante et ajustez la version du module en fonction de la version de votre Matlab local: module load matlab/2022a

Redémarrez MATLAB et retournez à votre répertoire /home avec
 cd(getenv('HOME'))  # ou sous Windows, cd(getenv('HOMEPATH'))

== Validation ==
N'utilisez pas l'outil de validation Cluster Profile Manager, mais exécutez l'exemple TestParfor avec un fichier de script ccSBATCH.m adéquatement configuré.
# Téléchargez et extrayez des exemples de code à partir de https://github.com/ComputeCanada/matlab-parallel-server-samples.
# Dans MATLAB, ouvrez le répertoire TestParfor  que vous venez d'extraire.
# Suivez les directives données dans le fichier https://github.com/ComputeCanada/matlab-parallel-server-samples/blob/master/README.md.

Note : Quand ccSBATCH.m se trouve dans votre répertoire courant, vous pouvez utiliser l’outil de validation Cluster Profile Manager pour les deux premiers tests car les autres ne sont pas encore pris en charge.

= Ressources externes =

Voyez aussi les ressources offertes par MathWorks.
* Documentation : https://www.mathworks.com/help/matlab/ (certaines pages sont en français)
* Auto-apprentissage : https://matlabacademy.mathworks.com/ (aussi en versions EN, JP, ES, KR, CN)

Certaines universités ont leur propre documentation, comme
* pour des exemples de scripts : https://rcs.ucalgary.ca/MATLAB