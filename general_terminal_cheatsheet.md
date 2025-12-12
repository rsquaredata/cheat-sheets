<!--
title: "Utiliser le terminal dans MacOS : Essentiels et Commandes avancées"
author: "rsquaredata"
last updated: 2025-12-12
-->

# Utiliser le Terminal

Ces commandes fonctionnent dans MacOS, des adaptations peuvent être nécessaires sous Windows.

## 1. Bases absolues pour survivre dans le Terminal

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

## 2. Fichiers et dossiers

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

## 3. Lire et inspecter des fichiers

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

## 4. Recherche

#### Trouver un fichier

```{bash}
find . -name "*.py"
```

#### Chercher dans des fichiers

```{bash}
grep "mot" fichier.txt
grep -R "pattern" dossier/
```

## 5. Processus & système

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

## 6. Réseau & Internet

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

## 7. Gestion des apps depuis le terminal

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

## 8. Environnements & dev

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

## 9. Raccourcis clavier

- `Ctrl` + `C` : arrêter une commande
- `Ctrl` + `D` : quitter
- `↑` / `↓` : historique
- `⇥` (Tab) : autocomplétion
- `Cmd` / `⌘` + `K` : nettoyer le terminal
































