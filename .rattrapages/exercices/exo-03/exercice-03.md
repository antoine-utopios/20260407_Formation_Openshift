# Exercice OpenShift #2 - Utilisation des BuildConfigs et du mécanisme S2I

## Objectifs

Appréhender le déploiement dans un cluster Openshift de code source via la stratégie S2I

## Sujet 

Réaliser, au sein d'un cluster Openshift Local (CRC), le déploiement d'une application de base React.

* Créer un dépot Git en ligne et le rendre publique
* Y déposer le code source de notre application React de base 
* Créer un projet à part au sein du cluster Openshift
* Demander la création d'une application en faisant appel à la stratégie S2I (sans ou avec Dockerfile)
* Rendre notre applicatif accessible au moyen d'une route
* Ajouter des sondes de santés et de vivacité au déploiement
* Ajouter des limites d'utilisations afin d'éviter les problèmes de gestion de ressources