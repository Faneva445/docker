#RAKOTOMALALA Faneva Hosana Valimbavaka 
#156/L1A/2024-2025
# docker
#  Introduction à Docker

Docker est une plateforme open source conçue pour automatiser le déploiement d'applications dans des **conteneurs logiciels**. Ces conteneurs permettent d'exécuter des applications de manière isolée, portable et reproductible.

---

##  Pourquoi utiliser Docker ?

-  Isolation : chaque conteneur a son propre environnement.
-  Portabilité : fonctionne de la même manière sur tous les systèmes.
-  Déploiement rapide : facile à partager et déployer.
-  Moins de conflits : pas de "ça marche chez moi mais pas chez toi".

---

##  Concepts de base

| Terme         | Description                                               |
|---------------|-----------------------------------------------------------|
| **Image**     | Modèle de base à partir duquel un conteneur est créé     |
| **Conteneur** | Instance en cours d'exécution d’une image                |
| **Dockerfile**| Fichier texte définissant comment construire une image   |
| **Docker Hub**| Registre public où sont stockées des images Docker       |

---

##  Installation de Docker

###  Sur Windows / Mac

Télécharger Docker Desktop :  
https://www.docker.com/products/docker-desktop/

###  Sur Linux (Ubuntu)

```bash
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```
### Vérifier l’installation
```bash
docker --version
```
### Télécharger une image
```bash
docker pull nom_image
```
### Lister les images téléchargées
```bash
docker images
```
### Exécuter un conteneur
```bash
docker run nom_image
```
### Exécuter en mode interactif
```bash
docker run -it ubuntu bash
```
### Lister les conteneurs
```bash
docker ps         # Affiche les conteneurs en cours d'exécution
docker ps -a      # Affiche tous les conteneurs (même arrêtés)
```
### Supprimer une image ou un conteneur
```bash
docker rm id_conteneur
```
# Utiliser une image de base officielle de Node.js
FROM node:18

# Définir le répertoire de travail dans le conteneur
WORKDIR /app

# Copier les fichiers locaux dans le conteneur
COPY . .

# Installer les dépendances de l'application
RUN npm install

# Spécifier la commande à exécuter lors du lancement du conteneur
CMD ["npm", "start"]

docker rmi nom_image
### Créer une image personnalisée
```bash
docker build -t mon_app .
```
### Lancer un conteneur avec ports et volumes
```bash
docker run -d -p 3000:3000 -v $(pwd):/app mon_app
````
### Nettoyage
```bash
docker system prune -a    # Supprimer tout ce qui n'est pas utilisé
```
### Exécuter deux conteneurs dans le même réseau
```bash
docker run -d --name conteneur1 --network mon_reseau nginx
docker run -d --name conteneur2 --network mon_reseau alpine sleep 3600
```
Volumes Docker

Les volumes servent à garder les données en dehors des conteneurs.
### Créer un volume
```bash
docker volume create mon_volume
```
###  Lister les volumes
```bash
docker volume ls
```
### Utiliser un volume dans un conteneur
```bash
docker run -v mon_volume:/data alpine
```
### Supprimer un volume
```bash
docker volume rm mon_volume
```
## Utilise l'image officielle de Python
FROM python:3.11-slim

## Définir le répertoire de travail
WORKDIR /app

## Copier les fichiers dans le conteneur
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

## Spécifier le port
EXPOSE 5000

## Commande de lancement
CMD ["python", "app.py"]
Astuces utiles
### Entrer dans un conteneur en cours
```bash
docker exec -it nom_conteneur bash
```
### Voir les logs
```bash
docker logs nom_conteneur
```
### Redémarrer automatiquement un conteneur
```bash
docker run --restart always nom_image
```
### Redémarrer tous les conteneurs arrêtés
```bash
docker start $(docker ps -a -q)
```
### Arrêter tous les conteneurs
```bash
docker stop $(docker ps -q)
```
### Supprimer tous les conteneurs arrêtés
```bash
docker rm $(docker ps -a -q)
```
### Supprimer toutes les images non utilisées (dangling)
```bash
docker image prune
```
### Supprimer toutes les images (attention !)
```bash
docker rmi $(docker images -q)
```
### Supprimer tous les volumes non utilisés
```bash
docker volume prune
```
### Sauvegarder une image Docker en fichier .tar
```bash
docker save -o mon_image.tar nom_image
```
### Charger une image depuis un fichier .tar
```bash
docker load -i mon_image.tar
```
### Sauvegarder un conteneur avec ses données

```bash
docker export nom_conteneur > conteneur_backup.tar
```
### Restaurer un conteneur
```bash
cat conteneur_backup.tar | docker import - nom_nouveau
```
### Inspecter un conteneur ou une image
```bash
docker inspect nom_ou_id
```
### Voir l'utilisation des ressources
```bash
docker stats
```
### Voir l’espace disque utilisé par Docker
```bash
docker system df
```
### Nettoyer TOUT (dangereux)
```bash
docker system prune -a --volumes
```
### Rechercher une image depuis le terminal
```bash
docker search nom_image
```
### Modifier une image en ajoutant un fichier (sans Dockerfile)
```bash
docker cp fichier.txt mon_conteneur:/app/fichier.txt
```
### Copier un fichier depuis un conteneur
```bash
docker cp mon_conteneur:/app/log.txt ./log.txt
```
### Créer un alias Bash utile (dans ~/.bashrc ou ~/.zshrc)
```bash
alias dclean="docker system prune -a --volumes -f"
alias dps="docker ps -a"
alias dimg="docker images"
alias dexec="docker exec -it"
```



