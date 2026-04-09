# Démo 02 — Gestion des projets et RBAC

## Objectif

Démontrer la création de projets, la gestion des utilisateurs et l'attribution de rôles.

## Étapes de la démonstration

### 1. Créer des projets

```bash
# Connexion admin
oc login -u kubeadmin -p <password> https://api.crc.testing:6443

# Créer des projets
oc new-project demo-dev \
  --display-name="Démo Développement" \
  --description="Environnement de développement pour la démo"

oc new-project demo-staging \
  --display-name="Démo Staging" \
  --description="Environnement de staging"

oc new-project demo-prod \
  --display-name="Démo Production" \
  --description="Environnement de production"

# Lister les projets
oc get projects
```

### 2. Créer des utilisateurs (HTPasswd)

```bash
# Créer le fichier HTPasswd
htpasswd -c -B -b /tmp/users.htpasswd alice password123
htpasswd -B -b /tmp/users.htpasswd bob password123
htpasswd -B -b /tmp/users.htpasswd charlie password123

# Créer le secret
oc create secret generic htpasswd-secret \
  --from-file=htpasswd=/tmp/users.htpasswd \
  -n openshift-config

# Vérifier
oc get secret htpasswd-secret -n openshift-config
```

### 3. Attribuer des rôles

```bash
# Alice : admin du projet dev
oc adm policy add-role-to-user admin alice -n demo-dev

# Bob : éditeur du projet dev
oc adm policy add-role-to-user edit bob -n demo-dev

# Charlie : lecteur seul sur tous les projets
oc adm policy add-role-to-user view charlie -n demo-dev
oc adm policy add-role-to-user view charlie -n demo-staging
oc adm policy add-role-to-user view charlie -n demo-prod
```

### 4. Vérifier les permissions

```bash
# Qui peut faire quoi ?
oc adm policy who-can create pods -n demo-dev
oc adm policy who-can delete pods -n demo-dev
oc adm policy who-can get pods -n demo-prod

# Tester en tant qu'utilisateur
oc login -u alice -p password123 https://api.crc.testing:6443
oc auth can-i create deployments -n demo-dev    # yes
oc auth can-i create deployments -n demo-prod   # no

oc login -u charlie -p password123 https://api.crc.testing:6443
oc auth can-i get pods -n demo-dev              # yes
oc auth can-i create pods -n demo-dev           # no
```

### 5. Appliquer un quota

```bash
oc login -u kubeadmin -p <password> https://api.crc.testing:6443

cat <<EOF | oc apply -f - -n demo-dev
apiVersion: v1
kind: ResourceQuota
metadata:
  name: quota-dev
spec:
  hard:
    pods: "10"
    requests.cpu: "2"
    requests.memory: 4Gi
    limits.cpu: "4"
    limits.memory: 8Gi
    persistentvolumeclaims: "5"
EOF

# Vérifier
oc describe resourcequota quota-dev -n demo-dev
```

### 6. Appliquer un LimitRange

```bash
cat <<EOF | oc apply -f - -n demo-dev
apiVersion: v1
kind: LimitRange
metadata:
  name: limits-dev
spec:
  limits:
    - type: Container
      default:
        cpu: "250m"
        memory: "256Mi"
      defaultRequest:
        cpu: "100m"
        memory: "128Mi"
      max:
        cpu: "1"
        memory: "1Gi"
      min:
        cpu: "50m"
        memory: "64Mi"
EOF

oc describe limitrange limits-dev -n demo-dev
```

## Nettoyage

```bash
oc delete project demo-dev demo-staging demo-prod
```

## Points à souligner

- Les projets permettent l'isolation multi-tenant
- Le RBAC est additif (pas de deny explicite)
- Les quotas protègent le cluster contre la surconsommation
- Les LimitRange imposent des valeurs par défaut aux conteneurs
