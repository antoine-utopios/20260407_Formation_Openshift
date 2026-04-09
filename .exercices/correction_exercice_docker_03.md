# Exercice Docker #3 - Réalisation d'une communication inter-conteneurs

## Sujet

Via l'utilisation de Docker Desktop et de vos connaissances Docker, réaliser le déploiement, dans un environnement Docker:

* De deux conteneurs:
  * Un conteneur servant de serveur qui se nommera `exo-06-server`
  * Un conteneur servant de client qui se nommera `exo-06-client`
* Ces deux conteneurs devront avoir été créés et initialisés à partir d'une image construite manuellement via l'utilisation des commits Docker et nommée en fonction (par exemple `exo-06-image`)
  * L'image proviendra d'une image de base d'Ubuntu (`ubuntu`)
  * Sur laquelle on aura mit à jour les registres de paquets (`apt update`)
  * Sur laquelle on aura installé la commande `ping` (`apt install`)
* Les deux conteneurs devront communiquer via l'utilisation d'un système de DNS interne propre à leur environnement Docker personnalisé (pensez à utiliser le système de réseaux virtualisés de Docker pour cela)

## Correction

Récupérer une image d'Ubuntu

```bash
docker search ubuntu

docker pull docker.io/library/ubuntu:latest
```

Construire un réseau en amont

```bash
docker network create exercice-04-net
```

Lancer un conteneur d'Ubuntu en intéractif

```bash
docker run -it --network exercice-04-net --name exo-06-server ubuntu
```

Installer `ping` dans le conteneur

```bash
apt update -y
apt install -y iputils-ping
```

Sauvegarder l'image du conteneur en tant que `exo-06-image`

```bash
docker commit exo-06-server exo-06-image
```

Créer un nouveau conteneur servant de client à partir de l'image que l'on vient de créer

```bash
docker run -it --network exercice-04-net --name exo-06-client exo-06-image
```

Tenter de ping le conteneur de serveur via la résolution DNS

```bash
ping exo-06-server
```
