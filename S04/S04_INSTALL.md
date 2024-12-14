## Documentation Administrateur

### Sommaire
- [Prérequis techniques]()
- [Étapes d'installation et de configuration]()
- [FAQ]()

### Prérequis techniques

# 1. Objectifs

- 1 . SÉCURITÉ - Gestion d'un firewall pfSense
	- 1 . Mise en place de règles de pare-feu WAN, LAN, DMZ (ex.: https://docs.netgate.com/pfsense/en/latest/recipes/example-basic-configuration.html)
	
  - 2 . Utiliser les principes de gestion de règles suivant :
	-	1. **Deny all** ou **Allow all**
	-	2. **Least privilege** (ex.: https://www.checkpoint.com/fr/cyber-hub/network-security/what-is-the-principle-of-least-privilege-polp/)
	-	3. **Order of rules** (ex. : https://www.tufin.com/blog/firewall-rules-order-intricacies-navigating-best-practices)

- 2 . SÉCURITÉ - Gestion de la télémétrie sur les clients Windows 10/11
 -	1 . Utilisation de script(s) PowerShell
 -	2 . Script(s) exécuté(s) depuis un serveur Windows ou directement sur les clients
 -	3 . Exécution par tâches AT

- 3 . SÉCURITÉ - Gestion de la télémétrie sur un client Windows 10/11
  - 1 . Utilisation de GPO (Utilisateur/Ordinateur)

-4 . RÉSEAU - Amélioration de l'infrastructure Proxmox avec des routeurs
  -	1 . Utilisation de template de routeurs Vyos
  -	2 . Lien avec le schéma réseau initial

- 5 . RÉSEAU - Amélioration de l'infrastructure Proxmox avec des vlans
  -	1 . Simulation de switch par utilisation de tag de vlan
 -	2 . Utilisation de sous-réseau de carte bridge
 -	3 . Lien avec le schéma réseau initial

# 2. VM pfSense

- Nom de la VM : **G2-pfsense-P3** (renommage possible)
- Connexion : `admin` / `P0se!don` (Mot de passe classique de formation à remettre)
	- Si le mot de passe ne fonctionne pas, à remettre à 0 en CLI
- Interfaces réseau :

| Type de sortie pfSense | Nom interface Proxmox | Adresse réseau | Adresse IP       | Passerelle (si existence) | Rmq                  |
| ---------------------- | --------------------- | -------------- | ---------------- | ------------------------- | -------------------- |
| WAN                    | vmbr1                 | 10.0.0.0/29    | 10.0.0.3/29      | 10.0.0.1                  | Ne pas changer l'@IP |
| LAN                    | vmbr555               | 10.10.0.0/16   | 10.10.255.254/16 | -                         | Accès console web    |
| DMZ                    | vmbr570               | 10.12.0.0/16   | 10.12.255.254/16 | -                         | -                    |

# 3. Identification des machines dans Proxmox

## a. Notes

Pour l'identification des VM/CT, remplissage **obligatoire** de la partie **Notes** dans le **Summary** de chaque machine.
Le template markdown ci-dessous doit être utilisé :

```markdown
# Nom de la machine

- Propriétaire : Groupe X (Nom de société)
- Login : xxx
- Mot de passe : xxx
- Usage principal : xxx
- @IP/CIDR (vmbrX)

## Services/Rôles/Logiciels (détails)

- Service/Rôle/Logiciel 1
- Service/Rôle/Logiciel 2
- ...

## Source

- Pour les CT : Nom du template d'origine
- Pour les VM : Nom du template d'origine (ou de l'ISO)
```

Toutes les machines **non-identifiées** au début de sprint suivant **seront supprimées**.
Rmq : Pour l'ajout d'informations, voir avec le formateur/la formatrice.

## b. Tags

Pour l'identification des VM/CT, utilisation **obligatoire** des tags suivant (1 de chaque tableau) :

**Gestion de l'environnement** :

| Tag à utiliser | Signification         |
| -------------- | --------------------- |
| env-prod       | Machine en production |
| env-test       | Machine en test       |

**Gestion de la criticité** :

| Tag à utiliser    | Signification                                                                                                                         |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| priority-critical | Machine critique pour l'infrastructure, sans elle plus rien ne fonctionne                                                             |
| priority-high     | Machine avec priorité haute, qui en cas d'arrêt, amène un arrêt complet sur des zones de l'infrastructure                             |
| priority-medium   | Machine avec priorité moyenne, qui en cas d'arrêt, amène des dysfonctionnement sur certains logiciels et/ou zones de l'infrastructure |
| priority-low      | Machine avec priorité basse, qui n'amène aucun autre problème que ceux engendrés par l'arrêt de cette machine                         |
Toutes les machines **non-taguées** au début de sprint suivant **seront supprimées**.
Rmq : Pour l'utilisation d'autres tags, voir avec le formateur/la formatrice.


### Étapes d'installation et de configuration


### FAQ :
