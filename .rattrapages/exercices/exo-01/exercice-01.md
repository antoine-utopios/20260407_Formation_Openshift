# Exercice OpenShift #1 - Création d'utilisateurs et affectation de rôles

## Objectifs

Appréhender le déploiement d'image de conteneur Docker provenant de DockerHub dans un cluster Openshift

## Sujet 

Réaliser, au sein d'un cluster Openshift Local (CRC), le déploiement d'une image Docker permettant de jouer au jeu 2048 dans un navigateur. 

* Rechercher une image Docker compatible sur DockerHub

```bash
docker search 2048
```

* Récupérer l'image en local et la tester via l'utilisation des commandes Docker 

```bash
docker pull quchaonet/2048

docker run --name game-2048 -d -p 8080:8080 quchaonet/2048
```

* Créer le compte de service compatible avec l'image Docker que l'on a récupéré

```bash
oc create serviceaccount exo-01-sa -n exo-01

oc adm policy add-scc-to-user anyuid -z exo-01-sa -n exo-01
```

* Créer les fichiers de manifeste pour la mise en place du déploiement dans le cluster

```bash

echo > exo-01-deployment.yaml << 'EOF'

apiVersion: apps/v1
kind: Deployment
metadata:
  name: exo-01-deployment
  labels:
    app.kubernetes.io/name: exo-01-deploy
spec:
  replicas: 3
  selector:
    matchLabels:
      app.kubernetes.io/name: exo-01-pod
  template:
    metadata:
      name: exo-01-pod
      labels:
        app.kubernetes.io/name: exo-01-pod
    spec:
      serviceAccountName: exo-01-sa
      containers:
        - name: exo-01-container
          image: quchaonet/2048
          resources:
            requests:
              memory: "128Mi"
              cpu: "100m"
            limits:
              memory: "512Mi"
              cpu: "250m"
          ports:
            - containerPort: 8080

EOF

echo > exo-01-service.yaml << 'EOF'

apiVersion: v1
kind: Service
metadata:
  name: exo-01-service
spec:
  type: LoadBalancer
  selector:
    app.kubernetes.io/name: exo-01-pod
  ports:
    - port: 8080
      targetPort: 8080

EOF
```
* Déployer l'applicatif via les commandes Openshift du CLI

```bash
oc apply -f .

oc apply -f exo-01-deployment.yaml
oc apply -f exo-01-service.yaml
```

* Tester le déploiement via la création d'un mapping de ports sur notre machine et l'ouverture de `http://localhost:8080` sur le navigateur

```bash
oc port-forward svc/exo-01-service 8080
```