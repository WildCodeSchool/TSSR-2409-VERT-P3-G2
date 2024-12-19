## Documentation Administrateur

### Sommaire
- [Prérequis techniques]()
- [Étapes d'installation et de configuration]()
- [FAQ]()

### Prérequis techniques

**1. Objectifs**

1. DOSSIERS PARTAGES - Mettre en place des dossiers réseaux pour les utilisateurs
	- 1 . Stockage des données sur un volume spécifique
	- 2 . Sécurité de partage des dossiers par groupe AD
	- 3 . Mappage des lecteurs sur les clients (au choix) :
		- 1 . GPO
		- 2 . Script PowerShell/batch
		- 3 . Profil utilisateur AD
	- 4 . Chaque utilisateur a accès à :
		- 1 . Un **dossier individuel** , avec une lettre de mappage réseau **I**, accessible uniquement par cet utilisateur
		- 2 . Un **dossier de service**, avec une lettre de mappage réseau **J**, accessible par tous les utilisateurs d'un même service.
		- 3 . Un **dossier de département**, avec une lettre de mappage **K**, accessible par tous les utilisateurs d'un même département.
2. STOCKAGE AVANCÉ - Mettre en place du RAID 1 sur un serveur
3. STOCKAGE AVANCÉ - Mettre en place LVM sur un serveur Debian
4. SAUVEGARDE - Mettre en place une sauvegarde de données
	- 1. Faire le bon choix des données à sauvegarder (ex.: dossiers partagés des utilisateurs)
	- 2. Emplacement de la sauvegarde sur un disque différents de celui des données d'origine
	- 3. Mettre en place une planification de sauvegarde (script, AT, GPO, logiciel, etc.)
5. MOT DE PASSE ADMINISTRATEUR LOCAL - Mise en place de LAPS
	- 1 . Console de gestion sur un AD (GUI)
	- 2 . Installation sur au moins 1 client (GPO ou script PS)
6. GESTION DES OBJETS AD - Déplacement automatique des ordinateurs dans les bonnes OU
	- 1 . Suivant le nom d'une machine et/ou la valeur d'un attribut AD
	- 2 . Automatisation par script PS exécuté par une tâches AT
7. SÉCURITÉ D'ACCÈS - Restriction d'utilisation
	- 1 . Pour les utilisateurs standard : connexion autorisée de 7h à 18h, du lundi au vendredi sur les clients (bloquée le reste du temps)
	- 2 . Pour les administrateurs : bypass
	- 3 . Gestion des exceptions : prévoir un groupe bypass

**2. Optimisation de l'infrastructure réseau**

L'infrastructure réseau du projet commence à prendre forme :
- Plus de serveurs
- Plus de services
Elle doit être la plus sécurisée possible.

Chaque éléments de l'infrastructure :
- Doit disposer d'une documentation fonctionnelle et à jour :
	- Prérequis matériel/logiciel
	- Version d'OS, de logiciel, ...
	- Installation :
		- Suivi pas-à-pas avec copie d'écran
		- Copie d'écran du résultat attendu
	- Configuration/Utilisation :
		- Cible
		- Suivi pas-à-pas avec copie d'écran
	- FAQ
- Doit être optimisé :
	- Optimisation dans le choix du hardware
	- MAJ système régulière
- Doit pouvoir être restauré rapidement en cas de défaillance :
	- Clone miroir
	- Script de restauration complet ou partiel d'OS
	- Script de restauration de configuration
	- Doc à jour


### Étapes d'installation et de configuration

 **1. DOSSIERS PARTAGES**  
- Installation du serveur de fichier avec Windows Server
- Renomer le serveur "G2-SRV-SHARE"
- Attribution de son adresse IP conformement au plan d'adressage (10.10.100.204/24)
 - Pour configurer le Server de fichier cliquer sur Add roles et Features  
 - Cocher "File and Storage Services" 
 - Cliquer sur suivant jusqu'à installation

**Céer un partage SMB sous Windows Server**  

- Toujours à partir du"Gestionnaire de server" cliquer sur "Service de fichiers et de stockage" puis sur partages  
- Cliquer sur le bouton "Tâches" pour lancer l'assistance
- Dans l'assistance, cliquer sur "Partage SMB-Rapide"
- Si ce n'est pas déjà fait, créez le répertoire correspondant au partage sur votre serveur (ex: C:\Partage)
- À l'étape "Emplacement du partage", nous choisissons "Tapez un chemin personnalisé" nous cliquon sur "Parcourir" puis sélectionner le répertoire "Partage" avant de valider
- Donner un nom à "Nom de partage"
- Activer l'enumération basée sur l'acces puis valider
- Dans les autorisations, nous laissons par defaut (pour le moment) les permissions NTFS du dossier
- Cliquer sur créer


### **Mappage d'un lecteur réseau par GPO**

- 1 - A l'aide de la console GPMC, créez une nouvelle GPO avec le nom défini
- 2 - Modifiez la GPO que l'on vient de créer, et parcourez l'arborescence de cette façon : Configuration utilisateur > Préférences > Paramètres Windows > Mappages de lecteur.
- 3 - Effectuez un clic droit : Nouveau > Lecteur mappé.
- 4 - Saisir les informations utiles à la connexion du lecteur réseau.
- 5 - Action : créer, mettre à jour, remplacer, supprimer 

- Emplacement : il s'agit du chemin UNC vers le partage réseau à connecter sur le poste client
- Reconnecter : rendre le lecteur réseau persistent si l'option est cochée, c'est-à-dire qu'il va se reconnecter à chaque ouverture de session
- Libeller en tant que : donner un nom personnalisé au lecteur réseau connecté
- Lettre de lecteur : lettre à utiliser par ce lecteur, un classique mais attention à un éventuel conflit vis-à-vis des lecteurs locaux
- L'option "Se connecter en tant que" est désactivée par Microsoft pour des raisons de sécurité : elle exposait les credentials dans les fichiers de la GPO, accessible via le dossier SYSVOL, ce qui était dangereux.

- Revenir en détails sur l'option "Action" car elle est très importante et il faut bien la comprendre. Les différentes actions sont :

- 1 - Créer : connecter un nouveau lecteur réseau pour cet utilisateur. Attention, la lettre à utiliser ne doit pas être déjà utilisée sinon il ne pourra pas se connecter
- 2 - Supprimer : déconnecter un lecteur réseau existant. S'il n'existe pas, l'action ne réalisera aucune action
- 3 - Remplacer : le lecteur réseau ciblé va être déconnecté (supprimé) puis reconnecté (créé) pour l'utilisateur
- 4 - Mettre à jour : le lecteur réseau va être connecté s'il ne l'est pas actuellement, et si il est déjà connecté il va être mis à jour pour intégrer les éventuels changements dans sa configuration. Contrairement à l'action "Remplacer", celle-ci ne supprime pas le lecteur réseau pour le recréer. Cette option est la plus souvent utiliser car c'est la plus flexible
 

- Note : en phase de migration, il est intéressant d'utiliser l'action "Remplacer" pour être sur que le lecteur réseau va être supprimé puis recréé vers le nouveau partage. Lorsque la migration est finalisée et la situation stable, il est préférable de se satisfaire de l'option "Mettre à jour".

- Je souhaite réaliser le mappage du partage "\\SRV-ADDS-01.odyssey.loca\Partage$" sur les sessions de mes utilisateurs, de façon persistante, en le nommant "PARTAGE" et utilisant la lettre "P". Ce qui donne :

Après validation, le lecteur réseau va apparaître dans la GPO. Il est possible de configurer plusieurs actions de mappages de lecteurs dans la même GPO.

 - **Tester la GPO**

La GPO étant configurée, il est temps de tester ! Pour cela, il vous suffit de vous connecter sur un poste du domaine avec un compte ciblé par la GPO actuelle et disposant des autorisations nécessaires pour connecter ce partage via un lecteur réseau.

Grâce à la GPO définie précédemment, le lecteur réseau se connecte, avec le bon nom et la lettre associée ??



- **Lecteur réseau et ciblage**

- Il est possible de connecter plusieurs lecteurs réseaux dans la même GPO. Cela est d'autant plus intéressant lorsque l'on associe cette possibilité au fait de pouvoir réaliser un ciblage particulier au niveau de chaque lecteur réseau. Pour finir ce tutoriel, nous allons aborder cette notion, ce qui peut permettre d'avoir une seule GPO pour connecter vos lecteurs réseaux, même si tous les utilisateurs ne doivent pas avoir les mêmes lecteurs.

- Pour rappel, l'accès aux options de ciblage s'effectue au niveau de chaque élément créé, c'est-à-dire pour chaque lecteur réseau déclaré dans la GPO, en cliquant sur l'onglet "Commun". Ensuite, il faut cocher l'option "Ciblage au niveau de l'élément" et cliquer sur le bouton "Ciblage".

- L'éditeur de cible va s'ouvrir, ensuite libre à vous de monter le lecteur réseau en fonction d'un critère spécifique en fonction de vos règles de gestion habituelles : basée sur l'appartenance à un groupe de sécurité ou sur une unité d'organisation, par exemple.



- Grâce à cette méthode nous pouvons centraliser la gestion des lecteurs réseaux dans une GPO unique, même si vos utilisateurs ne doivent pas avoir les mêmes lecteurs réseaux. Pour que le lecteur réseau soit supprimé si l'utilisateur n'est plus membre du groupe de sécurité en question, il faut ajouter un second lecteur réseau avec l'action "Supprimer" et créer un ciblage inverse : l'utilisateur n'est pas membre du groupe de sécurité XXX. C'est le ciblage qui fera la différence.








 
### FAQ :
