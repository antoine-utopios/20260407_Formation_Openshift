# Démo 06 — Monitoring et Troubleshooting

## Objectif

Démontrer les outils de surveillance et les techniques de diagnostic sur OpenShift.

## Étapes de la démonstration

### 1. Préparer l'environnement

```bash
oc new-project demo-monitoring --display-name="Démo Monitoring"

# Déployer une application
oc new-app nodejs:18-ubi8~https://github.com/sclorg/nodejs-ex.git \
  --name=monitored-app
  
oc new-app nodejs:20-ubi8~https://github.com/sclorg/nodejs-ex.git \
  --name=monitored-app
oc expose svc/monitored-app
```

### 2. Monitoring via la CLI

```bash
# Ressources des nœuds
oc adm top nodes

# Ressources des pods
oc adm top pods -n demo-monitoring

# Événements récents
oc get events -n demo-monitoring --sort-by='.lastTimestamp'

# État des pods
oc get pods -o wide
oc describe pod $(oc get pods -o name | head -1)
```

### 3. Logs

```bash
# Logs en temps réel
oc logs -f deployment/monitored-app

# Logs des 30 dernières minutes
oc logs deployment/monitored-app --since=30m

# Logs du build
oc logs -f bc/monitored-app
```

### 4. Console Web — Monitoring

Montrer dans la console :
- **Perspective Admin > Monitoring > Dashboards** : tableaux Grafana
- **Perspective Admin > Monitoring > Metrics** : requêtes PromQL
- **Perspective Admin > Monitoring > Alerts** : alertes actives

### 5. Simuler un problème et diagnostiquer

```bash
# Créer un pod défaillant volontairement
cat <<EOF | oc apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: pod-crash
spec:
  containers:
    - name: crash
      image: busybox
      command: ["sh", "-c", "exit 1"]
EOF

# Observer le CrashLoopBackOff
oc get pods -w

# Diagnostiquer
oc describe pod pod-crash
oc logs pod-crash --previous
oc get events --field-selector involvedObject.name=pod-crash
```

### 6. Debug interactif

```bash
# Ouvrir un shell de debug
oc debug pod/$(oc get pods -l app=monitored-app -o name | head -1 | cut -d/ -f2)

# Tester la connectivité réseau depuis un pod
oc rsh $(oc get pods -l app=monitored-app -o name | head -1)
# Dans le shell :
# curl localhost:8080
# nslookup monitored-app
# exit
```

### 7. Collecte de diagnostics

```bash
# Inspecter un namespace complet
oc adm inspect ns/demo-monitoring --dest-dir=/tmp/diag-demo

# Explorer les résultats
ls /tmp/diag-demo/
```

## Nettoyage

```bash
oc delete project demo-monitoring
```

## Points à souligner

- Prometheus est intégré et collecte automatiquement les métriques
- `oc debug` crée un pod de diagnostic avec les mêmes montages
- `oc adm inspect` collecte toutes les informations pour analyse offline
- Les logs `--previous` sont essentiels pour les CrashLoopBackOff
