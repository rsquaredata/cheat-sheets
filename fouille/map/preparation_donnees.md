<!--
Title: "Apprentissage massif et données déséquilibrées - Procédures d'apprentissage - Préparation des données"
Author: rsquaredata
Last updated: 2025-12-03
-->

# Préparation des données ML - Protocole et techniques

La **préparation des données** constitue la phase initiale et fondamentale de la *procédure d'apprentissage** en 
*Machine Learning* (ML). C'est une étape cruciale pour garantir que les algorithmes pourront traiter efficacement 
l'information et produire des modèles robustes et généralisables.

Le processus de préparation des données englobe l'étude des caractéristiques du jeu de données, la gestion des 
imperfections (valeurs manquantes) et la structuration des ensembles d'entraînement et de test.

## 1. Étude et identification des caractéristiques des données

La première étape de la préparation consiste à comprendre et caractériser le jeu de données.

- **Identification des caractéristiques** : Il est essentiel d'identifier la **taille** de l'échantillon ($m$ ou nombre 
d'observations), la **dimension** ($d$ ou nombre de variables/descripteurs), le **type de problème** (classification ou 
régression), et surtout le **ratio des classes**.

- **Analyse de la nature des variables** : Les jeux de données réels peuvent comporter des variables numériques, 
ordinales ou catégorielles. Par exemple, dans le cadre d'un projet de détection de fraude, les données peuvent inclure 
des montants de transaction, des dates, des heures, et des variables catégorielles comme le type d'enseigne.

- **Représentation des données** : Bien que les données puissent être initialement sous diverses formes (images, texte, 
données brutes), elles doivent être converties en une **représentation matricielle** (lignes = observations, colonnes = 
variables) pour être traitées par les algorithmes de ML. La qualité de cette représentation est un pilier important de 
l'apprentissage.

## 2. Nettoyage et gestion des données imparfaites

La qualité des données influence directement la performance du modèle.

- **Détection et imputation des valeurs manquantes** : La procédure de préparation doit inclure la détection des 
éventuelles **valeurs manquantes** et leur **imputation**.

- **Transformation des variables** : Dans les problèmes de haute dimension, surtout si les descripteurs sont sur des 
échelles de grandeurs différentes, il est souvent nécessaire de **mettre les données à l'échelle** (*scaling*) ou de 
les normaliser. Cette transformation est recommandée pour les algorithmes basés sur la notion de distance ou de produit 
interne (comme le $k$-Nearest Neighbor ou les SVM).

- **Normalisation (Z-score)** : Transforme les données pour qu'elles suivent une distribution normale centrée et 
réduite ($\mu_j = 0, \sigma_j = 1).

	- **Norme hypercube (Min-Max)** : Décale les valeurs des descripteurs pour qu'elles se situent dans 
l'intervalle $[0,1]$ ou $[-1,1]$.

## 3. Structuration des ensembles d'apprentissage et d'évaluation

La séparation des données est une étape fondamentale du protocole expérimental.

### 3.1. Séparation apprentissage et test

Pour évaluer la capacité de généralisation d'un modèle appris, il est impératif de le tester sur des données qu'il n'a 
pas vues durant l'entraînement.

- **Ensemble d'entraînement (_Training Set_)** : Utilisé pour apprendre les paramètres du modèle.

- **Ensemble de test (_Test Set)** : Utilisé uniquement *a posteriori* pour évaluer la performance finale du modèle, 
garantissant une mesure non biaisée des capacités de généralisation

- **Ratio de division** : Il est courant de conserver **environ les 2/3 des exemples pour l'entraînement** et le reste 
(1/3) pour le test, afin de s'assurer que l'ensemble de test soit suffisamment grand pour que les résultats soient 
statistiquement significatifs.

### 3.2. Gestion de la temporalité

Dans le cas des séries temporelles (comme les transactions bancaires), la procédure d'apprentissage doit respecter 
l'ordre chronologique des événements.

- **Apprentissage fixe** : Les ensembles d'entraînement et de test doivent être définis par des périodes temporelles 
distinctes. Par exemple, les transactions du 2017-02-01 au 2017-08-31 sont utilisées pour l'apprentissage, et celles 
du 2017-09-01 au 2017-11-30 sont utilisées pour le test. Il est crucial de ne pas effectuer de validation croisée 
(*cross-validation*) de manière aléatoire sur des données datées.

## 4. Gestion du déséquilibre des classes

En douille de données massives, les jeux de données sont souvent **déséquilibrés** (par exemple, seulement 0.28% de 
fraudes ou 5% de fraudes fiscales). La préparation des données doit aborder ce problème avant l'apprentissage.

- **Techniques de sampling** : Ces méthodes modifient le ratio des classes dans l'ensemble d'entraînement.

	- **_Oversampling_ (Sur-échantillonnage)** : Augmente le nombre d'exemples de la classe minoritaire, par simple 
duplication (**Random Oversampling**) ou en créant de nouveaux exemples synthétiques. Les algorithmes **SMOTE**, 
Borderline SMOTE et ADASYN sont cités comme des solutions avancées qui se concentrent sur la génération de points dans 
des zones difficiles à classer (proches de la frontière de décision).

	- **_Undersampling_ (Sous-échantillonnage)** : Réduit le nombre d'exemples de la classe majoritaire, soit 
aléatoirement, soit par des techniques visant à nettoyer la frontière de décision ou à accélérer le processus, comme 
**Condensed Nearest Neighbor (CNN)**, **Tomek Link** ou **Edited Nearest Neighbor (ENN)**.

- **Apprentissage sensible aux coûts (_Cost-Sensitive Learning_)** : Bien que techniquement une modification de la 
fonction de perte plutôt qu'un pré-traitement direct des données, cette approche est considérée comme une méthode 
pour gérer le déséquilibre. Elle consiste à attribuer un **coût plus élevé** aux erreurs sur la classe minoritaire 
(par exemple, les faux négatifs, $c_{FN}) afin de minimiser les pertes.

L'ensemble de ces étapes de préparation des données est essentiel pour initier la **procédure d'apprentissage**, 
permettant de transformer des données brutes en un format exploitable, en identifiant leurs propriétés critiques et en 
adaptant l'échantillon aux contraintes des algorithmes, notamment en présence de classes déséquilibrées.

