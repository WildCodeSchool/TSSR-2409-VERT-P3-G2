### Sommaire
- [Prérequis techniques]()
- [Étapes d'installation et de configuration]()
- [FAQ]()

**1. Objectifs**

1. AD-DS - Création d'un domaine AD
-	1 . Un serveur Windows Server 2022 GUI avec les rôles AD-DS, DHCP, DNS
-	2 . Un serveur Windows Server 2022 Core avec le rôle AD-DS
-	3 . Les 2 serveurs sont des DC du domaine et ont une réplication complète gérée
2. Gestion de l'arborescence AD
-	1 . Création des OU
-	2 . Création des groupes
-	3 . Création des comptes
3. Gestion de l'arborescence AD  entièrement automatisée à partir du fichier CSV
-	1 . Création des groupes
-	2 . Création des comptes
4. Création d'une VM Serveur Linux Debian mise sur le domaine AD accessible en SSH
5. Création d'une VM client
-	1 . Sur le domaine AD
-	2 . Avec un compte utilisateur ayant un accès SSH sur le serveur Linux



**- 2. Réseau (sous Proxmox)**

- Adresse IP de réseau : 10.10.0.0/16
- Adresse de passerelle : 10.10.255.254
- Adresse IP DNS : 10.10.255.254

**Prérequis techniques**

Configuration du rôle AD-DS sur Windows Server en GUI
Pour rappeler notre serveur G2-SRV-DC1 l'IP 10.10.100.201/24. Ce serveur possède les rôles DHCP , DNS et AD-DS .

**G2-SRV-DC1**

Modèle : Windows Server 2022/ Type : VM.
- Configuration IP : 10.10.100.201/24 Passerelle : 10.10.100.254/ Carte réseau : G2switchL3.
- Disque dur : 1 HDD 100GO(Système + Dossiers Partagés) / 1 HDD 100GO(RAID1) / 1 HDD 100Go(Sauvegarde).
- Processeur : 2.
- RAM : 8Go.
- Fonction : DHCP/ DNS/ ADDS.
- Rendez-vous à l'annexe ADDS_Conf_WinServGUI .

Installation et Configuration du rôle AD-DS sur Windows Server en Core
Pour rappeler notre serveur G2-SRV-DC2 l'IP 10.10.100.202/24 sera en version Core (non-graphique) et aura uniquement le rôle AD-DS . Il servira à la réplication des données AD-DS, afin d'avoir une redondance sur le réseau.

**G2-SRV-DC2**

Modèle : Windows Server 2022 Core/ Type : VM.
Configuration IP : 10.10.100.202/24 Passerelle : 10.10.100.254/ Carte réseau : G2switchL3.
Disque dur : 1 HDD 32Go(Système) / 1 HDD 32GO(RAID1).
Processeur : 2.
RAM : 2Go.
Fonction : DNS/ ADDS.

**Étapes d'installation et de configuration**

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





FAQ : Solutions aux problèmes connus et communs liés à l'installation et à la configuration

Configuration du rôle ADDS sur Windows Server en GUI
Dans le Gestionnaire de serveur , cliquez sur Outils puis Utilisateurs et ordinateurs Active Directory .



