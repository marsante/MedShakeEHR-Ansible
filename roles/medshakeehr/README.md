MedShakeEHR Role
=========

Le rôle ansible qui permet d'installer MedShakeEHR

Requirements
------------

Aucun

Role Variables
--------------

Les variables des collections et rôles `geerlingguy.apache`, `geerlingguy.php`, `geerlingguy.mysql`, sont décrites dans les `README` des projets sur Github.

| Nom                     | Valeurs par défaut                                                       | Description                                                                                |
| ----                    | ------------------                                                       | ------------                                                                               |
| timezone                | Europe/Paris                                                             | Mettre à l'heure le serveur                                                                |
| msehr_package           | ['acl','curl', 'ghostscript','git','grub','imagemagick','ntp','pdftk','python3-openssl','wget'] | Installer les dépendances de MSEHR                                                         |
| msehr_base_repo_url     | https://codeload.github.com/MedShake/MedShakeEHR-base/tar.gz/refs/tags/  | Url du dépôt où récupérer l'archive MsEHR                                                   |
| msehr_base_release      | 7.1.1                                                                    | Version de l'archive à récupérer (sans le préfixe `v`)                                     |
| msehr_base_checksum     | sha256:8f13f4f3d6e3de6c9d5e3c9de52dd3683909067d0b01c60d6cbf32e530526a2d  | Somme de contrôle à effectuer sur l'archive récupérer                                      |
| create_self_signed_cert | true                                                                     | Mettre à `false` si vous ne voulez pas créer de certificat auto signé pour le serveur web |
| countryName             | FR                                                                       | le code de votre pays pour le certificat                                                   |
| localityName            | Paris                                                                    | Votre ville pour le certificat                                                             |
| organizationName        | Dr Strange                                                               | Votre raison sociale pour le certificat                                                    |
| emailAdress             | email@domain.tld                                                         | Votre adresse mail pour le certificat                                                       |


Dependencies
------------

geerlingguy.php
geerlingguy.mysql
geerlingguy.apache

Example Playbook
----------------


    - hosts: medshakeehr
      roles:
         - { role: marsante.medshakeehr }

License
-------

GPL v3

Author Information
------------------

- Auteur
[![](https://github.com/marsante.png?size=50)](https://github.com/marsante)
- Contributeurs
[![](https://github.com/indelog.png?size=50)](https://github.com/indelog)
