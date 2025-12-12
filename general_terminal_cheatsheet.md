<!--
title: "Utiliser le terminal dans MacOS : Essentiels et Commandes avancées"
author: "rsquaredata"
last updated: 2025-12-12
-→

# Utiliser le Terminal

Ces commandes fonctionnent dans MacOS, des adaptations peuvent être nécessaires sous Windows.

## 1. Bases absolues pour survivre dans le Terminal

#### Où suis-je ?

```bash
pwd    # affiche le dossier courant
```

#### Voir le dossier courant

```bash
ls
ls -l    # détails
ls -a    # fichiers cachés
```

#### Se déplacer

```bash
cd dossier_où_je_veux_aller
cd ..    # dossier parent
cd ~     # home
```

## 2. Fichiers et dossiers

#### Créer

```bash
mkdir nouveau_dossier
touch nouveau_fichier.txt
```

#### Supprimer (⚠️ irréversible)

```bash
rm fichier_à_supprimer.txt
rm -r dossier_à_supprimer
```

## 3. Lire et inspecter des fichiers

#### Afficher le contenu

```bash
cat fichier.txt
less fichier.txt
```

#### Voir le début / la fin

```bash
head fichier.txt
tail fichier.txt
tail -f log.txt
```

`less` permet de scroller (`q` pour quitter)
## 4. Recherche

```bash
head fichier.txt
tail fichier.txt
tail -f log.txt
```









