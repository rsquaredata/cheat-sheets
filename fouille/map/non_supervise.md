<!--
Title: "Concepts fondamentaux de ML - Types d'apprentissage - Non supervisé (Clustering, Réduction de dimension)"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Apprentissage non supervisé : segmentation, anomalies, réduction

L'apprentissage non supervisé représente l'une des trois grandes branches du *Machine Learning*, aux côtés de l'apprentissage supervisé et de l'apprentissage par renforcement.

Contrairement à l'apprentissage supervisé, qui utilise des données étiquetées, 'apprentissage non supervisé exploite des données **sans étiquette**. La classification ou la segmentation est alors effectuée s**elon la distribution des données**.

## 1. Clustering (Segmentation)

Le *clustering*, ou segmentation, est une application majeure de l'apprentissage non supervisé.

### 1.1. Usage principal

Le *clustering* est principalement utilisé pour :
- La **segmentation de population** dans des stratégies de marketing.
- Les **systèmes de recommandation** basés sur la typologie des clients.
- Son rôle dans un protocole expérimental est d'être un outil permettant d'**extraire de la connaissance des données**.

### 1.2. Algorithmes et leur usage hybride

Bien que le *clustering* soit une méthode non supervisée, les sources mettent en évidence son usage critique en combinaison avec des algorithmes supervisés.

- **$k$-means** : Cet algorithme est explicitement mentionné et est disponible sur des plateformes comme H2O. Son usage le plus notable dans ce contexte est d'aider à l'approximation des méthodes à noyaux pour les Big Data.
  - Dans cette stratégie d'approximation, $k$-means peut être utilisé pour sélectionner les **landmarks** (points de repère), qui constituent un sous-ensemble $k \ lt m$ du jeu de données.
  - L'utilisation de $k$-means pour la sélection de *landmarks* doit être comparée à la sélection aléatoire pour évaluer ses avantages et inconvénients.
- **Autres algorithmes** : On peut utiliser d'autres méthodes non supervisées comme le clustering hiérarchique dans le cadre du projet pour détecter, par exemple, des fraudes.

### 1.2. La détection d'anomalies (One Class SVM et Auto-encodeurs)

La détection d'anomalies est une application clé de l'apprentissage non supervisé.

#### One Class SVM

Le One Class SVM est une **version non supervisée** de l'algorithme des Séparateurs à Vaste Marge (SVM) et est utilisé pour la détection d'anomalies.
- Son objectif est de séparer les individus en deux classes : les exemples "**normaux**", et les exemples "**aberrants**" ou "**anormaux**".
- Le fonctionnement est basé sur le **problème de la sphère minimum** (ou *Support Vector Data Description* - SVDD), visant à trouver la plus petite sphère contenant l'ensemble des points de l'échantillon.
- L'ajout de variables *slacks* ($\xi_i$) au problème d'optimisation (pour la version SVDD) autorise certains points à se trouver en dehors du cercle, et ces points sont alors considérés comme des anomalies.
- Un paramètre $\mu$ dans le One Class SVM peut être réglé pour contrôler la distance maximale autorisée à l'hyperplan et donner une borne supérieure sur le pourcentage d’anomalies dans le jeu de données.

#### Auto-encodeurs

Les **auto-encodeurs** sont également cités comme une méthode non supervisée que l'on peut' employer dans le cadre de la détection de fraude pour identifier des anomalies.

### 1.3. La réduction de dimension

La réduction de dimensionnalité est un autre aspect de l'apprentissage non supervisé. Elle est particulièrement pertinente dans le contexte des données massives (Big Data) caractérisé par un **grand nombre de variables**.

- **Objectif** : Vise à la **compression significative** (*Meaningful Compression*), la **découverte de structure** (*Structure Discovery*) et l'**élicitation de features** (*Feature Elicitation*).
- **Méthodes** : Elle peut être réalisée par des **méthodes statistiques** telles que l'Analyse en Composantes Principales (**PCA**).
- Dans des problèmes de haute dimension (comme les données génomiques ou des jeux de données avec 10 000 attributs numériques), la réduction de dimension peut être un préalable nécessaire pour le pré-traitement des données.

### 1.4. Intégration dans le protocole expérimental

L'apprentissage non supervisé est souvent vu comme **une étape de préparation ou une composante hybride** essentielle du protocole de résolution :
- **Combiner** des méthodes non supervisées (auto-encodeurs ou $k$-means) avec des algorithmes supervisés (SVM, arbres de décision, forêts aléatoires).
- **Utiliser** le *clustering* pour faire de l'approximation des noyaux (via les *landmarks*) afin de rendre les méthodes à noyaux applicables au Big Data.
