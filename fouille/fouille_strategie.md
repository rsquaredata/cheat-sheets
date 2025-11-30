<!--
Title: "Heuristique : Problèmes, solutions et stratégies en fouilles de données"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Table des matières

- [Table des matières](#table-des-matières)
- [1. Problème : Données massives / Big Data ($m$ grand)](#1-problème--données-massives--big-data-m-grand)
  - [1.1. Solution : Méthodes à noyaux approximées (pour SVM non linéaire)](#11-solution--méthodes-à-noyaux-approximées-pour-svm-non-linéaire)
  - [1.2. Solution : Méthodes ensemblistes scalables](#12-solution--méthodes-ensemblistes-scalables)
- [2. Problème : Données déséquilibrées (Imbalanced Learning)](#2-problème--données-déséquilibrées-imbalanced-learning)
  - [2.1. Solution : Adaptation des mesures de performance](#21-solution--adaptation-des-mesures-de-performance)
  - [2.2. Solution : Stratégies d'échantillonnage (*Sampling*)](#22-solution--stratégies-déchantillonnage-sampling)
  - [2.3. Solution : Apprentissage sensible aux coûts (Cost-Sensitive Learning)](#23-solution--apprentissage-sensible-aux-coûts-cost-sensitive-learning)
- [3. Problème : Haute dimension ($d$ grand) ou Surapprentissage](#3-problème--haute-dimension-d-grand-ou-surapprentissage)
  - [3.1. Solution : Régularisation et Réduction de complexité](#31-solution--régularisation-et-réduction-de-complexité)
  - [3.2. Solution : Extraction de *Features* et Modèles non linéaires](#32-solution--extraction-de-features-et-modèles-non-linéaires)
- [4. Stratégie générale : Combinaison de modèles et Protocole expérimental](#4-stratégie-générale--combinaison-de-modèles-et-protocole-expérimental)


---

# 1. Problème : Données massives / Big Data ($m$ grand)

Contexte : Le nombre d'exemples $mm est très grand (ex.: plus d'un million de greffes, millions de transactions). L'apprentissage doit être **rapide et scalable**.

## 1.1. Solution : Méthodes à noyaux approximées (pour SVM non linéaire)

**Méthodes possibles** :
- **Landmarks** (Points de repère).
- **Random Fourier Features (RFF)**.

**Avantages/Inconvénients** :
- *Inconvénient du SVM classique* : L'apprentissage d'un noyau nécessite le calcul de $m^2$ similarités et comporte $m$ paramètres, ce qui est très limité en Big Data. La complexité est généralement $O(m^2)$.
- *Avantage Landmarks* : L'idée est de sélectionner $k \lt m$ points de repère pour **réduire le nombre de similarités** et de paramètres.
- *Avantage RFF* : Approximer le produit interne du noyau par une **projection aléatoire** $u$, ce qui permet d'apprendre un **séparateur linéaire** sur les données projetées, rendant l'apprentissage "beaucoup plus rapide".

**Améliorations possibles** :
- La sélection des *landmarks* peut être faite **aléatoirement** ou via un algorithme comme $k$-means. 
- L'approximation par RFF peut être combinée au **Gradient Boosting** pour apprendre des *landmarks* optimaux de façon itérative.

## 1.2. Solution : Méthodes ensemblistes scalables

**Méthodes possibles** :
- **Gradient Boosting** (ex.: XGBoost).
- **Forêts aléatoires** (Random Forests).

**Avantages/Inconvénients** :
- *Avantage* : Ces algorithmes sont généralement peu gourmands en temps de calcul et **facilement parallélisables**.
- *Avantage XGBoost* :Très efficace en pratique, utilise une approximation d'ordre 2 de la fonction de *loss* et est rapide même sur des données volumineuses.
- *Inconvénient Forêts aléatoires* : Les arbres individuels sont très sensibles aux perturbations (*forte instabilité*).

**Améliorations possibles** :
- Dans les Forêts aléatoires, utiliser un **double échantillonnage** (sur les données et sur les variables) pour créer des modèles suffisamment diversifiés afin d'augmenter la stabilité et la généralisation.
- Utiliser des plateformes distribuées comme **H2O** pour exécuter le *Gradient Boosting* ou d'autres algorithmes sur plusieurs cœurs/processeurs ou *clusters/serveurs*.

# 2. Problème : Données déséquilibrées (Imbalanced Learning)

Contexte : Le nombre d'exemples d'une classe (minoritaire, ex : fraude/anomalie) est très faible par rapport à la classe majoritaire (ex.: 1 % de transactions frauduleuses, 0.28 % de fraudes, 5 % de fraude fiscale).

## 2.1. Solution : Adaptation des mesures de performance

**Mesures** :
- **à éviter** → L'**Accuracy** (taux d'erreur) n'est pas une bonne solution car elle tend à favoriser la classe majoritair.
- **adaptées** → **Précision**, **Rappel** (Recall), **F-mesure** ($F_{\beta}$), et **AUC ROC**.

**Avantages/Inconvénients** :
- *Avantage F-mesure* : Permet de se concentrer sur le comportement du modèle sur la classe minoritaire.
- *Inconvénient F-mesure* : La F-mesure est non-linéaire et non-convexe, ce qui la rend difficile à optimiser directement par des algorithmes de descente de gradient standard.

**Améliorations possibles** :
- Utiliser l'A**UC ROC**, qui est plus adaptée aux problématiques de *ranking* et offre plus d'informations sur la qualité du modèle.

## 2.2. Solution : Stratégies d'échantillonnage (*Sampling*)

**Méthodes possibles** :
- **Oversampling** (Sur-échantillonnage).
- ß (Sous-échantillonnage).

**Avantages/Inconvénients** :
- *Oversampling aléatoir*e : Simple, mais **n'ajoute pas d'information** et peut créer du bruit.
- *Undersampling aléatoire* : Risque de **perte de l'information importante**.

**Améliorations possibles (Sampling intelligent)** :
- **SMOTE** : Générer des exemples **synthétiques** par interpolation entre les voisins pour ajouter de l'information.
- **Borderline SMOTE / ADASYN** : Concentrer la génération d'exemples sur les points plus difficiles à classer ou situés à la frontière de décision.
- **Tomek Link / ENN** : Méthodes d'*Undersampling* qui nettoient les données en retirant des paires d'exemples d'étiquettes différentes situées à la frontière de décision (*Tomek Link*).

## 2.3. Solution : Apprentissage sensible aux coûts (Cost-Sensitive Learning)

**Méthodes possibles** : Utilisation du paramètre `class_weight` ou définition d'une matrice de coût.

**Avantages/Inconvénients** :
- *Avantage* : Permet d'indiquer à l'algorithme que mal classer un exemple majoritaire ($c_{FN}$) a un **coût plus élevé** que mal classer un exemple majoritaire ($c_{FP}$). Cela force le modèle à se focaliser sur la classe minoritaire.

**Améliorations possibles** :
- **Pondération par exemple** : Dans un contexte économique (fraude), il est possible d'attribuer un poids plus important aux fraudes de montant élevé qu'aux petites fraudes pour **minimiser les pertes**. Ceci est directement lié à l'objectif de la DGFIP de minimiser la perte due à la fraude fiscale.

# 3. Problème : Haute dimension ($d$ grand) ou Surapprentissage

Contexte : Le nombre de variables est très grand (ex.: 100 000 descripteurs génomiques, 10 000 attributs numériques). Nécessité de parcimonie et de bonnes capacités de généralisation..

## 3.1. Solution : Régularisation et Réduction de complexité

**Méthodes possibles** : Utilisation de la régularisation dans la formulation du problème d'optimisation.

**Avantages/Inconvénients** :
- *Régularisation $L_1 (\Vert w \Vert_1$)* : Favorise un modèle **parcimonieux** (sparse) en mettant à zéro les poids des variables non pertinentes, ce qui est crucial en haute dimension.
- *Régularisation $L_2 (\Vert w \Vert_2$)* : Vise à **réduire le poids des variables**.

**Améliorations possibles** :
- Utiliser la **théorie de la stabilité uniforme** : Le terme de régularisation $\lambda$ contrôle la complexité du modèle et influence la borne de généralisation. Augmenter $\lambda$ réduit la complexité et améliore la généralisation (réduit la borne). Le tuning de $\lambda$ par **validation croisée** (**_cross-validation_**) est essentiel pour trouver le bon compromis.

## 3.2. Solution : Extraction de *Features* et Modèles non linéaires

**Méthodes possibles** :
- **Réseaux de Neurones Convolutionnels** (CNN).
- **Metric Learning**.

**Avantages/Inconvénients** :
- *Avantage CNN* : Nécessaire pour la classification ou l'extraction de *features* (*feature extraction*) à partir de données images (ex : CIFAR10).
- *Avantage* : Ces méthodes permettent de projeter les données de l'*Input Space* $X$ dans un **espace latent** $Z$ (potentiellement de dimension infinie) où la tâche peut être résolue linéairement.

**Améliorations possibles** :
- Après l'extraction de *features* par CNN, ces nouvelles représentations peuvent être utilisées pour apprendre un modèle de Machine Learning standard.
- Le *Metric Learning* cherche à apprendre une nouvelle représentation des données en projetant les données dans un espace latent (meilleur).

# 4. Stratégie générale : Combinaison de modèles et Protocole expérimental

L'approche ne doit pas se limiter à l'application d'un seul algorithme.

**Solution** : **combinaison de modèles** (Bagging, Boosting) ou **combinaison d'approches** (supervisé + non supervisé).

**Avantages/Inconvénients** :
- *Avantage Combinaison* : Augmente la performance et surtout la stabilité des résultats.
- *Avantage Hybride* : Les méthodes non supervisées (ex.: k-means, One-Class SVM, auto-encodeurs) peuvent servir à détecter les anomalies ou à créer des scores d'anormalité, qui sont ensuite utilisés dans un modèle supervisé.

**Améliorations possibles (Protocole)** :
1. **Définir la mesure** : Toujours commencer par choisir la mesure de performance appropriée (F-mesure, AUC ROC, matrice de coût).
2. **Établir un baseline** : Commencer par un algorithme simple (ex.: SVM linéaire) comme point de référence.
3.  **Expérimentation cohérente** : Tester des versions combinées et complexes (ex.: SVM + sampling + pondération des classes).
4.  **Justification** : **Motiver** le choix des algorithmes et expliquer pourquoi ils sont pertinents pour le contexte du problème (volume de données, déséquilibre, objectif).
5.  **Tuning** : Utiliser la **validation croisée** pour optimiser les hyper-paramètres ($\lambda$, $C$, $\gamma$).
6.  **Évaluation** : Comparer non seulement les performances, mais aussi le **temps d'apprentissage** (pertinent en Big Data).








