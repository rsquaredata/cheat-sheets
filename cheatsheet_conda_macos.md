# Conda Cheat Sheet (macOS / Terminal)

> Utilisation typique avec `conda` (Anaconda/Miniconda) dans le terminal macOS (zsh/bash).  
> Sur Mac, tes environnements et commandes sont les mêmes que sous Linux.

---

## 1. Infos de base

```bash
# Vérifier la version de conda
conda --version

# Aide générale
conda --help

# Infos globales sur l'installation
conda info
```

---

## 2. Gestion des environnements

### Lister les environnements

```bash
conda env list
# ou
conda info --envs
```

### Créer un environnement

```bash
# Exemple : créer un environnement "myenv" avec Python 3.11
conda create -n myenv python=3.11
```

### Activer / désactiver un environnement

```bash
# Activer
conda activate myenv

# Désactiver (revenir à la base)
conda deactivate
```

### Supprimer un environnement

```bash
conda remove -n myenv --all
```

---

## 3. Gestion des paquets

### Chercher un paquet

```bash
conda search numpy
```

### Installer un paquet dans l'environnement actif

```bash
conda install numpy
```

### Installer une version précise

```bash
conda install numpy=1.26
```

### Mettre à jour un paquet

```bash
conda update numpy
```

### Mettre à jour tout l'environnement actif

```bash
conda update --all
```

### Lister les paquets de l'environnement actif

```bash
conda list
```

### Supprimer un paquet

```bash
conda remove numpy
```

---

## 4. Gestion des environnements via fichier YAML

### Exporter l'environnement actif dans un fichier

```bash
# Exporter l'env actif (sans chemins absolus) dans environment.yml
conda env export --from-history > environment.yml
```

### Créer un environnement à partir d’un fichier YAML

```bash
conda env create -f environment.yml
```

### Mettre à jour un environnement à partir d’un fichier YAML

```bash
conda env update -f environment.yml
```

---

## 5. Nettoyage

```bash
# Nettoyer le cache des paquets inutilisés
conda clean --all

# Voir ce qui serait supprimé (dry-run)
conda clean --all --dry-run
```

---

## 6. Config (utile sur macOS)

```bash
# Voir la configuration actuelle
conda config --show

# Ajouter un canal (channel) ex : conda-forge
conda config --add channels conda-forge

# Rendre conda-forge prioritaire
conda config --set channel_priority strict
```

---

## 7. Astuces spécifiques macOS / zsh

### Initialiser conda pour zsh (si `conda activate` ne marche pas)

```bash
conda init zsh
# puis fermer / rouvrir le terminal
```

### Désactiver le chargement automatique de base (optionnel)

```bash
conda config --set auto_activate_base false
```

---

## 8. Exemples rapides

```bash
# Créer un environnement pour un projet
conda create -n mmrclustvar python=3.11

# L'activer
conda activate mmrclustvar

# Installer des libs
conda install r-base r-essentials
conda install numpy pandas scikit-learn

# Exporter pour partager
conda env export --from-history > mmrclustvar_env.yml
```
