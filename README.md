# MedShakeEHR-Ansible

## Automatisation de l'installation de MedShakeEHR via Ansible pour Debian

Ce que le playbook fait :

- Installation d'Apache
- Installation de MariaDB
- Installation de PHP
- Installation de MedShakeEHR
- Installation d'Orthanc en localhost
- Sécurisation d'Apache
- Sécurisation de MariaDB
- Sécurisation de PHP
- Sécurisation de l'OS

À faire :
- Installation d'OpenVPN
- Sécurisation d'OpenVPN
- Installation d'Orthanc en réseau local
- ?

## Prérequis
- Ansible > 2.5
-  Ansible > 2.9 pour le rôle SSH-sec
- Avoir configuré correctement l'authentification SSH entre l'ordinateur client et l'ordinateur serveur.

## Installation 
- Cloner le projet et installer les rôles :
```bash
git clone https://github.com/marsante/MedShakeEHR-Ansible.git
cd MedShakeEHR-Ansible
ansible-galaxy install -r requirements.yml 
```
- Configurer les variables dans le dossier `variables/main.yml`

- Si vous souhaitez versionner publiquement le projet, créez un fichier `secret.yml`, déplacez toutes les variables sensibles dedans et ajoutez le fichier à `playbook.yml`

- Lancer le playbook en remplaçant user et host avec les bons paramètres :`ansible-playbook playbook.yml -u <user> -i <host>` ou via un fichier `host.yml` : `ansible-playbook -i hosts.yml  --user=<user> playbook.yml`

## Variables
Les variables des rôles `dev-sec.os-hardening`, `dev-sec.ssh-hardening`, `geerlingguy.firewall`, `jnv.unattended-upgrades`, `geerlingguy.apache`, `geerlingguy.php`, `geerlingguy.mysql`, `dev-sec.mysql-hardening`, sont décrites dans les `README` des projets sur Github.

|Nom  | Valeurs par défaut | Description  |
|---- | ------------------ | ------------ |  
|timezone | Europe/Paris | Mettre à l'heure le serveur |
|msehr_package | ['ghostscript', 'imagemagick', 'pdftk', git', 'curl', 'composer', 'ntp'] | Installer les dépendances de MSEHR |
|documentpublic | /home/ehr/public_html | Dossier public de MSEHR |
| countryName | FR | le code de votre pays pour le certificat |
| localityName | Paris | Votre ville pour le certificat |
| organizationName | Dr Strange | Votre raison sociale pour le certificat |
| emailAdress | email@domain.tld | Votre adresse mail pour le certificat |

## Auteurs et Licence

* Auteur : Michaël Val

Licence : GPL v3