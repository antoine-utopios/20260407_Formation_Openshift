# Démo 05 — Gestion des Images et Registre Interne

## Objectif

Démontrer la construction d'images, la gestion des ImageStreams et le registre interne.

## Étapes de la démonstration

### 1. Préparer l'environnement

```bash
oc new-project demo-images --display-name="Démo Images"
```

### 2. Importer une image externe

```bash
# Importer nginx depuis Docker Hub
oc import-image nginx:latest \
  --from=docker.io/library/nginx:latest \
  --confirm

# Voir l'ImageStream créé
oc get is
oc describe is/nginx

# Voir les tags
oc get istag
```

### 3. Créer une application avec un Dockerfile

```bash
# Créer un répertoire de travail
mkdir /tmp/demo-app && cd /tmp/demo-app

# Créer l'application
cat > app.py << 'PYEOF'
from http.server import HTTPServer, SimpleHTTPRequestHandler
import os

class HealthHandler(SimpleHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/healthz':
            self.send_response(200)
            self.send_header('Content-type', 'application/json')
            self.end_headers()
            self.wfile.write(b'{"status": "healthy"}')
        elif self.path == '/':
            self.send_response(200)
            self.send_header('Content-type', 'text/html')
            self.end_headers()
            hostname = os.environ.get('HOSTNAME', 'unknown')
            html = f"""
            <h1>Application Demo OpenShift</h1>
            <p>Hostname: {hostname}</p>
            <p>Version: {os.environ.get('APP_VERSION', '1.0')}</p>
            """
            self.wfile.write(html.encode())
        else:
            self.send_response(404)
            self.end_headers()

if __name__ == '__main__':
    port = int(os.environ.get('PORT', '8080'))
    server = HTTPServer(('0.0.0.0', port), HealthHandler)
    print(f"Serveur démarré sur le port {port}")
    server.serve_forever()
PYEOF

# Créer le Dockerfile
cat > Dockerfile << 'DEOF'
FROM registry.access.redhat.com/ubi8/python-39:latest
WORKDIR /opt/app-root/src
COPY app.py .
USER 1001
EXPOSE 8080
ENV PORT=8080
CMD ["python", "app.py"]
DEOF
```

### 4. Build dans OpenShift

```bash
# Créer le build
oc new-build --strategy=docker --name=demo-app --binary

# Lancer le build depuis le répertoire local
oc start-build demo-app --from-dir=. --follow

# Vérifier l'image dans le registre interne
oc get is demo-app
oc describe is demo-app
```

### 5. Déployer depuis le registre interne

```bash
oc new-app demo-app --name=demo-app
oc expose svc/demo-app
curl http://$(oc get route demo-app -o jsonpath='{.spec.host}')
```

### 6. Pousser une image avec Podman

```bash
# Obtenir l'URL du registre
REGISTRY=$(oc registry info)

# Login
podman login -u $(oc whoami) -p $(oc whoami -t) $REGISTRY

# Tag et push
podman tag demo-app:local $REGISTRY/demo-images/demo-app:v2
podman push $REGISTRY/demo-images/demo-app:v2

# Vérifier
oc get istag | grep demo-app
```

## Points à souligner

- Les ImageStreams découplent les déploiements des registres externes
- Le registre interne est accessible via une Route
- `--binary` permet de builder depuis des fichiers locaux
- Les images UBI sont recommandées pour OpenShift
