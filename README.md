# MedShakeEHR-Ansible
## Automatisation de l'installation de MedShakeEHR via Ansible pour Debian
En cours de développement, ne pas utiliser en production.

Fonctionnels :

- Installation d'Apache
- Installation de MariaDB
- Installation de PHP
- Installation de MedShakeEHR
- Sécurisation d'Apache
- Sécurisation de MariaDB
- Sécurisation de PHP
- Sécurisation de l'OS

Produits des erreurs :
- Rôle MariaDB
- Rôle Firewall
- Rôle SSH

A faire :
- Installation d'Orthanc
- Installation d'OpenVPN
- Sécurisation d'OpenVPN
- ?

## Installation 
- Cloner le projet
- Créer un fichier `hosts.yml` à la racine du projet comme cet exemple :

```bash
---
all:
    hosts:
        172.17.0.2:
```

- Lancer la commande ansible-playbook -i hosts.yml  --user=root playbook.yml 
- En remplaçant root par l'utilisateur ayant le fichier `authorized_key` de configuré sur la machine à approvisonner.