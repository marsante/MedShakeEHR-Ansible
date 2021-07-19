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
- Installation d'EasyRSA pour sécuriser les accès OpenVPN
- Installation d'OpenVPN

À faire :
- Installation d'Orthanc en réseau local
- Proposer l'installation d'un module
- Proposer l'initialisation de données de test avec VilagePeople
- ?

## Prérequis
- Ansible > 2.5
- Ansible > 2.9 pour le rôle SSH-sec
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

| Nom                         | Valeurs par défaut                                                       | Description                                                                                                                       |
| ----                        | ------------------                                                       | ------------                                                                                                                      |
| timezone                    | Europe/Paris                                                             | Mettre à l'heure le serveur                                                                                                       |
| msehr_package               | ['ghostscript', 'imagemagick', 'pdftk', git', 'curl', 'composer', 'ntp'] | Installer les dépendances de MSEHR                                                                                                |
| msehr_base_repo_url         | https://codeload.github.com/MedShake/MedShakeEHR-base/tar.gz/refs/tags/  | Url du dépôt où récupérer l'archive MsEHR                                                                                         |
| msehr_base_release          | 7.1.1                                                                    | Version de l'archive à récupérer (sans le préfixe `v`)                                                                            |
| msehr_base_checksum         | sha256:8f13f4f3d6e3de6c9d5e3c9de52dd3683909067d0b01c60d6cbf32e530526a2d  | Somme de contrôle à effectuer sur l'archive récupérer                                                                             |
| create_self_signed_cert     | true                                                                     | Mettre à `false` si vous ne voulez pas créer de certificat auto signé pour le serveur web                                         |
| countryName                 | FR                                                                       | le code de votre pays pour le certificat                                                                                          |
| localityName                | Paris                                                                    | Votre ville pour le certificat                                                                                                    |
| organizationName            | Dr Strange                                                               | Votre raison sociale pour le certificat                                                                                           |
| emailAdress                 | email@domain.tld                                                         | Votre adresse mail pour le certificat                                                                                             |
| easyrsa_alert_renew_service | false                                                                    | Active ou non le rapel par courriel des certificats EasyRSA qui vont expirer (néséssite msmtp mta)                                |
| easyrsa_ca_pass             | secret                                                                   | Passphrase pour la clé privé de la CA EasyRSA                                                                                     |
| openvpn_network             | 10.42.42.0                                                               | Réseau ip pour openvpn                                                                                                            |
| openvpn_netmask             | 255.255.255.0                                                            | Netmask pour le réseau OpenVPN                                                                                                    |
| clients_medshake            | Liste                                                                    | Liste les client devant accèder à medshake au travers du réseau VPN                                                               |
| openvpn_remote              | Not set                                                                  | Founir la valeur de `remote` sur les fichier de configuration des clients                                                         |
| only_vpn_access             | true                                                                     | Si vrais, l'instance MedShake ne sera accèsible que depuis le VPN                                                                 |

## Accès MedShake via OpenVPN

Le rôle met en place un serveur OpenVPN et une [PKI](https://fr.wikipedia.org/wiki/Infrastructure_%C3%A0_cl%C3%A9s_publiques) avec EasyRSA pour sécuriser son accès. Ceci peut être utilisé pour sécuriser les accès à l'instance MedShake à distance. Chaque client devant accéder au serveur dispose de ces propre clé d'accès et de sa propre ip au travers du réseau VPN. Les clients sont configurer dans la variable de configuration du fichier `vars/main.yml` avec la variable `clients_medshake` qui contient une liste ou chaque client est décrit par un nom (`name:`), une passpharse pour protéger la clé privé de son certificat (`pass:`) et l'ip qu'il doit posséder dans le VPN (`ip:`). Voir les exemple dans `vars/main.yml`.

### Configuration OpenVPN pour les client

Pour chaque client une configuration OpenVPN est généré. Les configuration des clients se trouvent sur le serveur dans le dossier `/etc/openvpn/config_for_client/medshake/` dans un fichier `.conf`. Le fichier généré est prêt à l'emploi et contient les clés pour se connecter au serveur. 

### Renouvellement des certificats de clients

Les certificats des clients expire par défaut au bout de 3 ans. Si le playbook est rejoué, le rôle *ansible-role-easyrsa* vas renouveler automatiquement le certificat sauf si le paramètre `renew: false` est ajouté sur l'élément de la liste `clients_medshake`. Le fichier de configuration client OpenVPN pour le client sera mis à jour avec la nouvelle clé et le nouveau certificat.

### Révoquer un certificat

Normalement supprimer un client de la liste `clients_medshake` suffit à lui interdire l'accès au serveur. Il est aussi possible le marquer son certificat comme révoqué en ajoutant `revoked: true` sur un élément de `client_medshake`.

## Auteur, Contributeurs et Licence
- Auteur
[![](https://github.com/marsante.png?size=50)](https://github.com/marsante)
- Contributeurs
[![](https://github.com/indelog.png?size=50)](https://github.com/indelog)

Licence : GPL v3
