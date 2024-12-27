## Documentation Utilisateur

### Sommaire
- [Utilisation de base]()
- [Utilisation avancée]()
- [FAQ]()

### Utilisation de base


1. INSTALLER PFSENSE SUR PROXMOX 

- a. télécharger l'image ISO de pfSENSE
- b. monter l'ISO dans l'hyperviseur Proxmox
- c. Création d'un VMBR dans l'hyperviseur Proxmox
- d. Création de la machine virtuelle pfSENSE
- e. Lancement et configuration de l'installation de pfSENSE
- f. Assignation de l'IP statique sur l'interface WAN
- g. Connexion à la console d'administration de pfSENSE


2 – INSTALLATION DE pfSENSE SUR L'HYPERVISEUR PROXMOX 

• 1 machine virtuelle pfSENSE (avec 2 interfaces "WAN" et "LAN")
-  1 machine Windows 10 de test qui sera connectée à l'interface "LAN" de pfSENSE
-  1 conteneur LXC Debian (qui abritera un serveur web) qui sera connecté à l'interface "LAN" de pfSENSE
-  Nous possédons une adresse IP Publique (dite "IP Failover" qui nous a été fournie par notre hébergeur)
-  L'adresse IP Failover fournie par notre hébergeur possède une adresse MAC virtuelle (fournie également)
-  Nous possédons une adresse de passerelle commune qui nous a été fournie par notre hébergeur
-  Nous avons un sous-domaine hébergé chez OVH pour nos tests (non obligatoire)

**1ère étape : téléchargement de l'image ISO de pfSENSE**

• Récupérez l'image ISO depfSENSE depuis le site officiel : Download pfSense Community Edition
• Sélectionnez l'architecture "AMD64" et l'installer "DVD Image (ISO)"
• Cliquez le bouton "Download" 

Attention, il faudra décompresser cette image car elle sera téléchargée au format "iso.gz". Pour la décompresser
et l'avoir au format ".iso", installer un utilitaire tel que "7zip" par exemple.
L'image pfSENSE doit être téléchargée avec "l'installer" DVD Image (ISO)
Installer. Attention, il s'agit d'un fichier au format ".iso.gz" qu'il faudra décompressé ensuite avec un utilitaire tels que "nanazip" ou "7zip"

 **2 ème étape : monter l'ISO pfSENSE sur l'hyperviseur Proxmox**

• Connectez-vous à l'interface de votre hyperviseur Proxmox
• Dans la vue serveur, cliquez le stockage "Local" et "Images ISO" dans le volet de droite
• Cliquez le bouton "Téléverser" pour transférer l'image depuis votre PC vers votre serveur Proxmox

Note : vous pouvez également téléverser l'image ISO de pfSENSE depuis une adresse URL valide si vousen avez une avec le fichier pfSENSE au format ".iso"

**3 ème étape : création d'un "vmbr" sur l'hyperviseur Proxmox**

Avant de créer la machine virtuelle, il faut créer des "vmbr" sur lesquels nous connecterons notre pfSENSE

• Cliquez sur le nom du nœud Proxmox et dans le volet de droite, cliquez sur "Réseau"
• Cliquez sur "Créer" et "Linux bridge" 

Logiquement, un "vmbr1" est créé et apparaît dans la liste des réseaux Proxmox :
Pensez à cliquez sur "Appliquer la configuration" pour rendre votre "vmbr" actif !
Pour l'installation de pfSENSE, nous aurons besoin de 2 cartes réseau virtuelles qui seront connectées à leur
"vmbr" respectifs (ici "vmbr0" et "vmbr1").
Ajoutez un nouveau "vmbr" à votre hyperviseur Proxmox avant de créer votre machine virtuelle.

**4 ème étape : création de la machine virtuelle pfSENSE**

• Cliquez le bouton "Créer une VM"
• Donnez un nom à votre VM et choisissez, dans l'étape suivante, l'image ISO d'installation
• Laissez les paramètres de l'onglet "Système" par défaut
• Dans l'onglet "Disques", affectez une taille de disque de 20 Go
• Laissez les paramètres de l'onglet "Processeur" par défaut
• Indiquez "1024" comme taille mémoire
• Sélectionnez, dans l'onglet "Réseau", le "vmbr0"

Attention, à ce stade, il convient d'indiquer, au niveau de l'adresse MAC, l'adresse MAC qui vous a été fournie par votre hébergeur et qui correspond à l'adresse MAC de votre adresse IP Failover !

• Confirmez la création de la machine mais ne la lancez pas !
• Ajoutez une 2ème carte réseau à votre machine virtuelle pfSENSE :
o cliquez sur votre machine virtuelle et, dans le volet de droite, cliquez sur "Matériel" et "Ajouter"
o cliquez sur "Carte réseau"
o sélectionnez le "vmbr1" pour cette carte (laissez l'adresse MAC en mode "auto"')
o cliquez "Ajouter" de manière à obtenir ceci :
La configuration du matériel réseau de votre machine virtuelle pfSENSE doit ressembler à ceci :
Vérifiez bien que votre "vmbr0" possède bien l'adresse MAC fournie par votre hébergeur et que l'autre carte est bien connectée au "vmbr1" !

La machine virtuelle pfSENSES est prête à être lancée.

**5 ème étape : lancement de l'installation de pfSENSE en mode console**

• Faites démarrer votre machine virtuelle pour lancer l'installeur, patientez et validez le contrat de licence en pressant la touche "Entrée" 
• Sélectionnez, ici, "Auto (UFS)" (nous n'avons qu'un seul disque pour pfSENSE) et faites "Entrée" 
• Faites "Entrée" sur le choix "Entire Disk" 
• Sélectionnez "MBR" et faites "Entrée" 
• Le disque de 20 Go initialement créé s'affiche ; pressez la touche "Entrée" pour valider la préparation du disque pfSENSE 
• Pressez la touche "Entrée" pour valider le lancement du partitionnement du disque 

Patientez pendant que l'archive soit extraite et que l'installation s'exécute 

• Pressez la touche "Entrée" pour que la machine pfSENSE redémarre 

Patientez le temps que la machine redémarre.
Au redémarrage de pfSENSE, les interfaces réseau sont détectées et pfSENSE va vous demander d'assigner ces interfaces en tant que "WAN" et "LAN".
Ici, pfSENSE a détecté nos 2 cartes réseau sous la forme "vtnet0" et "vtnet1". La carte "vtnet0" correspond à la carte pour laquelle on avait saisi l'adresse MAC de notre IP Failover (l'interface "WAN") 
Les 2 cartes réseau de notre machine pfSENSE ont été détectées et sont passées au statut "UP"

Nous devons assigner les cartes réseau détectées aux interfaces "WAN" et "LAN" (important !) 

• Indiquez "vtnet0" et faites "Entrée" pour assigner cette carte en tant qu'interface "WAN"
• Indiquez "vtnet1" pour l'autre carte pour l'assigner en tant qu'interface "LAN" 
• Saisissez "y" pour valider l'assignation des cartes et patientez pendant que pfSENSE assigne les interfaces aux cartes réseau spécifiées (cela peut prendre quelques minutes) :

**6 ème étape : assignation de l'IP Failover à l'interface "WAN"**

Une fois l'assignation des interfaces réseau effectuée, pfSENSE affiche l'écran suivant :
On peut remarquer qu'aucune adresse IP n'a été affectée à notre interface "WAN" car cette dernière doit être configurée de manière "statique" (il s'agit de l'IP Failover donnée par notre hébergeur sous la forme (xxx.xxx.xxx.xxx/32).

En revanche, l'interface "LAN" a bien reçue une adresse de type 192.168.1.1/24 (adresse par défaut allouée parpfSENSE lors de l'installation).

Il est donc nécessaire, ici, d'affecter une adresse IP statique à notre interface "WAN" depuis la console de pf SENSE 

• Saisissez "2" ("Set interface(s) IP address") et "1" pour sélectionner l'interface "WAN" 
• Répondez "n" à la question "Faut-il configurer DHCP sur WAN" (notre IP Wan étant statique) 
• Saisissez ici l'adresse IP Failover que votre hébergeur vous a fourni 
• Saisissez le masque de sous-réseau de votre IP Failover (/24) 
• Saisissez l'IP de la passerelle qui vous a été fournie par votre hébergeur 
• Répondez "n" à la question ("Voulez-vous configurer une adresse WAN IPv6) 

• Pressez la touche "Entrée" ici sans rien saisir (pas d'adresse WAN IPv6) 
• Répondez "n" ici car nous ne voulons pas obtenir l'adresse WAN via DHCP (WAN statique) 
• Ici nous répondons "n" afin de conserver un accès en "https" à la console de pfSENSE 
• Pressez la touche "Entrée" pour valider vos choix : Nos deux interfaces sont maintenant configurées 

**7 ème étape : connexion à l'interface web de pfSENSE, depuis une machine virtuelle connectée au "vmbr1"(interface LAN)**

• Créez une machine virtuelle avec une interface graphique (par exemple une machine Windows)
• Connectez cette machine au "vmbr1" afin qu'elle soit sur le réseau "LAN" et lancez-la

Ici, nous avons créé une machine Windows 10 qui possède une carte réseau connectée au "vmbr1" 
Il est important de bien connecter la carte réseau de votre machine Windows au "vmbr1" ici sinon la
machine n'accédera pas au réseau "LAN" de pfSENSE !
Les 2 cartes réseau de notre machine pfSENSE sont maintenant configurées.

Une fois la machine lancée, on constate qu'elle fait bien partie du réseau "LAN" (elle a reçu une adresse IP de type 192.168.1.xxx/24 du serveur DHCP de pfSENSE et elle est connectée à l'Internet) 

• Ouvrez le navigateur de la machine virtuelle et saisissez l'URL https://192.168.1.1 qui correspond à l'adresse LAN de pfSENSE et ajoutez l'exception dans votre navigateur pour valider le certificat autosigné envoyé par pfSENSE :
• Authentifiez-vous avec les identifiants par défaut "admin" (login) et "pfsense" (mot de passe) 
La machine virtuelle Windows, connectée au "vmbr1" a bien reçu une adresse IP locale de type 192.168.1.100/24 via le service DHCP de pfSENSE.
Pour vous connecter à l'interface de pfSENSE, vous devez accepter dans votre navigateur le certificat autosigné envoyé par pfSENSE.
Par défaut, l'authentification à la console d'administration de pfSENSE s'effectue avec le login "admin" et le mot de passe "pfsense". Ces identifiants connus de tous seront changés dans l'étape qui suit.

L'assistant de démarrage de pfSENSE s'ouvre automatiquement ; cliquez le bouton "Next" 
L'assistant propose 9 étapes de finalisation de l'installation de votre routeur. Dans la première fenêtre, vous pouvez personnaliser votre "Domain". Indiquez également des serveurs DNS (ici ceux de Quad9), puis cliquez sur le bouton "Next" :
Les serveurs DNS saisis ici sont ceux de Quad9 ; vous pouvez les modifier bien entendu.

• Dans l'étape 3, réglez la "Timezone" sur "Europe/Paris" et cliquez le bouton "Next" 
• Dans l'étape 4, assurez-vous que l'IP WAN est bien en type "Static" et cliquez "Next" 

• Dans l'étape 5, vous retrouvez l'adressage IP du "LAN" tel que vous l'avez configuré lors de l'installation de pfSENSE ; cliquez le bouton "Next" :
• Modifier le mot de passe par défaut pour l'accès à la console pfSENSE en mode graphique (ne laissez pas le mot de passe par défaut que tout le monde connaît !) et cliquez le bouton "Next" 
• La configuration est terminée ! Cliquez le bouton "Reload" pour valider vos choix :

Ces paramètres ont été saisis lors de l'installation de pfSENSE. Vous pouvez les modifier depuis la console de pfSENSE si nécessaire.
Ici, on modifie le mot de passe d'accès à l'interface web de
pfSENSE. Il est important de le faire car le mot de passe par défaut est connu de tous les administrateurs réseau !

pfSENSE relance l'assistant et un message vous confirme la bonne installation de votre routeur :

• Cliquez le bouton "Finish" 
• Validez le contrat de licence en cliquant le bouton "Accept" 
• Cliquez le bouton "Close" pour finaliser l'installation:

Il n'est pas utile, ici, de chercher des mises à jour puisque nous avons installé la dernièreversion du routeur mais vous pouvez le faire
pour vous assurer que vous disposez bien des derniers correctifs pour votre version.

La page d'accueil de pfSENSE s'affiche :
Votre routeur pfSENSE est maintenant totalement configuré et prêt à l'emploi !
Dans le chapitre 2, nous étudierons les bases de la configuration du pare-feu de pfSENSE.


### Utilisation avancée


### FAQ :
