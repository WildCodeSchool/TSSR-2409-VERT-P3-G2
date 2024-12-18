## Documentation Administrateur

### Sommaire
- [Prérequis techniques]()
- [Étapes d'installation et de configuration]()
- [FAQ]()

### Prérequis techniques

**1. AD-DS - Création d'un domaine AD**
	
- 1 . Un serveur Windows Server 2022 GUI avec les rôles AD-DS, DHCP, DNS
- 2 . Un serveur Windows Server 2022 Core avec le rôle AD-DS
- 3 . Les 2 serveurs sont des DC du domaine et ont une réplication complète gérée

**2. Gestion de l'arborescence AD**
	
- 1 . Création des OU
- 2 . Création des groupes
- 3 . Création des comptes

**3. Gestion de l'arborescence AD  entièrement automatisée à partir du fichier CSV**
	
- 1 . Création des groupes
- 2 . Création des comptes

**4. Création d'une VM Serveur Linux Debian mise sur le domaine AD accessible en SSH**

**5. Création d'une VM client**

- 1 . Sur le domaine AD
- 2 . Avec un compte utilisateur ayant un accès SSH sur le serveur Linux  

### Étapes d'installation et de configuration

**Création des groupes dans l'AD**

- Ouvrir le serveur ADDS préalablement installé et configuré
- Une fois le serveur mis en service 
- Aller dans "tool" en haut à droite puis "Active Directory Users and Computeurs"  
- Sélectionner l'OU correspondand puis faire un click droit  
- Ensuite, faite "New" puis "Group"  
- Entrez le nom du goupe en respectant bien la typographie et selectionner dans le Groupe scope "Global" et dans Groupe type "Sécurity" par défaut
- Cliquez sur "OK"
- Comme vous pouvez le constater, le groupe a été crée.



### FAQ :
