## Documentation Générale

### Sommaire
- [Présentation du projet]()
- [Membres du groupe de projet]()
- [Choix techniques]()
- [Difficultés rencontrées]()
- [Solutions trouvées]()
- [Améliorations possibles]()
- [FAQ]()

### Présentation du projet

### Membres du groupe de projet
![Tableau Team](https://github.com/user-attachments/assets/e211ef6c-1778-45a3-a84b-fe1debb810ad)


## Objectif du Projet

Soutenir la croissance d'EcoTech Solutions et améliorer l'efficacité de leurs opérations, tout en renforçant leur engagement envers l'innovation et la transition vers une économie verte.

## Détails Techniques et Fonctionnels

- **Utilisateurs** : Répartition et ajout de comptes.
- **Départements** : Communication, Développement, Direction, RH, DSI, Finance, Commercial.
- **Matériel Client** : PC portables hétérogènes.
- **Réseau Actuel** : Wifi avec une box FAI, réseau en 10.10.8.0/24.
- **Messagerie** : Hébergée en cloud.
- **Sécurité** : Pas de matériel spécifique, PC en workgroups sans mot de passe.
- **Stockage** : NAS grand public sans rétention ni redondance.
- **Téléphonie** : Fixe et mobile variable.

## Plan du Projet

- **Sprints Hebdomadaires** : Diviser le projet en sprints d'une semaine avec des objectifs clairs.
- **Validation et Réévaluation** : Évaluer les tâches et les réévaluer régulièrement pour améliorer l'infrastructure.
- **Documentation et Stockage** : Utiliser Proxmox pour l'infrastructure réseau, stockage des fichiers sur le drive et GitHub.

## Conclusion

- **Engagement** : Assurer une mise en place efficace et optimale de l'infrastructure réseau.



### Choix techniques
## 1. Infrastructure Réseau

- **Matériel Réseau** : Choisir des routeurs, switches, et points d'accès wifi adéquats pour assurer une couverture et une performance réseau optimales.
- **Configuration de VLAN** : Segmenter le réseau en différents VLAN pour chaque département afin de renforcer la sécurité et la gestion du réseau.
- **Adressage IP** : Planification de l'adressage IP pour inclure les plages IP nécessaires et éviter les conflits d'adresses.

## 2. Matériel et Administration

- **Matériel Client** : Utilisation de PC portables de différentes marques, en s'assurant de leur compatibilité avec le nouveau réseau.
- **Serveurs** : Évaluation des besoins en serveurs pour héberger des services critiques tels que les applications internes et la gestion du réseau.
- **Outils de Gestion** : Utiliser des outils de gestion réseau comme Ansible ou SolarWinds pour automatiser les tâches de configuration et de surveillance.

## 3. Sécurité

- **Sécurité des Identités** : Implémenter des méthodes d'authentification robustes, comme l'authentification multifactorielle (MFA).
- **Sécurité Réseau** : Déployer des pare-feu, des systèmes de détection d'intrusion (IDS), et d'autres mesures de sécurité pour protéger le réseau.
- **Cryptage** : Utiliser le chiffrement pour sécuriser les communications et les données sensibles.

## 4. Messagerie et Stockage

- **Messagerie** : S'assurer que la messagerie hébergée en cloud est correctement intégrée et sécurisée.
- **Stockage de Données** : Mettre en place des solutions de stockage avec redondance et rétention des données pour éviter la perte de données critiques.

## 5. Évolution et Flexibilité

- **Préparation pour l'Évolution** : Planifier le réseau et les systèmes pour permettre une croissance future, y compris la possibilité d'intégrer de nouveaux partenaires ou services.
- **Gestion du Changement** : Mettre en place des processus pour gérer les modifications de configuration et les mises à jour sans perturber les opérations.

## 6. Support et Maintenance

- **Support Technique** : Prévoir des solutions de support technique pour assister les utilisateurs et maintenir l'infrastructure.
- **Formation** : Former le personnel interne sur les nouvelles technologies et configurations déployées.


### Difficultés rencontrées


### Solutions trouvées


### Améliorations possibles


### FAQ :
