# Exercice Docker #2 - Réalisation d'un déploiement de base de données

## Sujet

Via l'utilisation de Docker Desktop et de vos connaissances Docker, réaliser le déploiement, dans un environnement Docker:

* D'un conteneur permettant de posséder une base de données de type MySQL
  * Ayant une base de données par défaut se nommant `testDB`
  * Utilisant un compte utilisateur autre que `root` et configuré avec un mot de passe particulier
  * Ne possédant pas d'accès `root` (pas de mot de passe pour l'utilisateur racine)
  * Dont les données seront persistantes et disponible dans de futurs conteneurs en cas de relance manuelle

Pour cela, il faudra: 
* Déployer le conteneur
* Exécuter des commandes de bases telles que: 
  * `USE testDB;` => Pour sélectionner la base de données en cours d'utilisation
  * `CREATE TABLE IF NOT EXISTS clients (client_id INT AUTO_INCREMENT PRIMARY KEY, client_name VARCHAR(200) NOT NULL, client_email VARCHAR(200) UNIQUE NOT NULL);` => Pour créer si non pré-existante la table des clients
  * `INSERT INTO clients (client_name, client_email) VALUES ("John DOE", "j.doe@example.com"), ("Johnny JOESTAR", "j.joestar@example.com"), ("Dio BRANDO", "d.brando@example.com");` => Pour entrer des données dans la table des clients
  * `SELECT * FROM clients;` => Pour afficher les clients stockés
* Sortir du conteneur puis le stopper et le supprimer
* Recréer un nouveau conteneur permettant d'avoir le même jeu de données et la même configuration

## Correction

Récupérer l'image de MySQL

```bash
docker search mysql

docker pull docker.io/library/mysql:latest
```

Déployer un conteneur de MySQL avec les bonnes variables d'environnement, un volume nommé pour la persistance et un nom pour le retrouver plus facilement

```bash
docker run \
  -d \
  --name exercice-03-database \
  -e MYSQL_DATABASE=testDB \
  -e MYSQL_USER=user \
  -e MYSQL_PASSWORD=password \
  -e MYSQL_ALLOW_EMPTY_PASSWORD=true \
  -v exercice-03-data:/var/lib/mysql \
  mysql
```

Entrer dans le conteneur et se connecter à MySQL

```bask
docker exec -it exercice-03-database bash

mysql -u user -p
```

Réaliser les commandes SQL pour peupler la base de données

```sql
USE testDB;
CREATE TABLE IF NOT EXISTS clients (client_id INT AUTO_INCREMENT PRIMARY KEY, client_name VARCHAR(200) NOT NULL, client_email VARCHAR(200) UNIQUE NOT NULL);
INSERT INTO clients (client_name, client_email) VALUES ("John DOE", "j.doe@example.com"), ("Johnny JOESTAR", "j.joestar@example.com"), ("Dio BRANDO", "d.brando@example.com");
SELECT * FROM clients;
```

Sortir de la base de données et du conteneur

```sql
exit;
```

```bash
exit
```

Supprimer le conteneur

```bash
docker rm -f exercice-03-database
```

Recréer le conteneur et s'y connecter

```bash
docker run \
  -d \
  --name exercice-03-database-bis \
  -v exercice-03-data:/var/lib/mysql \
  mysql

docker exec -it exercice-03-database-bis bash

mysql -u user -p
```

Vérifier que les données sont bien présentes

```sql
SELECT * FROM testDB.clients;
```
