# 🧠 QCM – Conteneurisation, Docker, Kubernetes & OpenShift

👉 **Format** : 1 seule bonne réponse sauf mention contraire

---

## 🔹 Partie 1 — Conteneurs & OpenShift (Jour 1)

### 1. Qu’est-ce qu’un conteneur ?

A. Une machine virtuelle légère
B. Une unité logicielle empaquetée avec ses dépendances 
C. Un système d’exploitation
D. Un hyperviseur

---

### 2. Quelle est une caractéristique clé des conteneurs ?

A. Ils embarquent un OS complet
B. Ils démarrent en minutes
C. Ils partagent le noyau de l’hôte 
D. Ils nécessitent un hyperviseur

---

### 3. Quelle différence principale avec une VM ?

A. Les conteneurs sont plus lourds
B. Les VM partagent le noyau
C. Les conteneurs sont plus rapides à démarrer 
D. Les VM sont plus portables

---

### 4. Kubernetes permet :

A. De créer des VM
B. D’orchestrer des conteneurs 
C. De compiler du code
D. De gérer uniquement Docker

---

### 5. Le “self-healing” signifie :

A. Sauvegarde automatique
B. Redémarrage automatique des conteneurs 
C. Mise à jour du système
D. Monitoring

---

### 6. OpenShift est :

A. Un Docker amélioré
B. Une VM
C. Une plateforme basée sur Kubernetes 
D. Un OS

---

### 7. Une “Route” OpenShift sert à :

A. Stocker des images
B. Exposer un service à l’extérieur 
C. Créer un pod
D. Gérer les logs

---

### 8. Un Pod est :

A. Un cluster
B. Une VM
C. La plus petite unité déployable 
D. Un réseau

---

### 9. Un Service permet :

A. De créer un pod
B. D’exposer un accès réseau stable aux pods 
C. De stocker des images
D. De gérer les utilisateurs

---

### 10. Deployment sert à :

A. Créer des images
B. Gérer le cycle de vie des pods 
C. Configurer le réseau
D. Créer des utilisateurs

---

### 11. ImageStream sert à :

A. Gérer les utilisateurs
B. Suivre des images de conteneurs 
C. Exposer des services
D. Créer des pods

---

### 12. S2I signifie :

A. Source-to-Image 
B. Service-to-Internet
C. System-to-Instance
D. Storage-to-Image

---

### 13. OpenShift inclut :

A. Un registry intégré 
B. Aucun outil CI/CD
C. Aucun monitoring
D. Pas de sécurité

---

### 14. Un projet OpenShift est :

A. Une VM
B. Un namespace avec RBAC 
C. Un pod
D. Un cluster

---

### 15. RBAC sert à :

A. Gérer les ressources CPU
B. Gérer les accès utilisateurs 
C. Gérer les pods
D. Gérer le réseau

---

### 16. Rôle avec accès complet cluster :

A. admin
B. edit
C. cluster-admin 
D. view

---

### 17. SCC sert à :

A. Gérer les logs
B. Définir les permissions des pods 
C. Créer des images
D. Gérer le stockage

---

### 18. LimitRange permet :

A. Limiter les ressources par défaut 
B. Créer des pods
C. Gérer les routes
D. Configurer DNS

---

### 19. NetworkPolicy permet :

A. Créer des volumes
B. Contrôler le trafic réseau entre pods 
C. Gérer les utilisateurs
D. Déployer des apps

---

### 20. CRC est :

A. Un cloud public
B. Un cluster OpenShift local 
C. Un orchestrateur
D. Un registre Docker

---

## 🔹 Partie 2 — Docker

### 21. docker pull sert à :

A. Supprimer une image
B. Télécharger une image 
C. Lancer un conteneur
D. Construire une image

---

### 22. docker run permet :

A. Créer et lancer un conteneur 
B. Supprimer un conteneur
C. Lister les images
D. Créer un réseau

---

### 23. Option -p :

A. Monter un volume
B. Mapper un port 
C. Définir une variable
D. Nommer un conteneur

---

### 24. Option -it :

A. Mode interactif + terminal 
B. Mode réseau
C. Mode sécurisé
D. Mode debug

---

### 25. Option --name :

A. Donner un nom au conteneur 
B. Créer une image
C. Lister les conteneurs
D. Ajouter un volume

---

### 26. Option -e :

A. Définir une variable d’environnement 
B. Exposer un port
C. Monter un volume
D. Créer un réseau

---

### 27. Option -v :

A. Mapper un port
B. Monter un volume 
C. Nommer un conteneur
D. Créer une image

---

### 28. Volume anonyme :

A. A un nom explicite
B. Est automatiquement créé sans nom 
C. Est persistant sur host uniquement
D. Est temporaire uniquement

---

### 29. Volume nommé :

A. Est supprimé automatiquement
B. A un nom explicite et persistant 
C. Ne peut pas être réutilisé
D. Est un fichier

---

### 30. Bind mount :

A. Volume Docker interne
B. Lien direct avec le filesystem hôte 
C. Réseau Docker
D. Image

---

### 31. docker exec sert à :

A. Lancer une image
B. Exécuter une commande dans un conteneur 
C. Supprimer un conteneur
D. Créer un réseau

---

### 32. docker ps affiche :

A. Les images
B. Les conteneurs actifs 
C. Les volumes
D. Les réseaux

---

### 33. docker rm sert à :

A. Supprimer un conteneur 
B. Supprimer une image
C. Lancer un conteneur
D. Créer un volume

---

### 34. Réseau bridge :

A. Réseau physique
B. Réseau Docker par défaut 
C. Réseau externe
D. Réseau Kubernetes

---

### 35. DNS Docker permet :

A. Résoudre les noms de conteneurs entre eux 
B. Gérer les ports
C. Gérer les volumes
D. Créer des images

---

## 🔹 Partie 3 — Kubernetes

### 36. Un Pod :

A. Contient un ou plusieurs conteneurs 
B. Contient uniquement une VM
C. Est un cluster
D. Est un réseau

---

### 37. ReplicaSet sert à :

A. Créer des images
B. Maintenir un nombre de pods identique 
C. Gérer le réseau
D. Gérer les volumes

---

### 38. Deployment permet :

A. Déployer une image
B. Gérer ReplicaSet + mises à jour 
C. Créer un cluster
D. Gérer les logs

---

### 39. ConfigMap sert à :

A. Stocker des données non sensibles 
B. Stocker des mots de passe
C. Créer des pods
D. Gérer le réseau

---

### 40. Différence Deployment vs ReplicaSet :

A. Aucune
B. Deployment gère les mises à jour et versions 
C. ReplicaSet est plus avancé
D. Deployment ne gère pas les pods