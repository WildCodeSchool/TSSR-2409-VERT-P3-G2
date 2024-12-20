# DHCP Relay

Pour configurer le DHCP relay, accédez à pfSense, puis à **Services** et sélectionnez **DHCP Relay**.

Cochez ensuite l'option **"Enable DHCP relay on interface"**.

Dans la liste des interfaces, choisissez **LAN** par exemple.

Ajoutez l'adresse IPv4 du serveur vers lequel les demandes DHCP doivent être relayées dans le champ **"Destination server"**.

Cliquez sur **Save**. Un message apparaîtra pour indiquer que les modifications ont été appliquées avec succès.

## Documentation Utilisateur

### Sommaire
- [Utilisation de base]()
- [Utilisation avancée]()
- [FAQ]()

### Utilisation de base


DHCP sur Windows Server 2022 et interface graphique
Sommaire
Installation du rôle DHCP

Configuration du rôle DHCP

![Capture DHCP-1](https://github.com/user-attachments/assets/03349c62-0512-42d8-81af-bcf5e84ac8c1)

Installation du rôle DHCP sur Windows Server en GUI

Dans le Gestionnaire de serveur , cliquez sur Gérer > Ajouter des rôles et des fonctionnalités .
DHCP


![Capture DHCP-2](https://github.com/user-attachments/assets/4c16d648-14e0-4b8b-8878-6fa6124255d9)


Dans l'onglet Avant de commencer > Suivant .
DHCP


![Capture DHCP-3](https://github.com/user-attachments/assets/2e6c7227-3bdb-4863-b3ee-c8505e774706)


Dans l'onglet Type d'installation > Installation basée sur les rôles ou les fonctionnalités > Suivant .
DHCP


![Capture DHCP-4](https://github.com/user-attachments/assets/7e8f239a-981d-4c69-9167-542f143681bb)


Dans l'onglet Sélection du serveur > Suivant .
DHCP


![Capture DHCP-5](https://github.com/user-attachments/assets/b26eccd0-fb9d-4132-ac25-330fce88b0f4)


Dans l'onglet Rôles du serveur , cochez Serveur DHCP puis Suivant .
DHCP


![Capture DHCP-6](https://github.com/user-attachments/assets/2f73f160-8e93-45bc-a5bc-a9223d23b7ab)


Cliquez sur Ajouter des fonctionnalités .
DHCP


![Capture DHCP-7](https://github.com/user-attachments/assets/e501ba33-6e77-4419-8301-7e8d6af4a592)


Puis Suivant .
DHCP


![Capture DHCP-9](https://github.com/user-attachments/assets/1e0867e6-217d-4115-8eee-3e36e053322e)


De retour sur le Server Manager , cliquez sur le triangle jaune, puis Complete DHCP Configuration .
DHCP


![Capture DHCP-10](https://github.com/user-attachments/assets/52eebd4b-3646-49c7-8d4b-1f5855dcf4a1)


Votre Rôle DHCP est désormais fonctionnel.




### Utilisation avancée


### FAQ :
