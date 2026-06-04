# Exercice OpenShift #1 - Création d'utilisateurs et affectation de rôles

## Objectifs

Appréhender le déploiement d'image de conteneur Docker provenant de DockerHub dans un cluster Openshift

## Sujet 

Réaliser, au sein d'un cluster Openshift Local (CRC), le déploiement d'une image Docker permettant de jouer au jeu 2048 dans un navigateur. 

* Rechercher une image Docker compatible sur DockerHub
* Récupérer l'image en local et la tester via l'utilisation des commandes Docker 
* Créer le compte de service compatible avec l'image Docker que l'on a récupéré
* Créer les fichiers de manifeste pour la mise en place du déploiement dans le cluster
* Déployer l'applicatif via les commandes Openshift du CLI
* Tester le déploiement via la création d'un mapping de ports sur notre machine et l'ouverture de `http://localhost:8080` sur le navigateur