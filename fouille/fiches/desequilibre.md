<!--
Title: "Big Data Mining - Fiches - Classification déséquilibrée"
Author: rsquaredata
Last updated: 2025-12-10
-->

# Classification déséquilibrée

## 1. Pourquoi le problème est difficile ?

Dans un dataset déséquilibré :  
- la classe minoritaire est très rare (ex : 1 %, 5 %)  
- un classifieur trivial peut atteindre 99 % d’accuracy en prédisant toujours la classe majoritaire → Accuracy ne veut rien dire.  

Le vrai enjeu :
> Détecter correctement la classe minoritaire tout en contrôlant les faux positifs.

## 2. Métriques adaptées

**À éviter** :  
- Accuracy  
- Balanced accuracy sans explication  

**À utiliser* :  
- Recall (taux de détection des cas positifs)  
- Precision (fiabilité des alertes)  
- F1-score (équilibre Precision / Recall)  
- AUC ROC (performance globale)  
- PR-AUC (encore mieux quand le déséquilibre est très fort)  

> L'accuracy est trompeuse dans un contexte déséquilibré. Nous privilégions Recall, F1 et AUC, ainsi que PR-AUC si la classe minoritaire est extrêmement rare.

## 3. Stratégies pour traiter le déséquilibre

### 3.1. Rééchantillonnage (sampling)

1. **Oversampling**
    - exemples :
        -  Random oversampling
        - SMOTE (synthèse de nouveaux points minoritaires)
        - ADASYN (SMOTE adaptatif)
    - Avantages : récupère des cas minoritaires, simple
    - Limites : peut créer du sur-apprentissage

2. **Undersampling**
    - exemples :
        - Random undersampling
        - Tomek links / Edited Nearest Neighbors
    - Avantages : rapide
    - Limites : on perd de l'information majoritaire

### 3.2. Méthodes intégrées dans le modèle

3. **Class weights**
    - exemples :
        - regression logistique avec `class_weight=balanced`
        - SVM avec pénalisation différenciée
        - Random Forest + XGBoost avec weights
    - Avantages : simple, robuste, évite de modifier les données. À citer absolument.
> Nous utilisons des poids de classes pour augmenter la pénalisation des faux négatifs.

4. **Focal Loss (si modèle boosting ou NN)**  
    - pénalise davantage les erreurs sur la classe minoritaire.  
    - très puissant, mais rarement nécessaire d’en parler.  

## 4. Ajustement du seuil de décision (thresholding)

**Principe** :  
- Un modèle renvoie une probabilité $p(y=1 \vert x)$.
- Par défaut : seuil = 0.5
- En déséquilibré : **toujours faux**.  

On choisit un seuil optimisé selon :
- la courbe précision/rappel
- un coût métier
- le recall souhaité
- la maximisation du F1

> Le seuil de décision doit être ajusté : on ne prédit pas à 0.5 mais au seuil qui maximise le F1 ou le Recall selon l'objectif métier.

## 5. Choix des modèles adaptés

**Très bons** :
- Random Forest
- Gradient Boosting (XGBoost, LightGBM)
- Logistic Regression + class weights
- SVM linéaire (si p ≫ n) + class weights
- Neural networks (si beaucoup de données)  

**Pour la détection d'anomalies** :
- Isolation Forest
- One-Class SVM
- Autoencoders

## 6. Pipeline rédigé

1. Le dataset est fortement déséquilibré : la classe positive est très rare. L’accuracy est donc inadaptée.

2. Nous appliquons une stratégie de rééquilibrage : oversampling (SMOTE) ou class weights, combinée à une validation croisée stratifiée.

3. Le modèle choisi est un Gradient Boosting ou une Random Forest, robustes et adaptés aux interactions non linéaires.

4. Les métriques utilisées sont : Recall, F1-score et AUC (ou PR-AUC si la rareté est extrême).

5. Nous ajustons le seuil de décision pour optimiser la courbe précision–rappel selon les besoins métier.

6. Le pipeline proposé maximise la détection de la classe minoritaire tout en limitant les faux positifs.

## 7. Huit points-clés mentionner absolument

1. Accuracy = trompeuse
2. utiliser Recall + F1 + AUC
3. sampling (SMOTE + undersampling)
4. class weights
5. optimiser le seuil
6. stratification obligatoire
7. modèle robuste (RF, XGB, LR weighted)
8. explication métier (minimiser faux négatifs)













