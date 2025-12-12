<!--
title: "Utiliser le terminal dans MacOS : Essentiels et Commandes avancées"
author: "rsquaredata"
last updated: 2025-12-12
-->

# Utiliser le Terminal

Ces commandes fonctionnent dans MacOS, des adaptations peuvent être nécessaires sous Windows.

---

## 1. Essentiels de survie dans le terminal

Section regroupant les **réflexes fondamentaux** à avoir avant toute autre action dans le terminal.  
Socle **mental et technique** pour arrêter d'être perdu·e et reprendre le contrôle.

### 🟢 Basique - Réflexes immédiats

```{bash}
pwd             # affiche le dossier courant
whoami          # savoir quel utilisateur exécute les commandes
clear           # nettoyer l’écran du terminal
```

**Raccourcis** :
- `Ctrl` + `C` : interrompre une commande en cours
- `Ctrl` + `D` : quitter
- `↑` / `↓` : historique
- `⇥` (Tab) : autocomplétion
- `Cmd` / `⌘` + `K` : nettoyer le terminal

### 🟠 Avancé - Reprendre le contrôle

```{bash}
history       # afficher l’historique des commandes tapées
which python  # identifier quel binaire Python est réellement utilisé
echo $PATH    # afficher les chemins où le shell cherche les commandes
type ls       # savoir si une commande est un alias, builtin ou exécutable
```

**Réflexes à acquérir** :
- si "c'est pas le bon Python" → `which python`
- si une commande "n'existe pas" → vérifier le `PATH`
- toujours comprendre **quelle version** d'un outil est utilisée

### 🔴 Expert - Discipline et sécurité

```{bash}
set -o noclobber   # empêche l’écrasement accidentel de fichiers avec >
alias rm='rm -i'   # demande confirmation avant suppression
```

**Règles non négociables** :
- je ne lance jamais une commande que je ne comprends pas
- toujours vérifier `pwd` avant un `rm -r`
- tester les commandes sur des fichiers non critiques
- préférer copier plutôt que supprimer

⸻

## 2. Navigation & fichiers

### 🟢 Basique - Se repérer et se déplacer


```{bash}
pwd                # affiche le chemin absolu du dossier courant
ls                 # liste les fichiers et dossiers du répertoire courant
ls -l              # liste détaillée (droits, taille, date)
ls -a              # inclut les fichiers cachés (commençant par .)
cd dossier         # se déplacer dans un dossier
cd ..              # remonter au dossier parent
cd ~               # aller dans le dossier personnel (home)
cd -               # revenir au dossier précédent
```

### 🟠 Avancé - Manipuler fichiers et dossiers

```{bash}
ls -lh                 # liste détaillée avec tailles lisibles (Ko, Mo, Go)
ls *.csv               # liste uniquement les fichiers correspondant au motif
mkdir data             # créer un dossier nommé data
mkdir -p a/b/c         # créer une arborescence complète si elle n’existe pas
cp file.txt copy.txt   # copier un fichier
cp -r src/ dst/        # copier un dossier récursivement
mv old.txt new.txt     # renommer un fichier
mv file.txt dir/       # déplacer un fichier dans un dossier
rm file.txt            # supprimer un fichier (irréversible)
rm -r dossier          # supprimer un dossier et son contenu (⚠️)
```

### 🔴 Expert - Métadonnées et permissions

```{bash}
stat file.txt          # afficher toutes les métadonnées d’un fichier
open .                 # ouvrir le dossier courant dans le Finder
open file.pdf          # ouvrir un fichier avec l’application par défaut
open -a "VS Code" .    # ouvrir le dossier courant dans VS Code
realpath file.txt      # obtenir le chemin absolu réel d’un fichier
ln -s source target    # créer un lien symbolique (raccourci intelligent)
chown user file.txt    # changer le propriétaire d’un fichier (admin)
chmod 755 script.sh    # changer les permissions (exécutable)
```

⸻

## 3. Recherche & inspection

### 🟢 Basique - Lire et inspecter

```{bash}
cat file.txt           # affiche tout le contenu d’un fichier (à éviter si gros fichier)
less file.txt          # affiche le fichier page par page (q pour quitter)
head file.txt          # affiche les 10 premières lignes du fichier
tail file.txt          # affiche les 10 dernières lignes du fichier
wc -l file.txt         # compte le nombre de lignes dans le fichier
```

### 🟠 Avancé - Rechercher efficacement

```{bash}
grep "mot" file.txt            # cherche les lignes contenant "mot" dans le fichier
grep -i "mot" file.txt         # recherche insensible à la casse (Mot = mot)
grep -n "mot" file.txt         # affiche aussi le numéro de ligne
grep -R "mot" dossier/         # recherche récursive dans tous les fichiers d’un dossier
find . -name "*.csv"           # trouve tous les fichiers .csv à partir du dossier courant
find . -type d -name "data"    # trouve tous les dossiers nommés data
```

### 🔴 Expert - Debug et inspection système

```{bash}
grep -R "pattern" . | less    # recherche récursive + affichage paginé
ps aux | grep python          # trouve les processus Python en cours
lsof -i :8501                 # identifie quel programme utilise le port 8501
file file.txt                 # détecte le type réel d’un fichier
watch -n 1 tail file.txt      # rafraîchit automatiquement la sortie toutes les secondes
```

⸻

## 4. Processus & système

### 🟢 Basique - Voir ce qui tourne

```{bash}
top                     # affiche en temps réel les processus (CPU, mémoire)
ps                      # affiche les processus lancés dans le terminal courant
ps aux                  # affiche TOUS les processus du système
whoami                  # affiche l’utilisateur courant
uname -a                # informations sur le système (OS, kernel)
```

### 🟠 Avancé - identifier et arrêter des processus

```{bash}
ps aux | grep python     # trouve les processus Python en cours
kill PID                 # termine proprement un processus par son identifiant
kill -9 PID              # force l’arrêt immédiat (à utiliser en dernier recours si kill PID ne suffit pas)
htop                     # version améliorée et interactive de top (si installée)
free -h                  # mémoire disponible (Linux ; sur macOS utiliser vm_stat)
vm_stat                  # statistiques mémoire détaillées macOS
```

### 🔴 Expert - Surveillance et intervention système

```{bash}
lsof -i                            # liste les connexions réseau ouvertes
lsof -i :8501                      # identifie le programme utilisant le port 8501
watch -n 1 "ps aux | grep python"  # surveille un processus en temps réel
uptime                             # durée de fonctionnement + charge moyenne
sudo reboot                        # redémarre la machine (droits admin requis)
```

⸻

## 5. Réseau & web

### 🟢 Basique - Vérifier la connectivité

```{bash}
ping google.com         # teste la connectivité réseau vers un hôte (Ctrl+C pour arrêter)
ifconfig                # affiche les interfaces réseau et adresses IP (macOS)
ipconfig getifaddr en0  # affiche l’adresse IP locale (WiFi)
netstat -an             # liste les connexions réseau et ports (vue brute)
```

### 🟠 Avancé - Télécharger et inspecter le web

```{bash}
curl https://example.com             # récupère le contenu d’une page web (HTML brut)
curl -I https://example.com          # affiche uniquement les en-têtes HTTP (HEAD)
curl -O https://site.com/file.zip    # télécharge un fichier en conservant son nom
wget https://site.com/file.zip       # télécharge un fichier (si wget est installé)
traceroute google.com                # montre le chemin réseau jusqu’à l’hôte
```

### 🔴 Expert - Débug réseau et API

```{bash}
lsof -i                              # liste toutes les connexions réseau ouvertes
lsof -i :8080                        # identifie le programme utilisant le port 8080
curl -X POST \
  -H "Content-Type: application/json" \
  -d '{"key":"value"}' https://api.example.com   # appel API POST avec payload JSON
nc -vz localhost 8501                # teste si un port local est ouvert (Streamlit, API)
```

**Réflexes à ancrer** :
- si une app "ne répond pas" → `lsof - i :PORT`
- si une API "ne marche pas" -> `curl` avant d'ouvrir un navigateur

⸻

## 6. Git

### 🟢 Basique - Cycle de travail quotidien

```{bash}
git status               # affiche l’état du dépôt (fichiers modifiés, suivis ou non)
git add file.txt         # ajoute un fichier précis à l’index (staging area)
git add .                # ajoute tous les fichiers modifiés à l’index
git commit -m "message"  # crée un commit avec un message descriptif
git push                 # envoie les commits vers le dépôt distant (GitHub)
```

### 🟠 Avancé - Branches et historiques

```{bash}
git branch                 # liste les branches locales
git checkout -b feature-x  # crée et bascule sur une nouvelle branche
git switch main            # change de branche (syntaxe moderne)
git pull                   # récupère et fusionne les changements distants
git log --oneline --graph  # affiche l’historique de manière compacte et lisible
git stash                  # met de côté les modifications non commitées
git stash pop              # restaure les modifications mises de côté
```

### 🔴 Expert - Réécriture et récupération

```{bash}
git rebase -i HEAD~5       # réécrit les 5 derniers commits (nettoyage avant push), outil quali, hésite pas à l'utiliser
git cherry-pick <commit>   # applique un commit précis sur la branche courante
git reflog                 # historique interne Git (sauvetage après erreur)
git reset --hard HEAD      # annule toutes les modifications locales (⚠️ destructif), si tu ne sais pas tu ne le fais pas
git clean -fd              # supprime les fichiers/dossiers non suivis (⚠️)
```

⸻

## 7. Environnements Python — Conda

**Règles d'or Conda** :
1. un projet = un environnement → jamais tout dans `base`.
2. toujours exporter l'environnement (`environment.yml`)

### 🟢 Basique - Créer et activer des environnements

```{bash}
conda --version                  # affiche la version de conda installée
conda info                       # informations générales sur l’installation conda
conda env list                   # liste tous les environnements conda disponibles
conda create -n env python=3.10  # crée un environnement nommé env avec Python 3.10
conda activate env               # active l’environnement env
conda deactivate                 # désactive l’environnement courant
```

### 🟠 Avancé - Gérer les dépendances et reproductibilité

```{bash}
conda install numpy pandas          # installe des packages dans l’environnement actif
conda install -n env scipy          # installe un package dans un env sans l’activer
conda remove pandas                 # supprime un package de l’environnement actif
conda list                          # liste les packages installés dans l’env courant
conda update conda                  # met à jour conda lui-même
conda env export > environment.yml  # exporte l’environnement (reproductibilité)
conda env create -f environment.yml # recrée un env à partir d’un fichier
```

### 🔴 Expert - Nettoyage et configuration fine

```{bash}
conda clean --all                        # nettoie caches, tarballs et index (libère de l’espace disque)
conda remove -n old_env --all            # supprime complètement un environnement
conda config --show channels             # affiche les canaux configurés
conda config --add channels conda-forge  # ajoute conda-forge (canal recommandé)
which python                             # vérifie quel Python est réellement utilisé
```

⸻

## 8. Environnements Python — uv

Conda gère **Python + libs natives + envs** tandis que uv gère **Python + packages Python** → les deux sont complémentaires, pas concurrents.  

### 🟢 Basique - Installer Python et créer un env

```{bash}
uv --version                   # affiche la version de uv installée
uv python list                 # liste les versions de Python disponibles
uv python install 3.12         # installe Python 3.12 localement
uv venv .venv                  # crée un environnement virtuel dans .venv
source .venv/bin/activate      # active l’environnement (macOS / Linux)
```

### 🟠 Avancé - Gestion rapide des dépendances

```{bash}
uv pip install numpy pandas    # installe des packages (remplace pip, beaucoup plus rapide)
uv pip list                    # liste les packages installés
uv pip uninstall pandas        # désinstalle un package
uv pip compile requirements.in -o requirements.txt  # génère un fichier verrouillé
uv pip sync requirements.txt   # synchronise l’env exactement avec requirements.txt
```

### 🔴 Expert - Exécution contrôlée et verrouillage

```{bash}
uv run python script.py        # exécute un script dans l’environnement uv
uv run pytest                  # lance des tests sans activer manuellement l’env
uv pip install -r requirements.txt --no-deps  # installe strictement les versions verrouillées
rm -rf .venv                   # supprime complètement l’environnement (reset propre)
```

⸻

## 9. Python en lignes de commande

### 🟢 Basique - Lancer et vérifier Python

```{bash}
python --version            # affiche la version de Python utilisée
which python                # montre le chemin du binaire Python réellement exécuté
python                      # ouvre l’interpréteur Python interactif
python script.py            # exécute un script Python
exit()                      # quitte l’interpréteur Python
```

### 🟠 Avancé - Exécution avancée et diagnostic

```{bash}
python -m module            # exécute un module Python comme un script
python -c "print('hello')"  # exécute une commande Python directement depuis le shell
python -i script.py         # exécute le script puis reste en mode interactif
python -X faulthandler script.py  # affiche des traces utiles en cas de crash
```

### 🔴 Expert - debug et performance

```{bash}
PYTHONPATH=. python script.py   # ajoute le dossier courant au chemin d’import
python -m pdb script.py         # lance le débogueur Python sur un script
python -O script.py             # exécute le script avec optimisations basiques
python -m timeit "func()"       # mesure le temps d’exécution d’une fonction
```

Quand "ça ne marche pas" :
1. `which python`
2. `python --version`
3. vérifier l'environnement actif

⸻

## 10. Terminal pour la Data Science

### 🟢 Basique - Explorer les données

```{bash}
ls *.csv           # liste les fichiers de données CSV
head data.csv      # affiche les premières lignes d’un dataset
tail data.csv      # affiche les dernières lignes
wc -l data.csv     # compte le nombre d’observations (lignes)
du -sh data.csv    # taille du fichier (utile pour anticiper la RAM)
```

### 🟠 Avancé - Prétraitement et logs

```{bash}
cut -d',' -f1 data.csv                # extrait une colonne (ici la 1e, CSV simple)
sort data.csv | uniq                  # valeurs uniques (approximatif, texte)
grep -v NA data.csv                   # supprime les lignes contenant NA
for f in *.csv; do wc -l "$f"; done   # compter les lignes de plusieurs datasets
python script.py > out.log            # redirige la sortie vers un fichier log
```

### 🔴 Expert - Jobs longs et monitoring

```{bash}
ps aux | grep python           # surveille les scripts Python en cours
lsof -i :8888                  # identifie un notebook Jupyter actif
watch -n 1 nvidia-smi          # surveille le GPU (si disponible)
time python script.py          # mesure le temps total d’exécution
nohup python script.py &       # lance un script long en arrière-plan
```

⸻

## 11. Docker & containers

### 🟢 Basique - Images et conteneurs

```{bash}
docker --version                 # affiche la version de Docker installée
docker images                    # liste les images disponibles localement
docker ps                        # liste les conteneurs en cours d’exécution
docker ps -a                     # liste tous les conteneurs (actifs + arrêtés)
docker pull python:3.10-slim     # télécharge une image depuis Docker Hub
```

### 🟠 Avancé - Cycle de vie et ports

```{bash}
docker run -it python:3.10-slim bash   # lance un conteneur interactif
docker run -p 8501:8501 image          # mappe un port (ex: Streamlit)
docker stop CONTAINER_ID               # arrête un conteneur
docker rm CONTAINER_ID                 # supprime un conteneur arrêté
docker rmi IMAGE_ID                    # supprime une image
```

### 🔴 Expert - Debug, build et nettoyage

```{bash}
docker exec -it CONTAINER_ID bash      # ouvre un shell dans un conteneur en cours
docker logs CONTAINER_ID               # affiche les logs d’un conteneur
docker system df                       # affiche l’espace disque utilisé par Docker
docker system prune                    # nettoie conteneurs/images inutilisés (⚠️)
docker build -t mon_image .            # construit une image depuis un Dockerfile
```

**Attention** :
- `docker system prune` **libère beaucoup d'espace** mais supprime ce qui n'est plus utilisé.
- `docker build`doit toujours partir d'un **Dockerfile propre**.

⸻

## 12. Automatisation & scripting

**Réflexe à avoir** : commande tapée plus de deux fois = candidate immédiate à l'automatisation.

### 🟢 Basique - Lancer des scripts

```{bash}
bash script.sh          # exécute un script bash
chmod +x script.sh      # rend un script exécutable
./script.sh             # exécute un script depuis le dossier courant
history                 # affiche l’historique des commandes
!!                      # rejoue la dernière commande
```

### 🟠 Avancé - Boucles et redirections

```{bash}
for f in *.csv; do echo "$f"; done              # boucle sur des fichiers
for f in *.csv; do python script.py "$f"; done  # applique un script à plusieurs fichiers
commande > out.txt                              # redirige la sortie standard vers un fichier
commande >> out.txt                             # ajoute à un fichier existant
commande 2> err.txt                             # redirige les erreurs
```

### 🔴 Expert - Pipelines et tâches planifiées

```{bash}
commande1 | commande2  # pipe : sortie de commande1 → entrée de commande2
set -e                 # arrête le script à la première erreur
set -u                 # erreur si variable non définie
crontab -e             # planifie des scripts automatiques
nohup ./script.sh &    # exécute un script en arrière-plan persistant
```

⸻

## 13. Serveurs & cloud

### 🟢 Basique - Connexion distante

```{bash}
ssh user@host  # se connecter à un serveur distant en SSH
ssh -V         # affiche la version du client SSH
hostname       # affiche le nom de la machine distante
whoami         # affiche l’utilisateur connecté
exit           # se déconnecte du serveur
```

### 🟠 Avancé - Transfert et synchronisation

```{bash}
ssh -i key.pem user@host            # connexion SSH avec clé privée
scp file.txt user@host:/path        # copie un fichier vers un serveur distant
scp -r dossier user@host:/path      # copie un dossier récursivement
rsync -av dossier/ user@host:/path  # synchronisation efficace (recommandé)
```

### 🔴 Expert - Tunnels et sessions persistantes

```{bash}
ssh -L 8501:localhost:8501 user@host        # tunnel SSH (exposer un service local distant)
ssh -N -f -L 8888:localhost:8888 user@host  # tunnel persistant sans shell
rsync -av --delete src/ dst/                # miroir exact (⚠️ destructif côté destination)
tmux                                        # lance un multiplexeur de sessions (si installé)
```

⸻

## 14. LLM / IA / API

### 🟢 Basique - Authentification et tests API

```{bash}
export OPENAI_API_KEY="sk-xxxx"                # définit la clé API (session courante)
echo $OPENAI_API_KEY                           # vérifie que la clé est bien chargée
curl https://api.openai.com/v1/models \
  -H "Authorization: Bearer $OPENAI_API_KEY"   # test simple de connexion à l’API
```

**Principe clé** : via le terminal, on utilise des **clés API**, pas un login/mot de passe. Les requêtes API **n’apparaissent pas** dans l’historique de ChatGPT.


### 🟠 Avancé - Appels LLM scriptés

```{bash}
# Appel API texte → texte (exemple générique)
curl https://api.openai.com/v1/responses \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "gpt-4.1-mini",
    "input": "Explique ce qu’est un LLM en une phrase"
  }'
```

**Bonnes pratiques** :
- tester une API avec `curl`avant de coder
- logger les prompts/réponses si on veut un historique

### 🔴 Expert - LLM local et exposition API

```{bash}
# Appel LLM local (ex: Ollama)
ollama list                       # liste les modèles locaux
ollama pull mistral               # télécharge un modèle local
ollama run mistral                # lance un chat local en terminal
```

```{bash}
# Exposition d’un LLM local via API
curl http://localhost:11434/api/generate \
  -d '{"model":"mistral","prompt":"Explique PCA"}'
```

```{bash}
# Logging des prompts
python script.py | tee llm.log    # affiche + sauvegarde les réponses
```

**Usage expert** :
- LLM local = gratuit à l'usage, dépend du CPU/GPU
- API cloud = payant mais scalable
- toujours contrôler coûts, log et contexte

⸻

































