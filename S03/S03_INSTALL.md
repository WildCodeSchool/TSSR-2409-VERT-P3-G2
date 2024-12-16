## Documentation Administrateur

# 1. Objectifs

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



- 1 . Installation du socle LAMP

apt install apache2 php mariadb-server -y
apt install php-xml php-common php-json php-mysql php-mbstring php-curl php-gd php-intl php-zip php-bz2 php-imap php-apcu -y
apt install php-ldap -y

- 2 . PREPARATION DE LA BASE DONNEE

mysql_secure_installation

n
y
y
y
y
y

mysql -u root -p

CREATE DATABASE db23_glpi;
GRANT ALL PRIVILEGES ON db23_glpi.* TO glpi_adm@localhost IDENTIFIED BY "Pascal01";
FLUSH PRIVILEGES;
EXIT
 
- 3 . Téléchargement de GLPI

- 3.1 . Téléchargement et installation

cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/10.0.10/glpi-10.0.10.tgz
tar -xzvf glpi-10.0.10.tgz -C /var/www/
chown www-data /var/www/glpi/ -R

- 3.2 . Sécurisation

Nous allons devoir créer plusieurs dossiers et sortir des données de la racine Web (/var/www/glpi) de manière à les stocker dans les nouveaux dossiers que nous allons créer. Ceci va permettre de faire une installation sécurisée de GLPI, qui suit les recommandations de l'éditeur.

- mkdir /etc/glpi
- chown www-data /etc/glpi/
- mv /var/www/glpi/config /etc/glpi
- mkdir /var/lib/glpi
- chown www-data /var/lib/glpi/
- mv /var/www/glpi/files /var/lib/glpi
- mkdir /var/log/glpi
- chown www-data /var/log/glpi

- 3.3 . Configuration pour indiquer à GLPI ou sont stockés les données.

nano /var/www/glpi/inc/downstream.php

<?php
define('GLPI_CONFIG_DIR', '/etc/glpi/');
if (file_exists(GLPI_CONFIG_DIR . '/local_define.php')) {
    require_once GLPI_CONFIG_DIR . '/local_define.php';
}

nano /etc/glpi/local_define.php

<?php
define('GLPI_VAR_DIR', '/var/lib/glpi/files');
define('GLPI_LOG_DIR', '/var/log/glpi');

- 4 . Préparer Apache2

nano /etc/apache2/sites-available/domaine.local.conf
<VirtualHost *:80>
   ServerName domaine.local

    - DocumentRoot /var/www/glpi/public

    # If you want to place GLPI in a subfolder of your site (e.g. your virtual host is serving multiple applications),
    # you can use an Alias directive. If you do this, the DocumentRoot directive MUST NOT target the GLPI directory itself.
    # Alias "/glpi" "/var/www/glpi/public"

    <Directory /var/www/glpi/public>
        Require all granted

        RewriteEngine On

        # Redirect all requests to GLPI router, unless file exists.
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>
</VirtualHost>
a2ensite domaine.local
a2dissite 000-default.conf
a2enmod rewrite
systemctl restart apache2

- 5 . Utilisation de PHP8.2-FPM

apt install php8.2-fpm
a2enmod proxy_fcgi setenvif
a2enconf php8.2-fpm
systemctl reload apache2
nano /etc/php/8.2/fpm/php.ini (recherchez l'option "session.cookie_httponly" et indiquez la valeur "on" pour l'activer)
; Whether or not to add the httpOnly flag to the cookie, which makes it
; inaccessible to browser scripting languages such as JavaScript.
; https://php.net/session.cookie-httponly
session.cookie_httponly = on
systemctl restart php8.2-fpm.service

nano /etc/apache2/sites-available/domaine.local.conf (rajouter avant le dernier

<FilesMatch \.php$>
    SetHandler "proxy:unix:/run/php/php8.2-fpm.sock|fcgi://localhost/"
</FilesMatch>
-systemctl restart apache2

- 6 . On vérifie et on ajoute l'utilisateur de la base Mariadb

nano /etc/hosts
Ajoutez une ligne qui mappe le nom d'hôte db23_glpi à l'adresse IP de votre serveur MariaDB (ou de la machine sur laquelle il se trouve). Par exemple :

127.0.0.1 db23_glpi

systemctl restart mariadb

- 7 . Installation de GLPI

voir le tuto https://www.it-connect.fr/installation-pas-a-pas-de-glpi-10-sur-debian-12/


- 8 . Synchronisation de L'annuaire LDAP

Pour intégrer les utilisateurs de l'ADDS dans GLPI , il suffit d'importer l'annuaire:

Entrer les différentes informations nécessaire

Capture d'écran 2024-12-06 095342

Puis se rendre dans l'onglet "Utilisateur" ,

Cliquer sur "Liaison annuaire LDAP "

Capture d'écran 2024-12-06 095452

Cliquer sur "Importer de nouveaux utilisateurs"

Capture d'écran 2024-12-06 110307

Cliquer sur "Mode expert"

Capture d'écran 2024-12-06 110605

La base de donnée de recherche , il ne reste lus qu'a valider en cliquant sur "Rechercher" et sélectionner les utilisteurs voulus.

Les nouveaux utilisateurs sont visibles dans l'onglet "Adminstration/Utilisateurs"






# - 2. Réseau (sous Proxmox)

- Adresse IP de réseau : 10.10.0.0/16
- Adresse de passerelle : 10.10.255.254
- Adresse IP DNS : 10.10.255.254


### Sommaire
- [Prérequis techniques]()
- [Étapes d'installation et de configuration]()
- [FAQ]()

### Prérequis techniques

Configuration du rôle AD-DS sur Windows Server en GUI
Pour rappeler notre serveur G2-SRV-DC1 l'IP 10.10.100.201/24. Ce serveur possède les rôles DHCP , DNS et AD-DS .

G2-SRV-DC1

Modèle : Windows Server 2022/ Type : VM.
- Configuration IP : 10.10.100.201/24 Passerelle : 10.10.100.254/ Carte réseau : G2switchL3.
- Disque dur : 1 HDD 100GO(Système + Dossiers Partagés) / 1 HDD 100GO(RAID1) / 1 HDD 100Go(Sauvegarde).
- Processeur : 2.
- RAM : 8Go.
- Fonction : DHCP/ DNS/ ADDS.
- Rendez-vous à l'annexe ADDS_Conf_WinServGUI .

Installation et Configuration du rôle AD-DS sur Windows Server en Core
Pour rappeler notre serveur G2-SRV-DC2 l'IP 10.10.100.202/24 sera en version Core (non-graphique) et aura uniquement le rôle AD-DS . Il servira à la réplication des données AD-DS, afin d'avoir une redondance sur le réseau.

G2-SRV-DC2

Modèle : Windows Server 2022 Core/ Type : VM.
Configuration IP : 10.10.100.202/24 Passerelle : 10.10.100.254/ Carte réseau : G2switchL3.
Disque dur : 1 HDD 32Go(Système) / 1 HDD 32GO(RAID1).
Processeur : 2.
RAM : 2Go.
Fonction : DNS/ ADDS.


### Étapes d'installation et de configuration

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



### FAQ :
