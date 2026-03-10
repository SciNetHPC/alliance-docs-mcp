---
title: "Transferring data/fr"
url: "https://docs.alliancecan.ca/wiki/Transferring_data/fr"
category: "General"
last_modified: "2025-09-02T20:17:46Z"
page_id: 2146
display_title: "Transfert de données"
language: "fr"
---

Pour transférer des données à partir ou en direction des grappes, veuillez utiliser les nœuds de copie plutôt que les nœuds de connexion. Pour connaître l'adresse URL d'un nœud de copie, consultez le tableau au haut des pages pour chacune des grappes (cliquez sur le nom de la grappe dans la barre de gauche de la fenêtre principale).

Globus utilise automatiquement les nœuds de copie.

==Entre un ordinateur personnel et nos équipements==
Pour télécharger ou téléverser des fichiers entre votre ordinateur et notre infrastructure, vous devez utiliser un logiciel offrant une fonctionnalité de transfert sécuritaire.
*Dans un environnement ligne de commande sous Linux ou Mac OS X, utilisez les commandes scp et sftp.
*Sous Windows, MobaXterm offre des fonctions de transfert de fichiers et une interface ligne de commande via  SSH; un autre programme gratuit pour le transfert de données est WinSCP. Pour configurer une connexion via clés SSH avec WinSCP, voyez ces directives.
Les commandes pscp et psftp de PuTTY fonctionnent sensiblement comme les commandes sous Linux et Mac.

S'il faut plus d'une minute pour transférer des fichiers entre votre ordinateur et nos serveurs, nous vous suggérons d'installer Globus Connect Personal et d'en faire l'essai; consultez la section Ordinateurs personnels. Le transfert avec Globus peut être configuré et fonctionner en arrière-plan, sans intervention.

==Entre nos systèmes ==
Globus est l'outil privilégié et devrait être utilisé autant que possible.

D'autres outils de transfert connus peuvent être utilisés pour des transferts entre nos équipements et entre un autre ordinateur et nos équipements, soit
* SFTP
* SCP ou Secure Copy Protocol
* rsync

Remarque : Pour transférer des fichiers entre une autre grappe et Trilloium, utilisez l'indicateur SSH -A en vous connectant à l'autre grappe. Par exemple, pour copier des fichiers de Fir à Trillium, la commande serait

ssh -A USERNAME@fir.alliancecan.ca

ensuite, effectuez la copie

[USERNAME@fir2 ~]$ scp file USERNAME@trillium.alliancecan.ca:

==À partir du web==
Pour transférer des données à partir d'un site web, utilisez wget. Un autre outil bien connu est curl. Les deux outils sont comparés dans cet article de StackExchange ou sur  le site DraculaServers. Même si notre sujet ici est le transfert entre les systèmes Linux de l'Alliance, nous voulons souligner ce tutoriel qui discute aussi de Mac et Windows. Les téléchargements interrompus peuvent être repris avec wget et curl en les relançant de nouveau en ligne de commande avec  -c et -C - respectivement. Pour obtenir des données de services infonuagiques comme Google cloud, Google Drive et Google Photos, utilisez plutôt rclone.  Par défaut, nos grappes offrent wget, curl et rclone sans avoir à charger un  module. Pour les options en ligne de commande, voir la documentation officielle ou lancez l'outil avec les commandes --help ou -h.

==Synchroniser les données==
La synchronisation de données a pour but de faire correspondre le contenu de deux sources de données situées à différents endroits. Il y a plusieurs façons de procéder; les plus courantes sont décrites ici.

===Transfert avec Globus===
Globus est un outil performant et fiable.

Lors d'un transfert avec Globus, les données provenant de la source écrasent habituellement les données dans la destination; toutes les données de la source sont donc transférées. Dans certains cas, les fichiers existent déjà à la destination; s'ils sont identiques à ceux de la source, il n'est pas nécessaire de les transférer. Sous Transfer Settings, le paramètre sync détermine comment Globus choisit les fichiers à transférer.

Les options de sélection des fichiers sont :

checksum is different            	examine les sommes de contrôle (checksum) pour détecter un changement ou une erreur de contenu dans des fichiers de même taille. Cette option ralentit considérablement le transfert, mais offre la plus grande précision.
file doesn't exist on destination	transfère uniquement les fichiers créés depuis la dernière synchronisation. Cette option est utile si vos fichiers sont créés par incréments.
file size is different           	transfère les fichiers dont la taille a été modifiée, assumant que le contenu aussi a été modifié. Cette option permet un test rapide.
modification time is newer       	transfère uniquement les fichiers dont l'estampille temporelle (timestamp) de la source est postérieure à celle de la destination. Avec cette option, cochez preserve source file modification times.

Pour plus d'information, consultez la page Globus.

===rsync===
L'utilitaire rsync vérifie la similitude entre deux jeux de données; il nécessite toutefois un temps considérable lorsqu'il y a un grand nombre de fichiers, que les sites sont à grande distance l'un de l'autre, ou qu'ils se trouvent sur des réseaux différents.

rsync compare les dates de modification et la taille des fichiers et fait le transfert uniquement si l'un des paramètres ne concorde pas.
Si les dates de modification sont susceptibles de différer, l'option -c analyse les checksums à la source et à la destination et transfère uniquement les fichiers dont les valeurs ne concordent pas.

Quand vous transférez des données vers les systèmes de fichiers /project, n'utilisez pas les indicateurs -p et -g. Les quotas pour /project sont calculés selon la propriété de groupe et le fait de conserver la même propriété pourrait produire le message d'erreur  Disk quota exceeded. Puisque -a inclut par défaut à la fois -p et -g, il faut ajouter les options --no-g --no-p comme suit

où LOCALNAME est un répertoire ou un fichier précédés par leur chemin et où somedir sera créé s'il n'existe pas déjà.  L'option -z compresse les fichiers (dont les suffixes ne sont pas dans la liste  pour l'option --skip-compress) et exige des ressources CPU additionnelles, alors que l'option -h permet de simplifier les chiffres qui représentent la taille des fichiers.  Si vous transférez de très gros fichiers, ajoutez l'option --partial pour que les transferts interrompus soient redémarrés.

L'option --progress affiche la progression du transfert de chaque fichier. Pour le transfert de plusieurs petits fichiers, il est préférable d'afficher la progression du transfert de l'ensemble des fichiers.
progress2 LOCALNAME someuser@nibi.alliancecan.ca:projects/def-professor/someuser/somedir/}}
Les exemples ci-dessus sont tous des transferts à partir d'un système local à destination d'un système à distance. Les transferts à partir d'un système à distance à destination du répertoire /project d'un système local fonctionnent de la même manière, par exemple

où REMOTENAME est un répertoire ou un fichier précédés par leur chemin et où somedir sera créé s'il n'existe pas déjà.
Plus simplement, pour transférer localement un répertoire ou un fichier (à partir de /home ou /scratch) à destination de /project dans le même système, n'indiquez pas le nom de la grappe.

où somedir sera créé s'il n'existe pas déjà, avant d'y copier le contenu de LOCALNAME.
En comparaison, la commande de copie peut aussi être utilisée pour transférer  LOCALNAME de /home à /project comme suit
"mode,timestamps" LOCALNAME ~/projects/def-professor/someuser/somedir/}}
Cependant, contrairement à ce qui se produit avec rsync, si LOCALNAME est un répertoire, il sera renommé somedir si somedir n'existe pas déjà.

===Comparaison des sommes de contrôle (checksums)===
Si vous ne pouvez p-as utiliser Globus pour synchroniser deux systèmes et si rsync est trop lent, les deux systèmes peuvent être comparés avec un utilitaire checksum. L'exemple suivant utilise sha1sum.

 xargs -0 sha1sum  tee checksum-result.log
}}

Cette commande crée dans le répertoire courant un nouveau fichier nommé checksum-result.log contenant toutes les sommes de contrôle des fichiers situés dans /home/username/; les sommes sont affichées pendant que le processus se déroule.
Lorsqu'il y a un grand nombre de fichiers ou dans le cas de fichiers de très grande taille, rsync peut travailler en arrière-plan en mode screen, tmux ou tout autre moyen lui permettant d'opérer malgré un bris de la connexion SSH.

Une fois l'opération terminée, l'utilitaire diff trouvera les fichiers qui ne concordent pas.

Il est possible que la commande find emprunte un ordre différent, détectant ainsi de fausses différences; pour contrer ceci, lancez la commande sort sur les deux fichiers avant de lancer diff, comme suit :

==SFTP==
Pour transférer des fichiers, SFTP (pour Secure File Transfer Protocol) utilise le protocole SSH qui chiffre les données transférées.

Dans l'exemple suivant, l'utilisateur USERNAME transfère des fichiers à distance vers ADDRESS.

[name@server]$ sftp USERNAME@ADDRESS
The authenticity of host 'ADDRESS (###.###.###.##)' can't be established.
RSA key fingerprint is ##:##:##:##:##:##:##:##:##:##:##:##:##:##:##:##.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'ADDRESS,###.###.###.##' (RSA) to the list of known hosts.
USERNAME@ADDRESS's password:
Connected to ADDRESS.
sftp>

L'authentification avec l'option  -i peut se faire en utilisant une clé SSH.

[name@server]$ sftp -i /home/name/.ssh/id_rsa USERNAME@ADDRESS
Connected to ADDRESS.
sftp>

À l'invite sftp>, vous entrez les commandes de transfert; utilisez la commande help pour obtenir la liste des commandes disponibles.

Des applications graphiques sont aussi disponibles :
*WinSCP et MobaXterm sous Windows,
*filezilla sous Windows, Mac et Linux,
*cyberduck, sous Mac et Windows.

==SCP==

SCP est l'abréviation de secure copy protocol. Comme SFTP, SCP utilise le protocole SSH pour chiffrer les données qui sont transférées. Contrairement à Globus ou rsync, SCP ne gère pas la synchronisation. Les cas d'utilisation de SCP suivants sont parmi les plus fréquents :

Cette commande transfère le fichier foo.txt qui se trouve dans le répertoire courant de mon ordinateur vers le répertoire $HOME/work de la grappe Rorqual. Pour transférer le fichier output.dat qui se trouve dans mon espace /project de la grappe Fir vers mon ordinateur local, je pourrais utiliser une commande comme

Voyez d'autres exemples. Prenez note que vous lancez toujours la commande scp à partir de votre ordinateur et non à partir de la grappe : la connexion SCP doit toujours être initiée à partir de votre ordinateur, peu importe la direction dans laquelle vous transférez les données.

L'option -r permet de faire un transfert récursif d'un groupe de répertoires et fichiers. Il n'est pas recommandé d'utiliser scp -r pour transférer des données vers /project parce que le bit setGID est désactivé dans les répertoires qui sont créés, ce qui peut générer des erreurs semblables à Disk quota exceeded lors de la création ultérieure de fichiers; voyez  Message Disk quota exceeded.

***Attention :*** si vous utilisez un nom de clé SSH personnalisé, c'est-à-dire autre chose que les noms par défaut id_dsa, id_ecdsa, id_ed25519 et id_rsa, vous devez utiliser l'option scp -i, suivie du chemin vers votre clé privée ainsi

==Mesures préventives et dépannage==

===Problème de lecture===

Assurez-vous que vous pouvez lire tout le contenu des répertoires avant de les transférer. Sous Linux, la commande suivante liste tous les éléments que vous n'avez pas en lecture.

===Problème d'écriture de nouvelles données===

* Vérifiez encore l'utilisation du stockage pour vous assurer qu'assez d'espace et assez de fichiers sont disponibles.
** Sur certaines grappes, le système de fichiers compresse les données automatiquement et indique l'espace disque utilisé par les données compressées. Sur d'autres grappes, l'espace disque utilisé représente la taille apparente des fichiers. Ceci explique pourquoi 1To de données compressées sur une grappe devient 2To de données sur une autre grappe.
** Avant de transférer un ensemble de données, vous pouvez connaître sa taille apparente avec l'option -b de la commande du.

* Vérifiez encore les permissions des systèmes de fichiers pour vous assurer que vous avez la permission d'écriture à l'endroit où vous transférez les nouveaux fichiers.