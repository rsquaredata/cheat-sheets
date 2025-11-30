<!--
Title: "Concepts fondamentaux de ML - Types d'apprentissage - Supervisé (classification régression)"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Apprentissage supervisé en données massives

L'apprentissage supervisé, englobant la classification et la régression, se distingue clairement des autres types d'apprentissage par la nature des données exploitées et par les objectifs de prédiction.

## 1. Contexte général - Les types d'apprentissage

Le domaine du Machine Learning (ML) se divise principalement en trois grandes branches :
- Apprentissage supervisé (Supervised Learning) : Elle concerne toutes les tâches pour lesquelles l'algorithme dispose d'un **ensemble de données étiquetées** (ou labellisées), c'est-à-dire que pour chaque observation ($X$), la valeur cible ($y$) est connue. L'objectif est d'apprendre une fonction $h$ (ou hypothèse) qui minimise l'erreur sur ces données pour pouvoir généraliser à de nouvelles instances.
- Apprentissage on supervisé (Unsupervised Learning) : Contrairement à l'apprentissage supervisé, les données utilisées ici **ne possèdent pas d'étiquette**. La classification ou la segmentation se fait alors selon la distribution des données. Les applications incluent le *clustering* (segmentation), la détection d'anomalies, la réduction de dimensionnalité (comme l'Analyse en Composantes Principales).
- Apprentissage par renforcement (Reinforcement Learning) : Cette catégorie est distincte des deux précédentes. Elle implique un système qui évolue dans un environnement et doit choisir des actions basées sur un s**ystème de récompenses/punitions**.

## 2. L'Apprentissage supervisé : Classification et Régression

L'apprentissage supervisé lui-même se subdivise en deux grandes catégories, définies par la nature de la variable cible ($Y$) :

### 2.1. Classification

- **Objectif** : Prédire l'**étiquette (classe) **d'une donnée. Il s'agit généralement d'une tâche de classification binaire, où l'étiquette prend des valeurs discrètes, souvent ${−1,+1}$.
- **Applications spécifiques** :
  - Détection d'anomalies et de fraudes (bancaires, fiscales, intrusions).
  - Classification d'images (comme le jeu de données Dogs vs Cats, MNIST ou CIFAR10).
  - Discrimination de spams.
- **Algorithmes étudiés** :
  - **Séparateurs à Vaste Marge (SVM)** : Principalement pour la classification binaire. Le SVM cherche à apprendre un *hyperplan* $h(x) = sign[\langle w,x \rangle+b]$ qui maximise la marge entre les classes.
  - **Régression logistique** : Proche des SVM dans la recherche d'un séparateur linéaire, mais vise à e**stimer la probabilité** qu'un exemple appartienne à une classe donnée.
  - **Méthodes ensemblistes** : Incluent les **arbres de décision**, les **forêts aléatoires** et le **Boosting** (comme Adaboost ou le Gradient Boosting/XGBoost).
  - **Contraintes particulières (Imbalanced Learning)** : Défis de la classification dans des contextes déséquilibrés (où la classe d'intérêt est minoritaire). Dans ce cas, les algorithmes de classification standard qui minimisent le taux d'erreur sont peu adaptés, nécessitant l'utilisation de mesures comme la **F-mesure** ou l'**AUC ROC**.

### 2.2. Régression

- **Objectif** : Prédire une valeur réelle ($Y \in \mathbb{R}$.
- **Applications spécifiques** :
  - Prédiction du prix d'une action ou d'une maison.
  - Analyse de séries temporelles.
  - Pprévisions météorologiques.
  - Estimation de la durée de vie : Un cas étudié dans le contexte de la transplantation d'organes est l'estimation de la probabilité de survie d'un greffon, traité comme un problème de régression.
-  **Algorithmes étudiés** :
   - **Régression linéaire** : Modèle paramétrique classique.
   - **Régression à noyaux (_Kernel Regression_)** : Modèle non paramétrique qui estime la valeur en utilisant la moyenne des valeurs des voisins, pondérée par une fonction noyau.
   - **Arbres de décision et Boosting** : Ces algorithmes existent également en version régression.
- **Métriques** : Les mesures de performance utilisées pour la régression sont typiquement l'**Erreur Quadratique Moyenne (MSE)** et l'**Erreur Absolue Moyenne (MAE)**.

## 3. Défis de l'apprentissage supervisé en contexte de données massives

Le contexte de données massives pousse l'étude des concepts supervisés vers des problématiques d'implémentation et de performance :
- **Contraintes du Big Data** : Les algorithmes supervisés doivent être évalués non seulement sur leur performance (précision, F-mesure, etc.) mais aussi sur leur **temps d'apprentissage** et leur **scalabilité**. Des méthodes puissantes comme les SVM avec noyaux classiques sont jugées très limitées lorsque le nombre de données $m$ est considérable, car elles nécessitent le calcul de $m^2$ similarités et l'inversion d'une matrice $K \in \mathbb{R}^{m \times m}$, ce qui n'est pas réalisable en Big Data.
- **Solutions en classification/régression** : Pour contourner ces limites, on utilise des approximations des méthodes à noyaux (comme les *Landmarks* et les *Random Fourier Features (RFF)*), et on privilégie des méthodes rapides et efficaces comme le **Gradient Boosting** (XGBoost).
