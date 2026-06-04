# Exercice OpenShift #2 - d'une Base de Données MySQL 

## Objectifs

Appréhender le déploiement d'image de conteneur Docker utilisant des volumes et des variables d'environnement dans un cluster Openshift

## Sujet 

Réaliser, au sein d'un cluster Openshift Local (CRC), le déploiement d'une image Docker d'une base de données de type MySQL.

* Rechercher une image Docker compatible sur DockerHub
* Récupérer l'image en local et la tester via l'utilisation des commandes Docker 
* Créer le compte de service compatible avec l'image Docker que l'on a récupéré
* Créer les fichiers de manifeste pour la mise en place du déploiement dans le cluster
* Déployer l'applicatif via les commandes Openshift du CLI
* Tester le déploiement via un shell sécurisé (`oc rsh`) en se branchant à un pod ou au déploiement directement
* Rendre au besoin le déploiement accessible via un mapping de port pour tester la base de données via un client GUI type dBeaver