[Continue Exploring My Portfolio: Return to Main Page](https://github.com/MousMaster/Portfolio)



# ansible-role-containerized-apache

## Aperçu
Le rôle `ansible-role-containerized-apache` est conçu pour déployer un serveur Apache dans un conteneur Docker sur des hôtes basés sur CentOS. Ce rôle assure l'installation et la configuration appropriées des prérequis nécessaires tels que Python et Docker avant de procéder au déploiement du conteneur Apache.

## Variables
Les variables clés utilisées dans ce rôle sont définies dans les fichiers `defaults/main.yml` et `vars/main.yml`. Voici quelques-unes de ces variables :

- `home_page_message` : Message à afficher sur la page d'accueil du serveur Apache.
- `system_user` : Utilisateur système sous lequel le conteneur Docker sera exécuté.
- `docker_container_name` : Nom du conteneur Docker.
- `external_port` et `apache_port` : Ports pour accéder au serveur Apache.
- `home_page_file_name` : Nom du fichier de la page d'accueil.
- `domain` : Domaine ou adresse IP de l'hôte.
- `python_version` : Version de Python à installer.

## Dépendances
Ce rôle dépend des modules Ansible suivants :

- pip
- ansible.builtin.yum
- ansible.builtin.yum_repository
- ansible.builtin.systemd
- docker_container

## Installation
Les tâches pour installer Docker et Python sont incluses dans `tasks/install-docker.yml` et `tasks/install-python.yml`, respectivement. Le déploiement du serveur Apache est géré par les tâches définies dans `tasks/main.yml`, qui :

- Importent les tâches pour l'installation de Docker et de Python.
- Copient le modèle de la page d'accueil sur l'hôte cible.
- Créent et démarrent un conteneur Docker pour le serveur Apache.
- Attendent que le conteneur Docker soit opérationnel et que le site Web soit accessible.


[Continue Exploring My Portfolio: Return to Main Page](https://github.com/MousMaster/Portfolio)

