### Sommaire
- [Prérequis techniques]()
- [Étapes d'installation et de configuration]()
- [FAQ]()

**Prérequis techniques**

<details>
<summary>Objectifs</summary>
1. Objectifs

1. GPO de sécurité - Création d'au moins 5 GPO dont au moins 3 dans la liste ci-dessous :
	1. Politique de mot de passe (complexité, longueur, etc.)
	2. Verrouillage de compte (blocage de l'accès à la session après quelques erreur de mot de passe)
	3. Restriction d'installation de logiciel pour les utilisateurs non-administrateurs
	4. Gestion de Windows update (heure, délai avant installation, etc.)
	5. Blocage de l'accès à la base de registre
	6. Blocage complet ou partiel au panneau de configuration
	7. Restriction des périphériques amovible
	8. Gestion d'un compte du domaine qui est administrateur local des machines
	9. Gestion du pare-feu
	10. Écran de veille avec mot de passe en sortie
	11. Forçage du type d'utilisation sécurisée du bureau à distance
	12. Limitation des tentatives d'élévation de privilèges
	13. Définition de scripts de démarrage pour les machines et/ou les utilisateurs
	14. Politique de sécurité PowerShell
2. GPO standard - Création d'au moins 5 GPO dont 3 dans la liste ci-dessous :
	1. Fond d'écran
	2. Mappage de lecteurs
	3. Gestion de l'alimentation
	4. Déploiement (publication) de logiciels
	5. Redirection de dossiers (Documents, Bureau, etc.)
	6. Configuration des paramètres du navigateur (Firefox ou Chrome)
3. Serveur de gestion de parc - Installation de Glpi
	1. Sur un serveur Debian (CLI) VM ou CT
	2. Synchronisation AD
	3. Gestion de parc : Inclusion des objets AD (utilisateurs, groupes, ordinateurs)
	4. Gestion des incidents : Mise en place d'un système de ticketing
	5. Accès et gestion à partir d'un client
4. Automatisation par script shell - Installation de Glpi
	1. Sur un serveur Debian (CLI) déjà installé (VM ou VT)
	2. Utilisation d'un fichier de configuration (contient le nom de la base de donnée, le nom du compte, etc.)
5. Automatisation par script PowerShell - Installation d'un AD-DS
	1. Sur un Windows Server Core (CLI) déjà installé
	2. Installation du rôle AD-DS et ajout à un domaine existant
	3. Utilisation d'un fichier de configuration (contient le nom du serveur, l'adresse IP du DNS, le nom du domaine, etc.)

# 2. Documentation

Si un objectifs de GPO est choisi, la configuration de chacune des GPO doit être clairement expliquée.

</details>

**Étapes d'installation et de configuration**
<details>
 <summary>installation des rôles sur le server ADDS DNS DHCP</summary>
Installation du rôle ADDS
Installation du rôle ADDS sur Windows Server en GUI
Dans le Gestionnaire de serveur , cliquez sur Gérer > Ajouter des rôles et des fonctionnalités .

![Capture DHCP-1](https://github.com/user-attachments/assets/54d33fae-e999-4f6e-895f-d556b1f82412)

 Dans le Gestionnaire de serveur , cliquez sur Gérer > Ajouter des rôles et des fonctionnalités .
 
![Capture DHCP-2](https://github.com/user-attachments/assets/f546ae76-d672-4c73-993b-e417f7d47e27)
 
 Dans l'onglet Avant de commencer > Suivant
 
![Capture DHCP-3](https://github.com/user-attachments/assets/1d0b02f2-d793-4d1e-82ff-ab00db368122)

Dans l'onglet Type d'installation > Installation basée sur les rôles ou les fonctionnalités > Suivant
Dans l'onglet Sélection du serveur > Suivant .

![Capture DHCP-4](https://github.com/user-attachments/assets/11f20317-cd08-4b39-97be-03ae18d05d3f)

Dans l'onglet Rôles du serveur , cochez Services de domaine Active Directory , puis Suivant .

![Capture DHCP-5](https://github.com/user-attachments/assets/4232a435-a594-4d72-8a91-2f56b795114a)

Cliquez sur Ajouter des fonctionnalités .

![Capture DHCP-6](https://github.com/user-attachments/assets/b32801e0-f6e8-4563-a56f-fd5fc4dc7076)

Cliquez sur Suivant .

![Capture DHCP-7](https://github.com/user-attachments/assets/2408fc2e-47c0-4429-bdd8-5fd757211e1f)

 Dans l'onglet AD DS > Suivant

![Capture DHCP-8](https://github.com/user-attachments/assets/2736fc0c-477d-4777-b9f3-a86534a3d661)

Dans l'onglet Confirmation > Installer .

![Capture DHCP-9](https://github.com/user-attachments/assets/943e10ee-574b-4f59-b675-2ceab602e8db)

A la fin de l'installation, cliquez sur Fermer .

![Capture DHCP-10](https://github.com/user-attachments/assets/01969b33-6b7c-48df-9bc5-b4855e029204)

Ajout de Forêt

![Capture DHCP-11](https://github.com/user-attachments/assets/f69653f2-b3cd-41a0-93b5-e853caa8a662)

Configuration du domaine avec mot de passe

![Capture DHCP-12](https://github.com/user-attachments/assets/0f88116a-41e6-4133-a83d-fc5a1e45e38d)
</details>

<details>
	<summary>Integration du server GLPI dans le domaine</summary>
	- ggiyg
	- coucou
</details>


**FAQ** 





