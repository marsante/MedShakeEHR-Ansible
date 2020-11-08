# MedShakeEHR-Ansible

## Automatisation de l'installation de MedShakeEHR via Ansible pour Debian

Ce que le playbook fait :

- Installation d'Apache
- Installation de MariaDB
- Installation de PHP
- Installation de MedShakeEHR
- Sécurisation d'Apache
- Sécurisation de MariaDB
- Sécurisation de PHP
- Sécurisation de l'OS

A faire :
- Installation d'Orthanc
- Installation d'OpenVPN
- Sécurisation d'OpenVPN
- ?

## Pré-requis
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

- Si vous souhaitez versionner publiquement le projet créez un fichier `secret.yml`, déplacez toutes les variables sensibles dedans et ajoutez le fichier à `playbook.yml`

- Lancer le playbook en remplaçant user et host avec les bons paramètres :`ansible-playbook playbook.yml -u <user> -i <host>` ou via un fichier `host.yml` : `ansible-playbook -i hosts.yml  --user=<user> playbook.yml`

## Variables
Les variables des rôles `dev-sec.os-hardening`, `dev-sec.ssh-hardening`, `geerlingguy.firewall`, `jnv.unattended-upgrades`, `geerlingguy.apache`, `geerlingguy.php`, `geerlingguy.mysql`, `dev-sec.mysql-hardening`, sont décrites dans les `README` des projets sur Github.

|Nom  | Valeurs par défaut | Description  |
|---- | ------------------ | ------------ |  
|timezone | Europe/Paris | Mettre à l'heure le serveur |
|msehr_package | ['ghostscript', 'imagemagick', 'pdftk', git', 'curl', 'composer', 'ntp'] | Installer les dépendances de MSEHR |
documentpublic | /home/ehr/public_html | Dossier public de MSEHR |

### Variable pour https
La configuration n'est que semi-automatique. Pour commencer, décommenter la variable suivante dans `vars/main.yml` :

```bash
    apache_vhosts_ssl:
      - servername: "local.dev"
        documentroot: "/var/www/html"
        certificate_file: "/home/vagrant/example.crt"
        certificate_key_file: "/home/vagrant/example.key"
```
Modifier les différentes informations pour que cela corresponde à l'usage. Il faut générer sur le serveur le certificat puis indiquer le chemin d'accès aux fichiers pour les variables `certificate_file`, `certificate_key_file` sur l'ordinateur client.

## Auteurs et Licence

* Auteur : Michaël Val

Licence : GPL v3