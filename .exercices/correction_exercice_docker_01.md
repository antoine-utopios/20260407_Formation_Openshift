# Exercice Docker #1 - Réalisation d'un conteneur de serveur web

## Sujet

Via l'utilisation de Docker Desktop, de la documentation de NGINX et de son image officielle, réaliser le déploiement, dans un environnement Docker:

* D'un conteneur de type NGINX
* Possédant une page d'accueil personnalisée
* Dont on a vérifié le fonctionnement via la commande `curl` en dehors de l'environnement Docker (Par exemple `curl http://localhost:8080/` depuis notre machine hôte)

## Correction

Récupérer l'image NGINX

```bash
docker search nginx

docker pull docker.io/library/nginx:latest
```

Lancer un conteneur avec l'image que l'on vient de récupérer. Pour le rendre disponible à l'url `http://localhost:8080/`, on va devoir faire une redirection des ports de `8080` vers `80`.

```bash
docker run -p 8080:80 --name exercice-02-webserver -d -v "$(pwd)/exo-02-website:/usr/share/nginx/html:ro" nginx:latest

curl http://localhost:8080/
```