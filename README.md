# MedShakeEHR-Ansible

## Automatisation de l’installation de MedShakeEHR via Ansible pour Debian

Ce que le playbook fait :

- Installation d’Apache
- Installation de MariaDB
- Installation de PHP
- Installation de MedShakeEHR
- Installation d’Orthanc en localhost
- Installation d’Wireguard (VPN)
- Sécurisation d’Apache
- Sécurisation de MariaDB
- Sécurisation de PHP
- Sécurisation de l’OS
- Sécurisation d’Wireguard (VPN)

À faire :
- ?

## Prérequis
- Ansible > 2.9.10
- Avoir configuré correctement l’authentification SSH entre l’ordinateur client et l’ordinateur serveur.

## Installation 
- Cloner le projet et installer les rôles :
```bash
git clone https://github.com/marsante/MedShakeEHR-Ansible.git
cd MedShakeEHR-Ansible
ansible-galaxy install -r requirements.yml 
```
- Configurer les variables dans le dossier `variables/main.yml` ou `group_var`
- Si vous souhaitez versionner publiquement le projet, créez un fichier `secret.yml`, déplacez toutes les variables sensibles dedans et ajoutez le fichier à `playbook.yml`
- Lancer le playbook en remplaçant user et host avec les bons paramètres :`ansible-playbook playbook.yml -u <user> -i <host>` ou via un fichier `host.yml` : `ansible-playbook -i hosts.yml  --user=<user> playbook.yml`

## Variables
| Nom                | Valeur par défaut                                      | Description                                                                                      |
|--------------------|--------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| country_name        | FR                                                     | Nom du pays                                                                                     |
| locality_name       | Paris                                                  | Nom de la ville                                                                                 |
| organization_name   | Dr Strange                                             | Nom de l’organisation                                                                           |
| email_address      | email@domain.tld                                       | Adresse e-mail                                                                                  |
| sql_root_password    | root                                                   | Mot de passe root pour SQL                                                                      |
| sql_user     | user                                                   | Nom du compte utilisateur SQL                                                                   |
| sql_user_password    | user                                                   | Mot de passe du compte utilisateur SQL                                                          |
| sql_database          | medshakeehr                                            | Nom de la base de données SQL                                                                   |
| msehr_base_release | 8.1.1                                                  | Version de MedShakeEHR                                                                  |
| msehr_base_repo_url| https://codeload.github.com/MedShake/MedShakeEHR-base/tar.gz/refs/tags/ | URL de MedShakeEHR                                          |
| msehr_base_checksum| sha256:5ae9ddf3e528eab4fe5882724cc00d912600a9fb1748ff31e47c6191a173b200 | Checksum  de MedShakeEHR                                        |
| msehr_dir          | /opt/ehr                                               | Chemin d’installation de MedShakeEHR                                                            |
| timezone           | Europe/Paris                                           | Fuseau horaire                                                                                   |
| domain             | msehr.local                                            | Nom de domaine                                                                                   |
| upload_max_filesize  | upload_max_filesize = 20M                              | Taille maximale des fichiers téléversés                                                         |
| post_maxsize        | post_max_size = 20M                                    | Taille maximale des données POST                                                                |
| max_input_vars       | max_input_vars = 20000                                 | Nombre maximal de variables d’entrée                                                             |
| error_reporting     | error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT    | Niveau de rapport d’erreurs                                                                      |
| display_errors      | display_errors = Off                                   | Affichage des erreurs (Off : désactivé, On : activé)                                            |
| display_startup_errors| display_startup_errors = Off                           | Affichage des erreurs de démarrage (Off : désactivé, On : activé)                               |
| msehr_packages      | [‘acl’, ‘apache2’, ‘composer’, ‘curl’, ‘ghostscript’, ‘git’, ‘grub2’, ‘imagemagick’, ‘mariadb-server’, ‘ntp’, ‘pdftk-java’, ‘php’, ‘php-bcmath’, ‘php-curl’, ‘php-gd’, ‘php-gnupg’, ‘php-imagick’, ‘php-imap’, ‘php-intl’, ‘php-json’, ‘php-mysql’, ‘php-soap’, ‘php-xml’, ‘php-yaml’, ‘php-zip’, ‘python3-mysqldb’, ‘python3-openssl’, ‘ufw’, ‘unattended-upgrades’ ] | Liste des packages requis pour MedShakeEHR |                                                   |

## Auteur, Contributeurs et Licence
- Auteur
[![](https://github.com/marsante.png?size=50)](https://github.com/marsante)
- Contributeurs
[![](https://github.com/indelog.png?size=50)](https://github.com/indelog)

Licence : GPL v3
