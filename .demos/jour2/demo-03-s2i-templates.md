# Démo 03 — Déploiement avec Source-to-Image (S2I)

## Objectif

Démontrer le déploiement automatique d'une application depuis un dépôt Git avec S2I.

## Étapes de la démonstration

### 1. Préparer le projet

```bash
oc new-project demo-s2i --display-name="Démo S2I"
```

### 2. Déployer une application Node.js avec S2I

```bash
# Déploiement depuis GitHub
oc new-app nodejs:18-ubi8~https://github.com/sclorg/nodejs-ex.git \
  --name=nodejs-demo

oc new-app nodejs:20-ubi8~https://github.com/sclorg/nodejs-ex.git \
  --name=nodejs-demo

# Suivre le build en temps réel
oc logs -f bc/nodejs-demo
```

### 3. Observer le processus S2I

```bash
# Voir le BuildConfig créé
oc get bc nodejs-demo -o yaml

# Voir l'ImageStream
oc get is nodejs-demo

# Voir le Build
oc get builds
oc describe build nodejs-demo-1
```

### 4. Exposer l'application

```bash
# Créer la route
oc expose svc/nodejs-demo

# Obtenir l'URL
oc get route nodejs-demo -o jsonpath='{.spec.host}'

# Tester
curl http://$(oc get route nodejs-demo -o jsonpath='{.spec.host}')
```

### 5. Déployer une application Python avec S2I

```bash
oc new-app python:3.9-ubi8~https://github.com/sclorg/django-ex.git \
  --name=django-demo

oc logs -f bc/django-demo
oc expose svc/django-demo
```

### 6. Déclencher un rebuild

```bash
# Rebuild manuel
oc start-build nodejs-demo

# Rebuild avec webhook (montrer la configuration)
oc describe bc/nodejs-demo | grep -A5 Webhook
```

## Points à souligner

- S2I détecte automatiquement le langage
- Le format est `<builder-image>~<repo-git>`
- Les ImageStreams suivent les images de manière déclarative
- Les webhooks permettent le CI/CD automatique

---

# Démo 04 — Templates OpenShift

## Objectif

Démontrer la création et l'utilisation de Templates paramétrables.

## Étapes

### 1. Créer un Template

```bash
cat <<'EOF' > /tmp/webapp-template.yaml
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: webapp-template
  annotations:
    description: "Template pour une application web avec base de données"
    tags: "webapp,nodejs,postgresql"
parameters:
  - name: APP_NAME
    description: "Nom de l'application"
    required: true
  - name: APP_REPLICAS
    description: "Nombre de réplicas"
    value: "2"
  - name: DB_PASSWORD
    description: "Mot de passe de la base"
    generate: expression
    from: "[a-zA-Z0-9]{16}"
objects:
  - apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: ${APP_NAME}
    spec:
      replicas: ${{APP_REPLICAS}}
      selector:
        matchLabels:
          app: ${APP_NAME}
      template:
        metadata:
          labels:
            app: ${APP_NAME}
        spec:
          containers:
            - name: ${APP_NAME}
              image: "bitnami/nginx:latest"
              ports:
                - containerPort: 8080
  - apiVersion: v1
    kind: Service
    metadata:
      name: ${APP_NAME}
    spec:
      selector:
        app: ${APP_NAME}
      ports:
        - port: 8080
          targetPort: 8080
EOF
```

### 2. Charger et utiliser le Template

```bash
# Charger dans le cluster
oc create -f /tmp/webapp-template.yaml

# Voir les paramètres
oc process webapp-template --parameters

# Instancier
oc process webapp-template \
  -p APP_NAME=ma-webapp \
  -p APP_REPLICAS=3 | oc apply -f -

# Vérifier
oc get all -l app=ma-webapp
```

## Nettoyage

```bash
oc delete project demo-s2i
```
