<!--
Title: "Fouille massive - Protocole expérimental et démarche de résolution"
Author: rsquaredata
Last updated: 2025-11-30
-->
# Table des matières

- [1. Phase d'initialisation et d'analyse (Pré-traitement)](#1-phase-dinitialisation-et-danalyse-pré-traitement)
  - [1.1. Caractérisation des données](#11-caractérisation-des-données)
  - [1.2. Définition des ensembles et validation](#12-définition-des-ensembles-et-validation)
    - [Séparation initiale](#séparation-initiale)
    - [Validation croisée (Cross Validation)](#validation-croisée-cross-validation)
- [2. Phase de définition des objectifs et mesures](#2-phase-de-définition-des-objectifs-et-mesures)
  - [2.1. Choix de la métrique de performance](#21-choix-de-la-métrique-de-performance)
- [3. Phase d'application algorithmique et stratégies](#3-phase-dapplication-algorithmique-et-stratégies)
  - [3.1. Stratégies face au Big Data ou à la Haute Dimension](#31-stratégies-face-au-big-data-ou-à-la-haute-dimension)
  - [3.2. Stratégies face au déséquilibre (Imbalanced Learning)](#32-stratégies-face-au-déséquilibre-imbalanced-learning)
    - [3.2.1. Échantillonnage (*Sampling*)](#321-échantillonnage-sampling)
    - [3.2.2. Apprentissage sensible aux coûts (*Cost-Sensitive Learning*)](#322-apprentissage-sensible-aux-coûts-cost-sensitive-learning)
    - [3.2.3. Combinaison de modèles](#323-combinaison-de-modèles)
- [4. Réalisation du protocole expérimental (Démarche en action)](#4-réalisation-du-protocole-expérimental-démarche-en-action)
- [5. Analyse des résultats et conclusions](#5-analyse-des-résultats-et-conclusions)

---

Le protocole vise à exposer une démarche de résolution complète, allant de la préparation des données au choix de l'algorithme, de la fonction de perte (*loss*), de la mesure de performance et des combinaisons de méthodes.

# 1. Phase d'initialisation et d'analyse (Pré-traitement)

Cette phase permet d'identifier les défis posés par les données et de définir les étapes de nettoyage nécessaires.

## 1.1. Caractérisation des données

Il est essentiel d'identifier les caractéristiques clés des jeux de données :
- **Taille des l'échantillon** ($m$) et **Dimension** ($d$) : Nombre d'observations et nombre de descripteurs/variables.
- **Type de problème** :
  - Classification (binaire ou multi-classe) ;
  - Régression ;
  - Apprentissage non supervisé (clustering, détection d'anomalies).
-  **Ratio des classes** : Déterminer si le jeu de données est équilibré ou déséquilibré (*imbalanced learning*).
-  **Types de variables** : Numériques, catégorielles, ordinales, etc., pour anticiper l'encodage (*feature engineering*).
-  **Valeurs Manquantes** : Détection et imputation si nécessaire.

## 1.2. Définition des ensembles et validation

Le jeu de données doit être partitionné pour l'apprentissage et l'évaluation.

### Séparation initiale

Diviser les données en **jeu d'entraînement** (*training set*) pour apprendre les paramètres du modèle, et en **jeu de test** (*test set*) pour évaluer le modèle final sur des données jamais vues.
- Règle standard : 2/3 entraînement, 1/3 test.
- <u>Attention</u> : Si les données sont **temporelles** (transactions), la séparation doit être faite de manière chronologique pour simuler une application réelle.

### Validation croisée (Cross Validation)

Nécessaire pour *tuner* les hyper-paramètres ($\lambda$, $C$, $\gamma$) et choisir le meilleur modèle.

- La méthode du *k-fold* (souvent $k=5$ ou $k=10$) divise l'ensemble d'entraînement en $k$ blocs. À chaque itération, $k−1$ blocs servent à l'apprentissage et 1 bloc sert à la validation.
- L'hyper-paramètre choisi est celui qui donne la meilleure performance moyenne sur les $k$ itération de validation.
- Le modèle final est appris sur tout l'ensemble d'entraînement avec l'hyper-paramètre optimal.

# 2. Phase de définition des objectifs et mesures

Cette étape est cruciale pour que le modèle réponde à l'objectif métier (ex.: minimiser les pertes, maximiser la détection).

## 2.1. Choix de la métrique de performance

L'Accuracy (taux d'erreur) est souvent inappropriée, surtout en cas de données déséquilibrées.

| Contexte | Métriques recommandées | Justification |
|----------|------------------------|---------------|
| **Déséquilibre / Détection de minoritaire** (fraude, anomalie) | **F-mesure** ($F_{\beta}$), **Rappel (Recall)***, **Précision (Precision)**, **AUC ROC**. | La F-mesure est une bonne mesure lorsqu'on se concentre sur le comportement du modèle sur la classe minoritaire, car c'est la moyenne harmonique du Rappel et de la Précision. </br> L'AUC ROC est adaptée aux problématiques de *ranking*. |
| **Objectif économique / Minimisation de perte** (fraude fiscale, bancaire) | **Cost-Sensitive Learning** (implémenté via une matrice de coûts ou fonction de perte personnalisée) | L'objectif est de minimiser l'erreur pondérée par le coût financier de l'erreur (ex.: un faux négatif FN coûte plus cher qu'un faux positif FP) |

# 3. Phase d'application algorithmique et stratégies

Proposer des algorithmes adaptés aux caractéristiques des données et aux contraintes (vitesse, mémoire).

## 3.1. Stratégies face au Big Data ou à la Haute Dimension

- **Si données massives ($m$ grand)** :
  - Privilégier les algorithmes **rapides et parallélisables** comme les forêts aléatoires (*Random Forests*) et les méthodes de **Boosting** (ex.: Gradient Boosting, XGBoost).
  - Si des méthodes à noyaux (comme le SVM Gaussien) sont envisagées, elles doivent être **approximées**, car le calcul de la matrice de similarité $K \in \mathbb{R}^{m \times m} est trop coûteux ($O(m^2)$).
  - **Approximation par _Landmarks_** : Sélectionner $k \lt m$ points de repère (aléatoirement ou via $k$-means) pour réduire le nombre de similarités à calculer.
  - **Approximation par RFF (_Random Fourier Features_)** : Projeter les données dans un espace de dimension $p$ très petite pour apprendre un séparateur linéaire plus rapidement.
- **Si haute dimension ($d$ grand)** :
  - Utiliser la régularisation $L_1$ (Lasso) pour obtenir un modèle parcimonieux (sélection automatique des *features*).
  - En cas de données images (ex.: MNIST, CIFAR10), utiliser des **réseaux de neurones convolutionnels** (CNN) pour l'extraction de *features*.

## 3.2. Stratégies face au déséquilibre (Imbalanced Learning)

Trois approches principales peuvent être utilisées, souvent en combinaison :

### 3.2.1. Échantillonnage (*Sampling*)

Vise à modifier la distribution des classes dans l'ensemble d'entraînement.

| Méthode | Principe| Objectif / Justification |
|-|-|-|
| **Oversampling**</br>**SMOTE** | Génère des exemples synthétiques de la classe minoritaire par interpolation entre les voisins proches. | Ajoute de l'information (contrairement au sur-échantillonnage aléatoire). |
| **Borderline/ADASYN** | Concentre la génération d'exemples sur la frontière de décision ou sur les points les plus difficiles à classer. | Améliore la frontière dans les zones critiques. |
| **Undersampling**</br>**(Tomek Link / ENN)** | Supprime intelligemment les exemples de la classe majoritaire, souvent ceux qui sont proches de la frontière (liens Tomek). | Nettoie la frontière de décision, réduit le temps d'apprentissage (très utile si $m$ est très grand). |

### 3.2.2. Apprentissage sensible aux coûts (*Cost-Sensitive Learning*)

Modifie la fonction de perte pour que l'algorithme accorde plus d'importance aux erreurs de la classe minoritaire (FN).

**Implémentation** : Utiliser la matrice de coût. Dans les librairies (ex.: Python/sklearn), cela se fait souvent avec le paramètre `class_weight='balanced'` ou en définissant des poids spécifiques.

**Justification** : Si l'objectif est de minimiser les pertes financières (par exemple dans la fraude fiscale), on doit donner un poids plus important aux erreurs de type FN (fraudes non détectées), et ce poids peut être fonction du montant de la transaction.

### 3.2.3. Combinaison de modèles

**Approches ensemblistes** : Utiliser Bagging ou Boosting (par exemple Adaboost ou Gradient Boosting).

**Modèles Hybrides** : Combiner par exemple le **clustering** (non supervisé, comme $$k-means ou One-Class SVM) pour identifier des groupes d'anomalies, puis utiliser les résultats pour la classification supervisée.

# 4. Réalisation du protocole expérimental (Démarche en action)

Le protocole doit être reproductible et justifié.

1. **Présentation des méthodes de base** : Commencer par tester des modèles de base (comme SVM linéaire) pour établir un baseline de performance.
2. **Choix des hyper-paramètres et _tuning_** :
   -  Préciser le domaine de valeurs testé pour chaque hyper-paramètre (ex.: $C \in {0.1, 0.5, 1, 2, 4}$ et $\gamma \in {0.01, 0.1, 1, 10}$ pour le SVM gaussien).
   -  Spécifier la méthode d'optimisation (généralement k-fold Cross-Validation).
3. **Expérimentations stratégiques** : Mettre en œuvre les méthodes avancées adaptées au contexte (ex.: si fraude, tester SMOTE + XGBoost, ou Cost-Sensitive SVM).
4. **Enregistrement des résultats** : Présenter les performances (mesures choisies) et le **temps d'apprentissage** (pour les problématiques Big Data) dans un tableau synthétique.

# 5. Analyse des résultats et conclusions

L'analyse ne se limite pas aux chiffres bruts, elle doit mettre en lumière la pertinence des choix effectués.

**Avantages et inconvénients** : Justifier *pourquoi* une méthode fonctionne mieux qu'une autre. Par exemple, si l'Undersampling est utilisé, l'avantage est la réduction du temps de calcul et le nettoyage de la frontière, mais l'inconvénient est le risque de perte d'information.

**Cohérence** : Les résultats doivent être analysés par rapport à l'objectif initial (ex.: si on cherche à minimiser les pertes, le modèle doit être celui qui obtient la meilleure *F-mesure* ou la meilleure performance selon la matrice de coût personnalisée).

**Conclusions et perspectives** : Conclure sur la procédure la plus efficace et proposer des pistes d'amélioration futures.






