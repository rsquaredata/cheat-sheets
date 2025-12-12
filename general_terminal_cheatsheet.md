<!--
title: "Utiliser le terminal dans MacOS : Essentiels et Commandes avancées"
author: "rsquaredata"
last updated: 2025-12-12
-->

# Utiliser le Terminal

Ces commandes fonctionnent dans MacOS, des adaptations peuvent être nécessaires sous Windows.

## 1. Essentiels

### 1.1 Bases absolues pour survivre dans le Terminal

#### Où suis-je ?

```{bash}
pwd            # affiche le dossier courant
```

#### Voir le dossier courant

```{bash}
ls
ls -l           # détails
ls -a           # fichiers cachés
```

#### Se déplacer

```{bash}
cd dossier_où_je_veux_aller
cd ..           # dossier parent
cd ~            # home
```

### 1.2. Fichiers et dossiers

#### Créer

```{bash}
mkdir nouveau_dossier
touch nouveau_fichier.txt
```

#### Supprimer (⚠️ irréversible)

```{bash}
rm fichier_à_supprimer.txt
rm -r dossier_à_supprimer
```

### 1.3. Lire et inspecter des fichiers

#### Afficher le contenu

```{bash}
cat fichier.txt
less fichier.txt
```

#### Voir le début / la fin

```{bash}
head fichier.txt
tail fichier.txt
tail -f log.txt
```

- `less` permet de scroller (`q` pour quitter)

### 1.4. Recherche

#### Trouver un fichier

```{bash}
find . -name "*.py"
```

#### Chercher dans des fichiers

```{bash}
grep "mot" fichier.txt
grep -R "pattern" dossier/
```

### 1.5. Processus & système

#### Voir les processus

```{bash}
top
ps aux
```

#### Tuer un processus

```{bash}
kill PID
kill -9 PID
```

#### Espace disque

```{bash}
df -h
du -sh *
```

### 1.6. Réseau & Internet

#### Tester une connexion

```{bash}
ping google.com
```

#### Télécharger

```{bash}
curl https://example.com
curl -O https://site.com/fichier.zip
wget https://site.com/fichier.zip
```

### 1.7. Gestion des apps depuis le terminal

#### Lancer une app MacOS

```{bash}
open .
open fichier.pdf
open -a "Visual Studio Code"
```

#### Installer des apps (Homebrew)

```{bash}
brew install git
brew install python
brew install wget
```

### 1.8. Environnements & dev

#### Python

```{bash}
python --version
python script.py
pip install nom_package
```

#### Conda

```{bash}
conda env list
conda activate mon_env
conda deactivate
```

#### Git

```{bash}
git status
git add .
git commit -m "message de commit"
git push
```

### 1.9. Raccourcis clavier

- `Ctrl` + `C` : arrêter une commande
- `Ctrl` + `D` : quitter
- `↑` / `↓` : historique
- `⇥` (Tab) : autocomplétion
- `Cmd` / `⌘` + `K` : nettoyer le terminal

## 2. Avancé : dev/data/automation

### 2.1. Shell avancé (zsh)

#### Historique & confort

```{bash}
history
!!         # répète la dernière commande
123        # rejoue la commande 123
```

#### Redirections

```{bash}
commande > out.txt    # écrase
commande >> out.txt   # ajoute
commande 2> err.txt   # erreurs
commande &> all.txt   # tout
```

#### Pipes

```{bash}
ls | grep "py"
cat file.txt | wc -1
```

### 2.2. Variables d'environnement

```{bash}
export VAR=value
echo $VAR
```

#### Variables persistantes

Dans ~/.zshrc :

```{bash}
export PATH="$PATH:/mon/chemin/"
```

Recharger :

```{bash}
source ~/.zshrc
```

### 2.3. Git

#### Cycle standard

```{bash}
git status
git add .
git commit -m "message"
git push
```

#### Branches

```{bash}
git branch
git checkout -b nouvelle-branche
git switch main
```

#### Historique

```{bash}
git log --oneline --graph
```

#### Annuler

```{bash}
git restore fichier
git reset --hard HEAD
```

⚠️ `reset --hard` supprime les modifications locales.

### 2.4. Conda & environnements

```{bash}
conda env list
conda create -n env python=3.10
conda activate
conda deactivate
```

#### Export / import

```{bash}
conda env export > environment.yml
conda env create -f envirpnment.yml
```

### 2.5 Python en ligne de commande

```{bash}
python
pytnon script.py
python -m module
```

#### Virtual check

```{bash}
which python
python --version
```

### 2.6. Docker


# Table des matières


⸻

## 1. Essentiels

🟢 Basique

🟠 Avancé

🔴 Expert

⸻

## 2. Navigation & fichiers

🟢 Basique


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

🟠 Avancé

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

🔴 Expert

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

🟢 Basique

```{bash}
cat file.txt           # affiche tout le contenu d’un fichier (à éviter si gros fichier)
less file.txt          # affiche le fichier page par page (q pour quitter)
head file.txt          # affiche les 10 premières lignes du fichier
tail file.txt          # affiche les 10 dernières lignes du fichier
wc -l file.txt         # compte le nombre de lignes dans le fichier
```

🟠 Avancé

```{bash}
grep -R "pattern" . | less    # recherche récursive + affichage paginé
ps aux | grep python          # trouve les processus Python en cours
lsof -i :8501                 # identifie quel programme utilise le port 8501
file file.txt                 # détecte le type réel d’un fichier
watch -n 1 tail file.txt      # rafraîchit automatiquement la sortie toutes les secondes
```

🔴 Expert

```{bash}
grep -R "pattern" . | less    # recherche récursive + affichage paginé
ps aux | grep python          # trouve les processus Python en cours
lsof -i :8501                 # identifie quel programme utilise le port 8501
file file.txt                 # détecte le type réel d’un fichier
watch -n 1 tail file.txt      # rafraîchit automatiquement la sortie toutes les secondes
```

⸻

## 4. Processus & système

🟢 Basique

```{bash}
top                     # affiche en temps réel les processus (CPU, mémoire)
ps                      # affiche les processus lancés dans le terminal courant
ps aux                  # affiche TOUS les processus du système
whoami                  # affiche l’utilisateur courant
uname -a                # informations sur le système (OS, kernel)
```

🟠 Avancé

```{bash}
ps aux | grep python     # trouve les processus Python en cours
kill PID                 # termine proprement un processus par son identifiant
kill -9 PID              # force l’arrêt immédiat (à utiliser en dernier recours si kill PID ne suffit pas)
htop                     # version améliorée et interactive de top (si installée)
free -h                  # mémoire disponible (Linux ; sur macOS utiliser vm_stat)
vm_stat                  # statistiques mémoire détaillées macOS
```

🔴 Expert

```{bash}
lsof -i                            # liste les connexions réseau ouvertes
lsof -i :8501                      # identifie le programme utilisant le port 8501
watch -n 1 "ps aux | grep python"  # surveille un processus en temps réel
uptime                             # durée de fonctionnement + charge moyenne
sudo reboot                        # redémarre la machine (droits admin requis)
```

⸻

## 5. Réseau & web

🟢 Basique

```{bash}
ping google.com         # teste la connectivité réseau vers un hôte (Ctrl+C pour arrêter)
ifconfig                # affiche les interfaces réseau et adresses IP (macOS)
ipconfig getifaddr en0  # affiche l’adresse IP locale (WiFi)
netstat -an             # liste les connexions réseau et ports (vue brute)
```

🟠 Avancé

```{bash}
curl https://example.com             # récupère le contenu d’une page web (HTML brut)
curl -I https://example.com          # affiche uniquement les en-têtes HTTP (HEAD)
curl -O https://site.com/file.zip    # télécharge un fichier en conservant son nom
wget https://site.com/file.zip       # télécharge un fichier (si wget est installé)
traceroute google.com                # montre le chemin réseau jusqu’à l’hôte
```

🔴 Expert

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

🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

## 7. Environnements Python — Conda

🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

## 8. Environnements Python — uv

🟢🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

## 9. Python en ligne de commande

🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

## 10. Terminal pour la Data Science

🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

## 11. Docker & containers

🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

## 12. Automatisation & scripting

🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

## 13. Serveurs & cloud

🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

## 14. LLM / IA / API

🟢 Basique

```{bash}
```

🟠 Avancé

```{bash}
```

🔴 Expert

```{bash}
```

⸻

































