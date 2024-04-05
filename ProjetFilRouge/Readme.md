[Continue Exploring My Portfolio: Return to Main Page](https://github.com/MousMaster/Portfolio)

 
 # PROJET FINAL DEVOPS DANS LE CADRE DU BOOTCAMP EAZYTRAINNING. 

## 1 Ennonce : 
Lien : 
La société **IC GROUP** dans laquelle vous travaillez en tant qu’ingénieur Devops souhaite mettre sur pied un site web vitrine devant permettre d’accéder à ses 02 applications : [Ennoncé](https://github.com/sadofrazer/ic-webapp)
  

## 2 Modele propose : 


### Partie 1 :  Dockeristaion du site vitrine 


L'application a été dockerisée en créant un fichier Dockerfile décrivant les dépendances et les étapes d'installation. Ensuite, Docker a été utilisé pour construire une image à partir de ce Dockerfile avec la commande docker build. Enfin, l'application a été exécutée dans un conteneur Docker isolé en utilisant la commande docker run.
Un apperçu de l'application est visble ci dessous.

![project](https://github.com/MousMaster/ProjetFilRouge/blob/main/images/ic-webapp.png)

### Partie 2 :  Déploiement des différentes applications dans un cluster Kubernetes

![project](https://github.com/MousMaster/ProjetFilRouge/blob/main/images/kubernetes.png)

Dans cette architecture Kubernetes, les choix des services ont été faits en tenant compte des exigences de sécurité et des bonnes pratiques. Les services NodePort ont été utilisés pour les applications nécessitant un accès externe, tout en garantissant une certaine sécurité grâce à la délimitation des ports exposés. Par exemple, les services NodePort sont appropriés pour des applications telles que les API ou les applications Web qui doivent être accessibles depuis l'extérieur du cluster.
Ainsi ils ont ete utilise pour B, D, et H

D'autre part, le service ClusterIP a été choisi pour le composant interne sensibles  la base de données F , afin de limiter son accessibilité uniquement à l'intérieur du cluster Kubernetes. Cela renforce la sécurité en empêchant tout accès externe non autorisé à ce service, tout en permettant aux autres composants de l'application d'interagir avec elle de manière sécurisée à l'intérieur du cluster.

voici la représentation de chaque lettre :

A, C et G ,  sont des nodePort
E est un clusterIp.
B, D, H et F representent respectivement ic-webapp, odoo, pgAdmin et la base de donnee de odoo. Il  sont de type deployments.

L'enssemble du manifest kubernetes se trouve dans le repertoire 'manifestes-k8s'.

### Partie 3 : Pipeline CI/CD 



#### Partie CI Jenkins 

<div style="text-align:center;">
    <img src="https://github.com/MousMaster/ProjetFilRouge/blob/main/images/Jenkins.png" width="300">
</div>

# Pipeline CI/CD pour l'application Web

Ce pipeline Jenkins automatise le processus de développement logiciel, du build à la publication, en passant par les tests et le déploiement.

## Environnement

Les variables d'environnement sont définies pour faciliter le déploiement et la gestion du pipeline.

- `IMAGE_NAME`: Nom de l'image Docker.
- `APP_CONTAINER_PORT`: Port utilisé par le conteneur de l'application.
- `DOCKERHUB_ID`: Identifiant Docker Hub.
- `DOCKERHUB_PASSWORD`: Mot de passe Docker Hub.
- `ANSIBLE_IMAGE_AGENT`: Image Ansible utilisée pour les déploiements.
- `IC_WEBAPP_SERVER_DEV`: Adresse IP du serveur pour l'environnement DEV.

## Stages du pipeline

1. **Build de l'image**:
   - Construction de l'image Docker à partir du Dockerfile.

2. **Analyse de l'image avec Snyk**:
   - Analyse de l'image Docker pour détecter les vulnérabilités de sécurité.

3. **Exécution du conteneur basé sur l'image construite**:
   - Lancement d'un conteneur Docker pour effectuer des tests.

4. **Test de l'image**:
   - Vérification de l'accessibilité de l'application via curl.

5. **Nettoyage du conteneur**:
   - Arrêt et suppression du conteneur Docker après les tests.

6. **Connexion et publication de l'image sur Docker Hub**:
   - Publication de l'image construite sur Docker Hub.

7. **Construction d'une instance EC2 sur AWS avec Terraform**:
   - Création d'une instance EC2 sur AWS à l'aide de Terraform.

8. **Préparation de l'environnement Ansible**:
   - Configuration des clés et des variables pour Ansible.

9. **Déploiement de l'environnement DEV pour les tests**:
   - Déploiement de l'application dans l'environnement DEV avec Ansible.

10. **Suppression de l'environnement DEV**:
    - Destruction de l'environnement DEV une fois les tests terminés.

11. **Déploiement en PRODUCTION**:
    - Déploiement de l'application dans l'environnement de production avec Ansible.

#### Notifications

Le résultat du pipeline est notifié via Slack pour une communication efficace au sein de l'équipe de développement.

---
Ce pipeline CI/CD automatisé garantit un déploiement fluide et sécurisé de l'application web, de la construction à la publication, en passant par les tests et le déploiement dans différents environnements.



##### Partie 4 : CD Ansible 
![project](https://github.com/MousMaster/ProjetFilRouge/blob/main/images/ansible.png)
 

Ce référentiel Ansible est une suite d'outils de déploiement pour la mise en place automatisée d'applications. Il offre une structure organisée, segmentée par rôles et playbooks, pour une application transparente des configurations à divers environnements.

#### Structure du Référentiel

#### Configuration Globale

- `ansible.cfg`: Les réglages par défaut pour Ansible, incluant les paramètres de performances et de connexion.
- `hosts.yml`: L'inventaire détaillé des hôtes, regroupés par environnement ou fonction.

##### Variables d'Environnement

Les variables d'environnement contrôlent les paramètres spécifiques à chaque déploiement et sont réparties en :

- `group_vars/`: Variables applicables à des groupes d'hôtes, avec des fichiers distincts pour les environnements de production (`prod.yml`), de développement (`dev.yml`), et globaux (`all.yml`).
- `host_vars/`: Configuration individuelle des hôtes, comme les adresses IP et les configurations spécifiques au serveur.

##### Rôles

Les rôles Ansible définissent des configurations réutilisables et des blocs de tâches pour les composants suivants :

- **Install Docker (`roles/install-docker/`)**: Installe Docker sur les hôtes cibles.
- **Odoo Server (`roles/odoo_role/`)**: Configure et déploie un serveur Odoo, y compris la création des conteneurs Docker via les templates.
- **PgAdmin Server (`roles/pgadmin_role/`)**: Déploie PgAdmin, configuré pour se connecter aux instances de PostgreSQL.

Chaque rôle inclut :
- `tasks/main.yml`: La liste des tâches exécutées par le rôle.
- `defaults/main.yml`: Les valeurs par défaut des variables pour le rôle.
- `vars/main.yml`: Les variables de surcharge pour des configurations plus spécifiques.
- `templates/`: Fichiers modèles Jinja2 utilisés pour générer des fichiers de configuration dynamiques.

#### Playbooks

Les playbooks orchestrent le déploiement de l'infrastructure et des applications. Voici quelques exemples :

- `deploy-ic-webapp.yml`: Automatise le déploiement de l'application web IC.
- `deploy-odoo.yml`: Gère le déploiement d'Odoo, en utilisant les rôles et les variables pour une mise en place cohérente.
- `deploy-pgadmin.yml`: Configure et déploie PgAdmin pour la gestion de bases de données PostgreSQL.
- `install-docker.yml`: S'assure que Docker est installé et configuré sur les hôtes spécifiés.



#### Partie 5 : Terraform
![project](https://github.com/MousMaster/ProjetFilRouge/blob/main/images/terraform.jpeg)

# Configuration Terraform pour l'infrastructure cloud

Cette configuration Terraform définit l'infrastructure nécessaire pour déployer une application dans le cloud, en utilisant des modules réutilisables pour chaque composant principal.

## Modules

### EC2

Définit les instances EC2 (serveurs virtuels) nécessaires pour l'application.

- `ec2/main.tf` : Configure les instances EC2 avec la taille souhaitée, l'AMI, et d'autres paramètres.
- `ec2/output.tf` : Fournit des informations sur les instances EC2 créées.
- `ec2/variable.tf` : Permet de personnaliser l'instance EC2, comme le type d'instance et la clé SSH.

### EIP

Gère les adresses IP élastiques pour assurer une connectivité réseau stable.

- `eip/main.tf` : Associe des adresses IP élastiques à des instances EC2 spécifiques.
- `eip/output.tf` : Sort les détails des adresses IP élastiques attribuées.
- `eip/variable.tf` : Définit des variables liées aux adresses IP élastiques.

### SG (Security Group)

Configure les groupes de sécurité pour contrôler l'accès réseau aux instances EC2.

- `sg/main.tf` : Définit les règles de groupe de sécurité pour les ports entrants et sortants.
- `sg/output.tf` : Donne les identifiants des groupes de sécurité créés.
- `sg/variable.tf` : Contient les variables pour personnaliser les règles de groupe de sécurité.


[Continue Exploring My Portfolio: Return to Main Page](https://github.com/MousMaster/Portfolio)
