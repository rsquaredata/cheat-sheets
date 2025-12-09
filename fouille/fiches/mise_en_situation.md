<!--
Title: "Big Data Mining - Fiches - Mise en situation"
Author: rsquaredata
Last updated: 2025-12-09
-->

# Mise en situation

**Squelette universel** :  
1. Compréhension du problème
2. Analyse du dataset + prétraitements
3. Feature engineering
4. Choix du modèle / des modèles
5. Choix de la loss + métriques de performance
6. Stratégies spécifiques (déséquilibre, grande dimension, bruit, corrélations)
7. Conclusion + justification pipeline

## 1. Compréhension du problème

Donner le cadre du sujet :  
- Type de tâche : **classification binaire**, **régression**, *détection d'anomaloes**, etc.
- Contraintes clés :
  - données **déséquilibrées**,
  - **haute dimension**,
  - **volume élevé**,
  - données **hétérogènes**,
  - données **manquantes**,
  - objectif business (ex.: minimiser fraude, maximiser recall, etc.).

> Nous sommes dans un problème de XXX, avec contraintes YYY. La stratégie doit donc être adaptée pour gérer ZZZ.

## 2. Prétraitement

**Gestion des valeurs manquantes** :  
- imputation simple : moyenne / médiane / mode
- imputation avancée : kNN imputer, MICE
- suppression si trop nombreuses  

**Normalisation/standardisation** : indispensable pour  
- SVM
- Logistic regression
- kNN
- régression linéaire
- réseaux de neurones
> Les variables sont sur des échelles différentes → standardisation en Z-score.  

**Encodage des variables catégorielles** :  
- one-hot encoding
- target encoding (rare, seulement si high cardinality)
- CatBoost permet l'encodage interne  

**Suppression des doublons / incohérences**  

**Split train / validation / test**  
Stratification si classificatio déséquilibrée.  

## 3. Feature engineering

**Variables temporelles**    
- extraire : jour, mois, heure
- temps écoulé depuis un événément
- rolling windows  

**Variables d'interactions**  
- produits croisés
- ratios (ex.: montant / salaire)  

**Réduction de dimension** (pour les datasets énormes)  
- ACP
- sélection par variance
- sélection via modèle (L1, random forest)
- auto-encoders (advanced)  
> Dans un contexte de grande dimension, il est essentiel de réduire la dimension pour limiter le surapprentissage et améliorer la stabilité.  

## 4. Choix du modèle

**Classification binaire**   
- Logistic Regression (baseline)
- Random Forest (robuste, marche bien directement)
- Grandient Boosting (XGBoost, LightGBM)
- SVM linéaire si p très grand
- kNN parfois, mais rarement optimal
- NN dans les cas complexes  

**Détection de fraude/anomalies**  
- Isolation Forest
- One-Class SVM
- Auto-encoders
- Clustering (kmeans) + distance au centre  

**Régression**  
- Régression linéaire / Ridge / Lasso
- Random Forest Regression
- Gradient Boosting Regression
- Réseau de neurones pour signaux complexes  

## 5.Choix de la loss et des métriques

**Classification déséquilibrée**  
- PAS Accuracy, inadapté  
- choisir :  
  - F1-score
  - Recall si faux négatifs très coûteux (fraude, maladie)
  - Precision si faux positifs coûteux
  - AUC (ROC)
  - PR-AUC  
> Dans un problème déséquilibré, la précision globale n'a aucun sens. Nous utiliserons F1, Recall et AUC.  

**Régression**  
- R%SE
- MAE
- R²
- MAPE (si valeurs positives)  

## 6. Stratégies adaptées aux contraintes du sujet

**Données déséquilibrées (fraude bancaire, fiscale, maladie)** :  
- **oversampling** (SMOTE, random oversampling)
- **undersampling**
- **class weights** dans le modèle
- **threshold tuning**
- **focal loss** (pour boosting/ deep learning)  
> Plutôt que d’optimiser l'accuracy, on optimise la courbe de précision/rappel et on ajuste le seuil de décision.

**Grande dimension** :  
- SVM linéaire (pas kernel !)
- ACP / sélection de variables
- régression Ridge
- Gradient Boosting + early stopping
- Random Forest (ok si p raisonnable)  
> Un noyau RBF serait prohibitif en O(n²) mémoire, donc on privilégie des modèles linéaires ou des modèles parcimonieux.


**Mélage numérique / catégoriel**  
- One-hot encoding
- CatBoost (gère mieux les catégorielles)
- Random Forest (insensible au scale)  

**Très grande volumétrie (Big Data)**  
- mini-batch gradient descent
- méthdodes online
- arbres peu profonds type gradient boosting distribué
- Spark MLlib  

**Objectif business spécifique** (ex.: minimiser les pertes financières, détecter une maladie)  
Nécessité d'adapter :  
- métrique
- seuil de décision
- loss éventuelle (local loss, weighted loss)  

## 7.Conclusion + justification

Expliquer pourquoi le pipeline est cohérent.  
> Le pipeline proposé combine une préparation rigoureuse, un modèle robuste aux contraintes du problème, et des métriques adaptées pour optimiser réellement l'objectif métier. L'ensemble assure un modèle performant, stable et interprétable.

---

### Modèle

1. Nous sommes dans un problème de classification binaire fortement déséquilibrée. Les enjeux sont la détection des cas minoritaires et la minimisation du coût associé aux faux négatifs.

2. Le jeu de données présente des valeurs manquantes et des variables hétérogènes. Nous appliquons imputation simple, standardisation pour les variables numériques, et one-hot encoding pour les variables catégorielles.

3. Nous enrichissons les données par des variables temporelles, des ratios pertinents, et éventuellement une réduction de dimension (PCA) si p est très grand.

4. Comme modèles, nous considérons : logistic regression (baseline), Random Forest, et Gradient Boosting. Le boosting est souvent le plus performant pour ce type de problème.

5. Les métriques retenues sont Recall, F1-score et AUC, car l'accuracy est inadaptée au déséquilibre.

6. Pour gérer le déséquilibre, nous utilisons un weighted loss, SMOTE, et un ajustement du seuil.

7. Au final, la démarche garantit un modèle robuste, bien calibré, et adapté aux contraintes métier.

