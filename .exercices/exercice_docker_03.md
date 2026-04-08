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