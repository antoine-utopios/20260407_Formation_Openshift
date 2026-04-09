# Démo 07 — Migration d'une application vers OpenShift

## Objectif

Démontrer le processus complet de migration d'une application traditionnelle vers OpenShift.

## Scénario

Une application Flask (Python) qui tourne sur un serveur traditionnel doit être migrée vers OpenShift.

## Étapes de la démonstration

### 1. L'application originale

```bash
mkdir /tmp/migration-demo && cd /tmp/migration-demo

# Application Flask
cat > app.py << 'PYEOF'
from flask import Flask, jsonify
import os
import socket

app = Flask(__name__)

@app.route('/')
def home():
    return jsonify({
        "app": "Demo Migration",
        "version": os.environ.get("APP_VERSION", "1.0"),
        "hostname": socket.gethostname(),
        "environment": os.environ.get("APP_ENV", "development"),
        "database": os.environ.get("DB_HOST", "non configuré")
    })

@app.route('/healthz')
def health():
    return jsonify({"status": "healthy"}), 200

@app.route('/ready')
def ready():
    return jsonify({"status": "ready"}), 200

if __name__ == '__main__':
    port = int(os.environ.get('PORT', '8080'))
    app.run(host='0.0.0.0', port=port)
PYEOF

cat > requirements.txt << 'EOF'
flask==3.0.0
gunicorn==21.2.0
EOF
```

### 2. Créer le Dockerfile optimisé pour OpenShift

```bash
cat > Dockerfile << 'DEOF'
FROM registry.access.redhat.com/ubi8/python-39:latest

WORKDIR /opt/app-root/src

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

# Utilisateur non-root (obligatoire sur OpenShift)
USER 1001

EXPOSE 8080

ENV PORT=8080 \
    APP_ENV=production \
    APP_VERSION=2.0

CMD ["gunicorn", "--bind", "0.0.0.0:8080", "--workers", "4", "app:app"]
DEOF
```

### 3. Déployer sur OpenShift

```bash
oc new-project demo-migration

# Build
oc new-build --strategy=docker --name=migrated-app --binary
oc start-build migrated-app --from-dir=. --follow

# Déployer
oc new-app migrated-app --name=migrated-app

# Configurer
oc create configmap app-config \
  --from-literal=APP_ENV=production \
  --from-literal=DB_HOST=postgresql.demo-migration.svc.cluster.local

oc set env deployment/migrated-app --from=configmap/app-config

# Exposer
oc expose svc/migrated-app

# Health checks
oc set probe deployment/migrated-app \
  --liveness --get-url=http://:8080/healthz \
  --initial-delay-seconds=10 --period-seconds=10

oc set probe deployment/migrated-app \
  --readiness --get-url=http://:8080/ready \
  --initial-delay-seconds=5 --period-seconds=5

# Tester
curl http://$(oc get route migrated-app -o jsonpath='{.spec.host}')
```

### 4. Ajouter l'autoscaling

```bash
oc autoscale deployment/migrated-app --min=2 --max=5 --cpu-percent=70
oc get hpa
```

## Nettoyage

```bash
oc delete project demo-migration
```

## Points à souligner

- Migration en 4 étapes : conteneuriser, configurer, déployer, surveiller
- Gunicorn remplace le serveur de dev Flask
- La configuration est externalisée via ConfigMaps
- Les health checks assurent la fiabilité en production
