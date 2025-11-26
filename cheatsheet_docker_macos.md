# Docker Cheat Sheet (macOS / Terminal)

> Pour Docker Desktop sur macOS.  
> Les commandes sont les mêmes que sous Linux, mais n’oublie pas de lancer **Docker Desktop** avant d’utiliser le terminal.

---

## 1. Vérifications de base

```bash
# Vérifier que Docker est installé et joignable
docker --version
docker info
```

---

## 2. Images

### Lister les images locales

```bash
docker images
```

### Télécharger (pull) une image

```bash
# Exemple : récupérer l'image officielle de RStudio Server Rocker
docker pull rocker/rstudio

# Exemple : image Ubuntu
docker pull ubuntu:22.04
```

### Supprimer une image

```bash
docker rmi IMAGE_ID
```

---

## 3. Conteneurs

### Lister les conteneurs

```bash
# Conteneurs en cours d'exécution
docker ps

# Tous les conteneurs (y compris stoppés)
docker ps -a
```

### Lancer un conteneur

```bash
# Exemple simple (mode interactif) :
docker run -it --name test-ubuntu ubuntu:22.04 bash

# Exemple avec port mapping (RStudio sur port 8787) :
docker run -d   --name rstudio   -p 8787:8787   -e PASSWORD=motdepasse   rocker/rstudio
```

### Arrêter / démarrer / redémarrer un conteneur

```bash
# Arrêter
docker stop CONTAINER_NAME

# Démarrer
docker start CONTAINER_NAME

# Redémarrer
docker restart CONTAINER_NAME
```

### Supprimer un conteneur

```bash
docker rm CONTAINER_NAME
```

### Voir les logs d’un conteneur

```bash
docker logs CONTAINER_NAME

# Suivre les logs en temps réel
docker logs -f CONTAINER_NAME
```

---

## 4. Exécuter des commandes dans un conteneur existant

```bash
# Ouvrir un shell bash dans un conteneur en cours d'exécution
docker exec -it CONTAINER_NAME bash

# Changer le mot de passe pour l'utilisateur rstudio dans un conteneur (exemple)
docker exec -it CONTAINER_NAME passwd rstudio
```

*(C’est ce type de commande que tu utilises pour réinitialiser ton mot de passe RStudio dans le conteneur.)*

---

## 5. Volumes et fichiers (macOS)

### Monter un dossier local dans le conteneur

```bash
# Sur Mac, on monte souvent un dossier du $HOME
docker run -it --name dev-r   -v $HOME/projets:/home/rstudio/projets   rocker/rstudio bash
```

- `-v chemin_local:chemin_dans_conteneur`
- Sur macOS, pense à autoriser Docker à accéder au dossier dans Docker Desktop (Settings > Resources > File Sharing).

### Lister les volumes Docker

```bash
docker volume ls
```

### Supprimer un volume

```bash
docker volume rm VOLUME_NAME
```

---

## 6. Construire une image (Dockerfile)

### Build

```bash
# Dans un dossier où se trouve un fichier Dockerfile
docker build -t mon-image:latest .
```

- `-t` : nom:tag de l’image (par ex. `mon-image:0.1`)

---

## 7. Docker Compose (nouvelle syntaxe)

Sur macOS récent avec Docker Desktop, on utilise :
```bash
docker compose
```
(et plus `docker-compose`).

### Lancer un projet docker compose

```bash
# Dans le dossier où se trouve docker-compose.yml
docker compose up
# Mode détaché
docker compose up -d
```

### Arrêter les services

```bash
docker compose down
```

---

## 8. Nettoyage

```bash
# Supprimer tous les conteneurs stoppés
docker container prune

# Supprimer toutes les images non utilisées
docker image prune

# Tout nettoyer (dangereux, supprime beaucoup de choses)
docker system prune
```

---

## 9. Exemples rapides utiles pour toi

```bash
# Trouver le nom du conteneur RStudio
docker ps -a

# Entrer dans le conteneur pour changer le mot de passe rstudio
docker exec -it NOM_DU_CONTENEUR passwd rstudio

# Relancer RStudio si nécessaire
docker restart NOM_DU_CONTENEUR
```
