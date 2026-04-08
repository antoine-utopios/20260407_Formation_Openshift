# Exercice OpenShift #1 - Création d'utilisateurs et affectation de rôles

## Objectifs

Appréhender la manipulation du RBAC et de la gestion des utilisateurs Openshift

## Sujet 

Réaliser, au sein d'un cluster Openshift Local (CRC), la mise en place de cinq utilisateurs disposant chacun de droits particuliers: 

* Deux administrateurs de cluster: `paul` / `bella`
* Un utilisateur étant capable de gérer les ressources et le RBAC du projet `test-project` => `test-admin`
* Un utilisateur étant capable de créer et de modifier les ressources du projet `test-project` => `mark`
* Un utilisateur étant capable de simplement visualiser les ressources du projet `test-project` => `harold`

Pour vérifier le fonctionnement, ne pas hésiter à lancer des applicatifs avec les commandes:
```bash
oc new-app demo-app --image=nginx --name=demo
```

Puis:
* Supprimer ensuite les utilisateurs `bella` et `test-admin`
* Le mot de passe de Mark n'est pas le bon. Editer son mot de passe pour le changer en `edited` 