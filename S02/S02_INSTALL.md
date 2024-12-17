## Documentation Administrateur

### Sommaire
- [Prérequis techniques]()
- [Étapes d'installation et de configuration]()
- [FAQ]()

### Prérequis techniques

** 1. Objectifs **

- Serveur ADDS DNS DHCP	G2-SRV-DC1	100	ADM   
  10.10.100.201/16	  
  10.10.100.0  
  255.255.255.0  
  10.10.100.254  
  
- Serveur ADDS CORE (RODC)	G2-SRV-DC2	100	ADM	  
  10.10.100.202/16  
  10.10.100.0  
  255.255.255.0  
  10.10.100.254  

- Serveur ADDS Debian	G2-SRV-GLPI	100	ADM	  
  10.10.100.203/16  
  10.10.100.0  
  255.255.255.0  
  10.10.100.254  



** 1. AD-DS - Création d'un domaine AD **
	
- 1 . Un serveur Windows Server 2022 GUI avec les rôles AD-DS, DHCP, DNS
- 2 . Un serveur Windows Server 2022 Core avec le rôle AD-DS
- 3 . Les 2 serveurs sont des DC du domaine et ont une réplication complète gérée

** 2. Gestion de l'arborescence AD **
	
- 1 . Création des OU
- 2 . Création des groupes
- 3 . Création des comptes

** 3. Gestion de l'arborescence AD  entièrement automatisée à partir du fichier CSV **
	
- 1 . Création des groupes
- 2 . Création des comptes

** 4. Création d'une VM Serveur Linux Debian mise sur le domaine AD accessible en SSH **

** 5. Création d'une VM client **

- 1 . Sur le domaine AD
- 2 . Avec un compte utilisateur ayant un accès SSH sur le serveur Linux




**1. PREPARATION DE LA MACHINE DEBIAN 12**

a. Mise à jour des paquets
b. Installation d’un serveur LAMP
c. Création de la base de données sur MariaDB
** 2. LANCEMENT DE L’INSTALLATION DE GLPI **

Prérequis :

• Une machine Debian 12 fonctionnelle
• La machine a accès à Internet
** 1 – Mise à jour des paquets Debian **

On commence par mettre à jour les paquets présents :

**apt update & apt upgrade -y**

**2 – Installation, si nécessaire, du serveur LAMP (Linux Apache MariaDB PHP)**

Remarque : cette étape n’est pas nécessaire si un serveur LAMP a déjà été installé.

- a .Installation d’Apache :
apt install apache2
- b . Installation PHP 8.2 (GLPI 10 nécessite une version PHP 8 au minimum) :
Pour installer PHP 8.2 en tant que module Apache, vous devez procéder ainsi (Debian 12 possède les paquets PHP 8.2 par défaut) :
apt install php libapache2-mod-php
sudo systemctl restart apache2
- c . Installation de MariaDB :

**apt install mariadb-server**

Une fois l’installation de MariaDB effectuée, lancez l’utilitaire de configuration du mot de passe root en saisissant la commande
suivante :

mysql_secure_installation (suivez les étapes pour sécuriser MariaDB en définissant le mot de passe du root)

- 3 – Création de la base de données « GLPI »

Pour commencer nous allons nous connecter à MariaDB afin de créer une base de données :

**mysql -u root -p**

(saisir le mot de passe du root que vous avez défini lors de l’installation)
Ensuite nous allons créer une base de données nommée « glpi », créer un utilisateur « glpi », lui donner un mot de passe et lui
accorder tous les droits de lecture/écriture. Pour cela, nous saisissons les commandes :

create database glpi; (création de la base de données « glpi »)
create user 'glpi'@'localhost' identified by 'glpi'; (création de l’utilisateur avec son mot de passe qui sera « glpi »)
grant all privileges on glpi.* to 'glpi'@'localhost' with grant option; (on augmente les droits de l’utilisateur)

flush privileges; (on met à jour les modifications apportées)
quit (ou exit)
- 4 – Téléchargement et décompression de l’archive « GLPI »

Pour installer GLPI, il est nécessaire de connaître le lien de téléchargement du logiciel. En parcourant le web, on trouve l’adresse
exacte de téléchargement de la dernière version stable (on évitera les versions beta et RC) :
https://github.com/glpi-project/glpi/releases/download/10.0.10/glpi-10.0.10.tgz

Sur la machine Debian, on peut créer un dossier « glpi » dans lequel on téléchargera l’archive, puis on lance le téléchargement de
l’archive GLPI depuis ce dossier.


wget https://github.com/glpi-project/glpi/releases/download/10.0.10/glpi-10.0.10.tgz

Une fois l’archive téléchargée, il faut la décompresser en saisissant :

**tar xvf glpi-10.0.10.tgz**

Un dossier « glpi » est créé (et contient tous les fichiers nécessaires à l’installation de GLPI) :

On va maintenant déplacer ce dossier décompressé nommé « glpi » dans l’arborescence d’Apache et à l’endroit suivant :
/var/www/html (c’est à cet endroit que se trouve la page d’accueil par défaut d’Apache ; le fameux « It Works ! »). Ici, nous
n’avons pas créé de virtualhost pour simplifier le tutoriel.

**mv glpi /var/www/html/glpi**

Le dossier « glpi » est maintenant situé dans l’arborescence du serveur web Apache2.
Attention, nous travaillons, ici, en mode laboratoire. Si vous travaillez en mode production, GLPI conseille de séparer les dossiers
de données et de log et de les mettre dans un autre emplacement que l’emplacement par défaut d’Apache.

- 2 – LANCEMENT DE L’INSTALLATION DE GLPI 10.0.10

Avant de lancer l’installation de GLPI, vous devez ajouter les modules PHP suivants qui sont nécessaires à GLPI :

apt install php8.2-curl php8.2-gd php8.2-mbstring php8.2-zip php8.2-xml php8.2-ldap php8.2-intl php8.2-mysql php8.2-dom
php8.2-simplexml php-json php8.2-phpdbg php8.2-cgi

Il faut apporter des modifications nécessaires à la bonne installation de GLPI, notamment au niveau du propriétaire et des droits.

On commence par donner la propriété du dossier GLPI à l’administrateur d’Apache (le « www-data ») et on accorde les droites
nécessaires :

**chown -R www-data:www-data /var/www/html/glpi/**
**chmod -R 755 /var/www/html/glpi/**

On redémarre le serveur Apache :

**systemctl restart apache2**

Pour terminer l’installation de l’helpdesk GLPI, il suffit d’ouvrir le navigateur et de saisir, dans la barre d’adresse, l’IP de votre
serveur web Apache suivi de /glpi. 

Attention, si vous avez configuré un virtualhost, adaptez l’URL pour lancer l’installation de GLPI.
On obtient alors l’affichage de l’assistant d’installation de GLPI. On sélectionne le langage, puis « OK » :
On accepte le contrat de licence, puis « Continuer » :
Comme il s’agit d’une première installation, on clique sur le bouton « Installer » :

Attention, il est possible que l’installation ne puisse pas être lancée si certains modules PHP sont absents sur votre machine
Debian. 
Dans ce cas, retournez sur votre console Debian et ajoutez les modules absents via « apt install php-xxx ».
On retourne sur la page web de l’installeur GPLI et on rafraîchit la page ; normalement, l’écran affiche ceci :
La 1ère étape consiste à se loguer au serveur SQL (MariaDB). On indique « localhost » et l’utilisateur « glpi » précédemment
configuré (avec son mot de passe !) et on clique sur le bouton « Continuer » :

L’ensemble des extensions nécessaires doit être en statut « Requis » sinon l’installation ne peut pas se poursuivre.
Si certains modules PHP obligatoires sont absents sur votre machine Debian, retournez sur votre console Debian et ajoutez les modules absents via « apt install php-xxx ».

Si tout est correct comme l’image ci-contre, cliquez le bouton « Continuer » afin de poursuivre l’installation de GLPI.

Logiquement, la connexion à la base « glpi » doit s’effectuer (message « Connexion à la base de données réussie »). Si la connexion
est fonctionnelle, la base « glpi » apparaît. On la sélectionne et on clique le bouton « Continuer » :

Il faut attendre l’initialisation de la base de données (attention cette phase peut prendre du temps ; soyez patient !). 
Si tout se passe bien au niveau de l’initialisation de la base, une fenêtre s’affiche ; cliquez le bouton « Continuer » :

La fin de l’assistant s’affiche et des identifiants de tests sont fournis. Le logiciel est prêt à être utilisé :
Les comptes par défaut de GLPI sont :

glpi/glpi
tech/tech
normal/normal
post-only/post-only

Cliquez le bouton « Utiliser GLPI » : l’écran d’authentification s’affiche : on saisit, ici, les identifiants de base « glpi » - « glpi »
comme stipulé par l’installeur pour entrer en mode administrateur :

Lors de la première connexion, GLPI affichera ce message :

Pour changer les mots de passe des utilisateurs par défaut, il suffit de cliquer sur le lien hypertexte de ces derniers et de modifier
le mot de passe dans le profil.

Pour le fichier « install.php », il faudra revenir sur notre serveur web (Debian) et taper cette commande pour supprimer le fichier
par mesure de sécurité :

rm -f /var/www/html/glpi/install/install.php

Si on déconnecte la session administrateur et que l’on se reconnecte avec « glpi » - « glpi », l’écran d’accueil s’affiche (nous avons,
ici, laissé les mots de passe par défaut mais l’alerte sur le fichier « install.php » a bien disparu) :

Ici, nous utilisons l’identifiant « glpi » et le mot de passe « glpi » pour se connecter en tant qu’administrateur et on clique sur « Se connecter ».




** 2. Réseau (sous Proxmox) **

- Adresse IP de réseau : 10.10.0.0/16
- Adresse de passerelle : 10.10.255.254
- Adresse IP DNS : 10.10.255.254

** Étapes d'installation et de configuration **


### FAQ :
