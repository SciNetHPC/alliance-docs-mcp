---
title: "Globus/fr"
url: "https://docs.alliancecan.ca/wiki/Globus/fr"
category: "General"
last_modified: "2026-01-30T08:59:26Z"
page_id: 404
display_title: "Globus"
language: "fr"
---

Globus est un service qui permet le transfert de fichiers de façon rapide, fiable et sécuritaire. Conçue expressément pour les besoins de la recherche, l'interface graphique de Globus comporte des fonctions de suivi en arrière-plan qui automatisent la gestion des transferts de fichiers entre deux supports, qu'il s'agisse de nos grappes ou d'un autre site, d'une grappe localisée sur un campus, d'un serveur de laboratoire, d'un microordinateur ou d'un ordinateur portatif.

Globus utilise le protocole de transfert GridFTP, mais vous permet d'éviter les tâches complexes et laborieuses qui s’y associent ainsi que d’autres aspects liés au déplacement des données. Le service améliore la performance des protocoles GridFTP, rsync, scp et sftp par le réglage automatique des paramètres de transfert, le redémarrage automatique lorsqu’il y a interruption du transfert et la vérification de l’intégrité des fichiers.

Vous pouvez accéder au service par le site web de Globus ou par notre portail Globus sur  https://globus.alliancecan.ca/.

== Utilisation ==
Rendez-vous sur le portail Globus de l'Alliance. Sélectionnez Alliance de recherche numérique du Canada ou Digital Research Alliance of Canada (et non Digital Research Alliance of Canada - Staff) dans la liste déroulante et cliquez sur Continuer.  Entrez les informations d'identification pour votre compte CCDB.  Ceci vous conduit au portail web de Globus.

=== Lancer un transfert ===

Les transferts de données se font entre collections (points de chute dans les versions précédentes). Des collections sont déjà définies pour la plupart de nos systèmes. Pour transférer des fichiers en provenance ou à destination de votre ordinateur, vous devez créer une collection. Une fois que cette étape quelque peu exigeante est accomplie, il ne restera qu'à vous assurer que l'application Globus Connect Personal est en opération sur votre ordinateur pour effectuer un transfert. Voir la section Ordinateurs personnels ci-dessous.

Si la page File Manager du portail Globus n'est pas affichée (voir l'image), sélectionnez-la à partir de la barre de gauche.

Trois boutons Panels se trouvent à droite dans le haut de la page; pour voir ensemble deux collections, cliquez sur le deuxième bouton.

Pour trouver une collection, cliquez sur Search et entrez le nom de la collection.

Pour sélectionner une collection, vous pouvez commencer à entrer son nom. Par exemple, pour transférer des données en provenance ou à destination de Rorqual, entrez rorqual, attendez deux secondes et sélectionnez

* que le partage par Globus doit être activé,
* le nom de la grappe,
* le chemin,
* la permission (lecture seule ou lecture-écriture).

Nous vous suggérons d'utiliser un chemin dont le nom indique clairement que les fichiers pourraient y être partagés, par exemple

/project/my-project-id/Sharing

Une fois le partage activé pour le chemin, vous pourrez créer un nouveau point de chute Globus partagé pour tout sous-répertoire sous ce chemin. Par exemple, vous pourriez créer les sous-répertoires

/project/my-project-id/Sharing/Subdir-01

et

/project/my-project-id/Sharing/Subdir-02

Créez un Share différent pour chacun et partagez-les avec de différents utilisateurs.

==== Étape 2 - Préparation des données à partager ====

Si ce n'est pas déjà fait, les données qui seront partagées doivent être copiées ou déplacées dans le chemin sélectionné. La création d'un lien symbolique ne permettra pas d'accéder aux données.

Autrement, vous aurez l'erreur

:: The backend responded with an error: You do not have permission to create a shared endpoint on the selected path. The administrator of this endpoint has disabled creation of shared endpoints on the selected path.

==== Étape 3 - Configuration de la collection partagée ====

Avec vos identifiants Globus, connectez-vous au portail Globus de l'Alliance. Une fenêtre de transfert sera affichée. Dans le champ endpoint, entrez l'identifiant du point de chute que vous voulez partager (par exemple alliancecan#fir, computecanada#graham-globus, alliancecan#rorqual, alliancecan#trillium_home etc.) et activez le point de chute si on vous le demande.

Sélectionnez un répertoire que vous voulez partager et cliquez sur le bouton Share à la droite de la liste des répertoires.

Cliquez sur le bouton Add a Guest Collection dans le coin supérieur droit.

Entrez un nom qui sera facilement reconnaissable. Vous pouvez aussi indiquer l'endroit à partir duquel se fera le partage avec le bouton Browse.

===Gestion des accès===

Après création d'une collection partagée, vous verrez la liste actuelle des accès autorisés, qui ne contiendra que votre compte. Le partage s’avérant peu utile sans une seconde personne, cliquez sur le bouton Add Permissions -- Share With afin d’ajouter les personnes ou les groupes avec qui vous voulez partager vos données.

Dans le formulaire suivant, le champ Path sert à définir le partage; puisque dans la plupart des cas vous voudrez partager la collection au complet, ce champ contiendra /. Par contre pour partager le sous-répertoire Subdir-01 avec certaines personnes en particulier, entrez /Subdir-01/ ou utilisez le bouton Browse pour le sélectionner.

On vous demandera ensuite d’indiquer si vous voulez procéder au partage en utilisant une adresse courriel, un nom d’utilisateur ou un groupe.
*Si vous choisissez le nom d’utilisateur, une fenêtre vous permettra d’effectuer une recherche par nom propre ou par nom d’utilisateur Globus.
**L’adresse courriel est un bon choix si vous ignorez le nom d’utilisateur employé par la personne concernée sur Globus. Elle vous permettra également de partager les données avec des personnes qui ne possèdent pas de compte Globus, même si elles devront en créer un pour accéder aux fichiers partagés.
**Cette solution est idéale pour ceux qui possèdent déjà un compte Globus, car ces derniers n’auront rien à faire pour participer au partage. Saisissez le nom de la personne ou le nom d’utilisateur Globus (si vous le connaissez), choisissez le nom correspondant dans la liste puis cliquez sur Use Selected.
*Le choix group permet de partager le fichier simultanément avec plusieurs personnes. Il est possible d’effectuer une recherche d’après le nom du groupe ou son Identifiant universel unique UUID. Le nom d’un groupe pouvant être ambigu, assurez-vous que le partage s’effectue bien avec le groupe désiré. On évitera ce problème en employant l’UUID du groupe, indiqué à la page Groups (voir la partie Groupes).

Pour accorder la permission de lecture, cliquez sur la case write pour le groupe ou l'utilisateur. Prenez note qu'il n'est pas possible de retirer l'accès en lecture. Quand le formulaire est complet, cliquez sur le bouton Add Permission. Il est aussi possible d'ajouter ou de supprimer l'accès en écriture  en cliquant dans la case WRITE.

Pour supprimer un utilisateur ou un groupe de la liste de partage, il suffit de cliquer sur le x au bout de la ligne correspondante.

===Suppression d’une collection partagée===

Lorsque vous n’en aurez plus besoin, vous pouvez supprimer la collection partagée.  Pour ce faire,

*Cliquez sur Collections à la gauche de l'écran, cliquez ensuite sur Shareable by You tab et ensuite sur le titre de la collection à supprimer.
*Cliquez sur le bouton Delete Endpoint à la droite de l'écran.
*Confirmez en cliquant sur le bouton rouge.

La collection est maintenant supprimée. Ceci ne supprime pas vos fichiers ni ceux que d'autres pourraient avoir téléversés.

===Sécurité===

Partager des fichiers suppose un certain risque. En autorisant le partage, vous permettez à d’autres de consulter des fichiers que vous étiez seul à contrôler jusque là. Bien que non exhaustive, la liste ci-dessous énumère certains éléments à prendre en considération avant de procéder à un partage.

*Si vous n’en êtes pas le propriétaire, assurez-vous que vous avez le droit de partager les fichiers.
*Assurez-vous que vous ne partagez les fichiers qu’avec les bonnes personnes. Vérifiez si la personne que vous ajoutez à la liste est bien celle que vous pensez; certains noms peuvent se ressembler. Rappelez-vous que les noms d’utilisateur Globus n’ont aucun lien avec ceux de l'Alliance. Nous préconisons la méthode de partage reposant sur l’adresse courriel, à moins que vous ne connaissiez le nom exact du compte.
*Si le partage s’effectue avec un groupe sur lequel vous n’exercez aucun contrôle, assurez-vous que la personne qui dirige le groupe est digne de confiance, car des personnes non autorisées à consulter vos données pourraient s’y ajouter.
*Si vous accordez le droit de modifier les données, conservez une copie de sauvegarde des fichiers importants ailleurs que sur le point de chute partagé, car il se pourrait que des utilisateurs du point de chute partagé suppriment ou modifient les fichiers, ou en fassent tout ce que vous pourriez en faire personnellement.
*Nous recommandons vivement que le partage se limite à un répertoire secondaire et ne s’applique pas au répertoire du plus haut niveau.

== Groupes Globus ==
Les groupes Globus sont un moyen facile de gérer les permissions pour le partage avec plusieurs utilisateurs. Quand vous créez un groupe, vous pouvez l'utiliser à partir de l'interface de partage pour contrôler l'accès des utilisateurs.

===Création d’un groupe===
Cliquez sur le bouton Groups dans la barre de gauche. Cliquez sur le bouton Create New Group dans le coin supérieur droit. Ceci affiche la fenêtre Create New Group.

*Entrez le nom du groupe dans le champ Group Name.
*Entrez la description du groupe dans le champ Group Description.
*Indiquez si le groupe sera visible uniquement aux yeux de ses membres (groupe privé) ou si tous les utilisateurs de Globus pourront le voir.
*Cliquez sur Create Group pour ajouter le groupe.

===Invitations===
Après avoir créé le groupe, vous pouvez y ajouter des utilisateurs en sélectionnant Invite Users puis en ajoutant leur adresse courriel (méthode privilégiée) ou en cherchant leur nom d’utilisateur. Après avoir choisi les utilisateurs qui sont conviés à se joindre au groupe, cliquez sur le bouton Add afin qu’ils reçoivent un message les invitant à se joindre. Lorsqu’ils auront accepté l’invitation, leur nom figurera dans le groupe.

===Permissions===
Cliquez sur un nom d'utilisateur pour modifier son rôle ou son statut. Les rôles confèrent les permissions Admin (toutes les permissions), Manager (modifier les rôles) et Member (aucune permission de gestion). Cliquez sur Save.

==Interface ligne de commande (CLI)==
===Installation===
L'interface ligne de commande Globus est un module Python qui s'installe avec pip. Voici la procédure d'installation sur une de nos grappes ː
# Créez un environnement virtuel pour y installer l'interface (voir Créer et utiliser un environnement virtuel).$ virtualenv $HOME/.globus-cli-virtualenv
# Activez l'environnement virtuel. $ source $HOME/.globus-cli-virtualenv/bin/activate
# Installez l'interface (voir Installer des modules).$ pip install globus-cli
# Désactivez l'environnement virtuel.$ deactivate
# Pour ne pas avoir à charger l'environnement virtuel à chaque utilisation de Globus, modifiez le chemin. >$HOME/.bashrc

===Utilisation===
* Consultez la page Globus Command Line Interface (CLI).
===Scripts===
* Pour des renseignements sur l'API Python, consultez Globus SDK for Python.

== Machines virtuelles dans les nuages Arbutus, Fir et Nibi==
Les points de chute Globus existent pour les grappes (Fir, Nibi, Rorqual, Trillium, etc.), mais pas pour les machines virtuelles infonuagiques. Il nous est impossible de créer un point de chute particulier parce qu'il n'y a pas d'espace de stockage réservé à chaque machine virtuelle.

Si vous avez besoin d'un point de chute pour votre machine virtuelle et que vous n'avez pas d'autre mécanisme de transfert, vous pouvez utiliser Globus Connect Personal ou Globus Connect Server.

=== Globus Connect Personal ===
Globus Connect Personal est plus facile à installer, à gérer et à passer le pare-feu, mais est conçu pour être installé sur les ordinateurs personnels.

* Installation pour Windows

* Installation pour Linux

=== Globus Connect Server ===
Globus Connect Server est conçu pour des environnements en ligne de commande (sans interface graphique) et comporte certaines fonctionnalités que vous n'utiliserez probablement pas, par exemple la possibilité d'ajouter plusieurs serveurs à un point de chute. Quelques ports doivent être ouverts pour permettre les transferts (voir https://docs.globus.org/globus-connect-server/v5/#open-tcp-ports_section).

== Stockage objet sur Arbutus ==

Pour utiliser le stockage objet sur Arbutus, votre projet infonuagique doit avoir une allocation de stockage. La procédure suivante est faite une seule fois.
Vous devez d'abord générer l'identifiant (access ID) et la clé secrète (secret key) avec un client ligne de commande OpenStack.
1. Importez vos identifiants avec source -openrc.sh.
2. Créez la clé d'accès et la clé secrète avec openstack ec2 credentials create.
3. Connectez-vous au portail Globus avec https://www.globus.org/.
4. Dans la fenêtre File Manager, entrez ou sélectionnez Arbutus S3 buckets.

5. Cliquez sur Continue pour consentir à l'accès aux données.
6. Cliquez sur Allow.
7. Cliquez sur Continue. Dans le champ AWS IAM Access Key ID, entrez le code d'accès généré par openstack ec2 credentials create; dans le champ AWS IAM Secret Key, entrez la clé secrète.

8. Cliquez sur Continue pour terminer la configuration.

==Soutien technique et renseignements additionnels==
Pour en apprendre davantage sur comment nous utilisons Globus ou si vous avez besoin de soutien technique pour ce service, écrivez à globus@tech.alliancecan.ca en incluant les renseignements suivants :

* nom
* identifiant CCRI (Compute Canada Role Identifier)
* établissement
* demande ou problème; n'oubliez pas de mentionner les sites de provenance et de destination pour votre transfert