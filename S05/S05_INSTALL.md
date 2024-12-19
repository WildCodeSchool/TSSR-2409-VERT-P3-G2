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



 
### FAQ :
