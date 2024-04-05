[Continue Exploring My Portfolio: Return to Main Page](https://github.com/MousMaster/Portfolio)


# Résumé des Manifestes Kubernetes

Voici une brève description de chaque manifeste Kubernetes fourni, organisée par fichier :

## 1
app-wordpress-secret.yml : Ce fichier contient les secrets nécessaires pour l'application WordPress, incluant  des informations sensibles comme les identifiants de la base de données.

## 2
kustomization.yml : Un fichier de Kustomize qui définit comment les ressources Kubernetes doivent être personnalisées pour un déploiement spécifique.

## 3
mysql-deployement.yml : Ce manifeste décrit le déploiement d'une instance MySQL. Il contient les spécifications pour créer et gérer les pods MySQL, y compris la version de l'image, les volumes pour la persistance des données, et les configurations nécessaires.

## 4 
mysql-service.yml : Définit un service Kubernetes pour MySQL, permettant la communication et l'accès au déploiement MySQL à travers un nom DNS interne ou une adresse IP stable

## 5 
namespace.yml : Spécifie un espace de noms Kubernetes, permettant d'isoler les ressources déployées. C'est utile pour organiser les ressources dans des groupes logiques et pour la gestion des accès.



## 6 
wordpress-service.yml : Ce fichier crée un service Kubernetes pour WordPress, facilitant l'accès à l'application WordPress via un réseau.

## 7
wordpress-deployment.yml) : Contient les détails du déploiement de l'application WordPress, y compris la configuration des pods, l'utilisation des secrets pour les identifiants de la base de données, et la configuration du service pour exposer l'application.

[Continue Exploring My Portfolio: Return to Main Page](https://github.com/MousMaster/Portfolio)

