# Exercice OpenShift #1 - Création d'utilisateurs et affectation de rôles

## Objectifs

Appréhender la manipulation du RBAC et de la gestion des utilisateurs Openshift

## Sujet 

Réaliser, au sein d'un cluster Openshift Local (CRC), la mise en place de cinq utilisateurs disposant chacun de droits particuliers: 

* Créer le cluster et s'y connecter en tant qu'Administrateur

```bash
crc config set cpus 6
crc config set memory 16384
crc config set disk-size 50

crc setup
crc start

eval $(oc get-env)
oc login -u kubeadmin https://api.crc.testing:6443

oc new-project exo-01
```

Objectif:
* Deux administrateurs de cluster: `paul` / `bella`
* Un utilisateur étant capable de gérer les ressources et le RBAC du projet `test-project` => `test-admin`
* Un utilisateur étant capable de créer et de modifier les ressources du projet `test-project` => `mark`
* Un utilisateur étant capable de simplement visualiser les ressources du projet `test-project` => `harold`


Créer le fichier `users.htpasswd` contenant les utilisateurs demandés
```bash
htpasswd -c -b -B users.htpasswd paul password
htpasswd -b -B users.htpasswd bella password
htpasswd -b -B users.htpasswd test-admin password
htpasswd -b -B users.htpasswd mark password
htpasswd -b -B users.htpasswd harold password
```

Créer le secret à partir du fichier pour l'avoir au sein du cluster
```bash
oc create secret generic htpasswd-exo-01 --from-file=htpasswd=./users.htpasswd -n openshift-config
```

Modifier les providers OAuth du cluster
```bash
oc edit oauth cluster

# modifier en ajoutant le nouveau provider
  # - htpasswd:
  #     fileData:
  #       name: htpasswd-exo-01
  #   mappingMethod: claim
  #   name: exo_01_custom_htpasswd
  #   type: HTPasswd
```

Attendre la relance du pod d'authentification
```bash
oc get pods -n openshift-authentication -w
```

Une fois le pod relancé, on peut tenter de se connecter en tant que mark
```bash
oc login -u mark -p password https://api.crc.testing:6443
```

On ajoute ensuite les droits d'utilisateurs
```bash
oc login -u kubeadmin https://api.crc.testing:6443

oc adm policy add-cluster-role-to-user cluster-admin paul
oc adm policy add-cluster-role-to-user cluster-admin bella

oc adm policy add-role-to-user admin test-admin -n exo-01
oc adm policy add-role-to-user edit mark -n exo-01
oc adm policy add-role-to-user view harold -n exo-01
```

Pour vérifier le fonctionnement, ne pas hésiter à lancer des applicatifs avec les commandes:
```bash
oc login -u mark -p password https://api.crc.testing:6443

oc new-app demo-app --image=nginx --name=demo
# C'est censé passer

oc login -u harold -p password https://api.crc.testing:6443

oc new-app demo-app --image=nginx --name=demo
# C'est censé bloquer
```

Puis:
* Supprimer ensuite les utilisateurs `bella` et `test-admin`

```bash
oc login -u paul -p password https://api.crc.testing:6443

oc extract secret/htpasswd-exo-01 -n openshift-config --to=. --confirm
# Cela va générer dans le dossier actuel un fichier 'htpasswd', que l'on va modifier

htpasswd -D htpasswd bella
htpasswd -D htpasswd test-admin
```

* Le mot de passe de Mark n'est pas le bon. Editer son mot de passe pour le changer en `edited` 

```bash
htpasswd -b -B htpasswd mark edited
```

Enfin, on met à jour le contenu du secret dans le cluster pour changer les utilisateurs

```bash
oc set data secret/htpasswd-exo-01 -n openshift-config --from-file=htpasswd=./htpasswd

# Attendre ensuite que le pod se redémarre
```