## Documentation Administrateur

### Sommaire
- [Prérequis techniques]()
- [Étapes d'installation et de configuration]()
- [FAQ]()

### Prérequis techniques

### Fournir un plan d'adressage réseau complet cohérent :

    - Configuration IP de LAN/VLAN
    - Configuration IP des matériels réseaux
    - Fournir un plan schématique du futur réseau :
    - Nom des matériels
    - Configuration IP
    - Faire la liste des serveurs/matériels nécessaires à l'élaboration de la future infrastructure réseau
    
- Mettre en place une nomenclature de nom :
    
    - Serveurs
    - Ordinateurs clients
    - Utilisateurs
    -Groupes

- Création de VM serveur / client
    
    - Configuration 
    - OS 
    - Fonction / rôle 
    
- Création d'un domaine
    
    - Nom du domaine AD
    - Configuration

### Configuration

- Création des OU correspondant aux différents service de la société
    
    - OU de départements
    - OU de services
    - Création des groupes correspondant aux différents groupes d'utilisateurs de la société
    - Groupes de départements
    - Groupes de services
    - Groupes fonctionnels
    - Groupes de sécurité
    - Intégration des données dans l'AD :
    - Utilisateurs
    - Ordinateurs



### Étapes d'installation et de configuration

Installation et Configuration des équipements et ressources
Configuration du rôle AD-DS sur Windows Server en GUI
Pour rappeler notre serveur G2-SRV-DC1 l'IP 10.10.100.201/24. Ce serveur possède les rôles DHCP , DNS et AD-DS .

- G2-SRV-DC1

Modèle : Windows Server 2022/ Type : VM.
Configuration IP : 10.10.100.201/24/ Passerelle : 10.10.100.254/ Carte réseau : G2SwitchL3.
Disque dur : 1 HDD 32 Go 
Processeur : 2.
RAM : 4 Go.
Fonction : DHCP/ DNS/ ADDS.


Installation et Configuration du rôle AD-DS sur Windows Server en Core
Pour rappeler notre serveur G2-SRV-DC2 l'IP 10.10.100.202/24 sera en version Core (non-graphique) et aura uniquement le rôle AD-DS . Il servira à la réplication des données AD-DS, afin d'avoir une redondance sur le réseau.

- G2-SRV-DC2

Modèle : Windows Server 2022 Core/ Type : VM.
Configuration IP : 10.10.100.202/24 / Passerelle : 10.10.100.254/ Carte réseau : G2SwitchL3
Disque dur : 1 HDD 32 Go 
Processeur : 2.
RAM : 2 Go.
Fonction : DNS/ ADDS.





### FAQ :
