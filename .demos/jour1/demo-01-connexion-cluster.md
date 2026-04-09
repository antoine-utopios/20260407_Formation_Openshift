# Démo 01 — Connexion au cluster OpenShift

## Objectif

Démontrer la connexion au cluster OpenShift via la CLI et la Console Web.

## Prérequis

- OpenShift Local (CRC) installé et démarré
- `oc` CLI installé

## Étapes de la démonstration

### 1. Vérifier l'état du cluster

```bash
crc status
```

Résultat attendu :
```
CRC VM:          Running
OpenShift:       Running (v4.14.x)
RAM Usage:       8192MB / 16384MB
Disk Usage:      25GB / 50GB
```

### 2. Récupérer les identifiants

```bash
crc console --credentials
```

### 3. Connexion en tant que développeur

```bash
oc login -u developer -p developer https://api.crc.testing:6443
oc whoami
oc whoami --show-server
oc whoami --show-console
```

### 4. Connexion en tant qu'administrateur

```bash
oc login -u kubeadmin -p $(crc console --credentials | grep kubeadmin | awk -F"'" '{print $2}') https://api.crc.testing:6443
oc whoami
```

### 5. Explorer le cluster

```bash
oc get nodes
oc get nodes -o wide
oc cluster-info
oc get clusterversion
oc get clusteroperators
```

### 6. Console Web

Ouvrir dans le navigateur :
```bash
crc console
```

Montrer :
- La perspective Administrateur
- La perspective Développeur
- Le basculement entre les deux
- La page de monitoring

### 7. Configuration de l'auto-complétion

```bash
# Bash
source <(oc completion bash)

# Tester : taper "oc get " puis Tab
```

## Points à souligner

- Différence entre `developer` et `kubeadmin`
- La console Web est accessible via une Route
- `oc` est un superset de `kubectl`
- L'auto-complétion accélère considérablement le travail
