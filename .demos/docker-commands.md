# Commandes Docker

## Commandes de base

* Lister les conteneurs:

```bash
docker ps

# OU

docker container ls
```

* Lister les conteneurs (avec ceux stoppés):

```bash
docker ps -a
```

* Lister les images:

```bash
docker images

# OU

docker image ls
```

* Rechercher parmi les images:

```bash
docker search <input>
```

Un nom d'image est contitué de trois sections:
```text
<registry-url>/<registry-name>/<image-name>
```

Dans le cas où les images proviennent de DockerHub, il y aura plusieurs raccourcis pour les noms d'image: 

* Dans le cas général:
```text
docker.io/<registry-name>/<image-name> => <registry-name>/<image-name>
```

* Si l'image est une image officielle:
```text
docker.io/library/<image-name> => <image-name>
```

* Récupérer une image:

```bash
docker pull <registry-url>/<registry-name>/<image-name>
```

* Sauvegarder l'état d'un conteneur en une nouvelle image:

```bash
docker commit <container-name|container-id> <registry-url>/<registry-name>/<image-name>
```

* Compiler une nouvelle image:

```bash
docker build -t <registry-url>/<registry-name>/<image-name> <context-path>
```

* Inspecter une image:

```bash
docker inspect <registry-url>/<registry-name>/<image-name>
```

* Inspecter un conteneur:

```bash
docker inspect <container-name|container-id>
```

* Créer un conteneur à partir d'une image :

```bash
docker run <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (mode intéractif avec profil TTY) :

```bash
docker run -it <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (spécifier le nom du conteneur) :

```bash
docker run --name <container-name> <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (mode détaché) :

```bash
docker run -d <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (redirection de ports) :

```bash
docker run -p <host-port>:<container-port> <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (volume anonyme) :

```bash
docker run -v /file/path/in/container <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (volume nommé) :

```bash
docker run -v volume-name:/file/path/in/container <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (bind mount) :

```bash
docker run -v /file/path/in/host:/file/path/in/container <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (bind mount read only) :

```bash
docker run -v /file/path/in/host:/file/path/in/container:ro <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (avec variable d'environnement) :

```bash
docker run -e "ENV_VARIABLE_NAME=value" <registry-url>/<registry-name>/<image-name>
```

* Créer un conteneur à partir d'une image (avec réseau virtualisé par Docker) :

```bash
docker run --network <network-name> <registry-url>/<registry-name>/<image-name>
```

* Executer une commande au sein d'un conteneur lancé :

```bash
docker exec <container-name|container-id> <command>
```

* Executer une commande au sein d'un conteneur lancé (interactive) :

```bash
docker exec -it <container-name|container-id> <command>
```

* Arrêter un conteneur :

```bash
docker stop <container-name|container-id>
```

* Supprimer un conteneur :

```bash
docker rm <container-name|container-id>
```

* Supprimer un conteneur (sans avoir besoin de le stopper en amont):

```bash
docker rm -f <container-name|container-id>
```

* Lister les ressources docker:

```bash
docker <resource-type> ls
```

* Créer une nouvelle ressource docker:

```bash
docker <resource-type> create <resource-name> [OPTIONS...]
```

* Supprimer toutes les ressources d'un certain type :

```bash
docker <resource-type> prune
```