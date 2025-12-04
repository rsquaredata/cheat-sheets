<!--
Title: "protocole mise en situation"
Author: rsquaredata
Last updated: 2025-12-04
-->

## 1. identification du problème

### 1.1. Étude de la variable cible $y$ pour déterminer la nature du problème

| $y$ ... | Exemple | Type de tâche |
|---------|---------|---------------|
| prend des valeurs **numériques continues** | prix, température, consommation, durée | Régression |
| prend des valeurs **catégorielles** | O/1, malade/sain, fraude/non-fraude, classes | Classification |
| n'est **pas connue** | on cherche à trouver une structure dans les données | Clustering (non supervisé) |
| est connue mais il y a **pluseurs labels par instance** | - | Classification multi-étiquette |
| est **ordinale** | faible/moyen/fort | Régression ordinale / Classification hiérarchique |

### 1.2. Étude de la variable d'entrée $X$ (*features*)

| Type de variable | Exemple | Conséquences sur les méthodes |
|------------------|---------|-------------------------------|
| Numérique continue | âge, revenu, sexe | Normalisation nécessaire pour SVM, kNN, régression |
| Catégorielle | sexe, région | Encodage obligatoire (One-Hot, OrdinalEncoder) |
| Mixte | âge + région + niveau d'étude | Algorithmes mixtes (Random Forest, Gradient Boosting, k-protoypes) |

### 1.3. Cadrage expérimental

| Contexte des données | Conséquences |
|----------------------|--------------|
| Jeu **déséquilibré** | Utiliser `class_weight`, SMOTE, métriques Recall/F1 |
| Données **nombreuses** | Préférer des *modèles rapides* (modèles linéaires, sous-échantillonnage) |
| Données **faibles ou bruitées** | Renforcer la *régularisation* et la *validation croisée* |
| Variables **fortement corrélées** | Réduire la dimension (ACP, Lasso) |
| Besoin d'**interprétabilité** | Privilégier *régression logistique*, *arbres de décision* |

### 1.4. Formuler le problème

1. Définir l'input et l'output : $X \in \mathbb{R}^{m \times d}$ ; $y \in \mathbb{R}^m$ ou $y \in {{0,1}}^m$

2. Formuler la tâche :
    - Régression : trouver $f : X \to \mathbb{R}$
    - Classification : trouver $f : X \to {{0,1}}$ ou $f : X \to {{1, \ldots, K}}$

3. Choisir l'objectif d'optimisation (minimiser une *loss*) :
  - Régression : MSE, RMSE, MAE
  - Classification : log loss, hinge loss, exponential loss

## 2. Choix des modèles selon le problème

### 2.1. Régression

| Algorithmes classiques | Avantages | Limites |
|------------------------|-----------|---------|
| Régression linéaire / Ridge / Lasso | Interprétable, rapide | Sensible à la colinéarité, sous-ajustement |
| SVR (linéaire / RBF) | Bonné généralisation, robuste | Paramètres $C$ et $\gamma$ à tuner |
| Random Forest Regressor | Gère la non-linéarité, pas de normalisation | Peu interprétatble, lent si gros dataset |
| Gradient Boosting / XGBoost | Très performant, gère les features hétérogènes | Sensible au surapprentissage |

### 2.2. Classification

| Algorithmes classiques | Avantages | Limites |
|------------------------|-----------|---------|
| Régression logistique | Interprétable, baseline solide | Frontière linéaire uniquement |
| SVM (linéaire / RBF) | Performant sur les données peu bruitées | Sensible au scaling / $C$ / $\gamma$ |
| k-NN | Simple, non-paramétrique | Coût élevé en test, choix du $k$ |
| Random Forest | Gère les données mixtes, importance des variables | Peux explicatif sur les décisions |
| Gradient Boosting | Très efficace sur déséquilibre | Long à entraîner, nécessite réglage fin |
| Réseaux de neurones | Très flexible | Données nombreuses + tuning lourd |

## 3. Prétraitement des données

1. Valeurs manquantes → imputation (moyenne / médiane / mode)
2. Données hétérogènes → encodage (one-hot, label encoding)
3. Variables corrélées → réduction (ACP, sélection de features)
4. Classes déséquilibrées → `class_weight`, oversampling/undersampling, SMOTE

## 4. Définir le protocole expérimental

1. Split : Train / Validation / Test → 70/15/15 ou $k$-fold CV (5-CV)
2. Choix de la métrique principale
3. Comparaison des modèles selon performance + temps d'apprentissage
4. Interprétation / validation finale

## 5. Choix des métriques

```text
→ Type de problème :
    ├── Régression :
    │       ├── RMSE ou MAE  → erreur absolue / relative
    │       ├── R² → part de variance expliquée
    │       └── Ajustement du modèle (biais / variance)
    │
    └── Classification :
            ├── Jeu équilibré :
            │       ├── Accuracy (taux de bonne prédiction)
            │       └── AUC si probas disponibles
            │
            └── Jeu déséquilibré :
                    ├── F1-score → compromis précision/rappel
                    ├── Recall → éviter les faux négatifs
                    ├── Precision → éviter les faux positifs
                    └── Balanced accuracy ou MCC
```

### 5.1. Pour les problèmes de régression

## 6. Tuning des hyperparamètres

| Méthode | Paramètre clé | Comment / Pourquoi |
|---------|---------------|--------------------|
| SVM | $C$ (régularisation)</br>$\gamma$ (noyau RBF) | `GridSearchCV`sur $C$ ($C \in \[0.1,1,10\]$, $\gamma \in \[0.01,0.1,1\]$ |
| Random Forest | n_estimators</br>max_depth</br>min_samples_split | plus d'arbres → meilleur stabilité mais plus lent |
| Boosting | n_estimators</br>learning_rate</br>max_depth | petit learning_rate → réduit le surapprentissage |
| Ridge / Lasso | $\lambda = \alpha$ (régularisation) | tuning par CV |
| k-NN | $k$, distance | $k$ impair, scaling obligatoire |
| Neural Net | lr</>nb de couches</br>epochs | attention à l'overfitting, early stopping |

Toujours normaliser les données pour SVM / k-NN / NN.
Toujours vérifier la stabilité par $k$-fold (5-CV par défaut).

## 7. Validation et interprétation

1. Comparer plusieurs modèles sur le même split :
    - performances (métrique choisie)
    - temps d'apprentissage
    - stabilité des résultats (variance CV)
  
2. Choisir le modèle qui offre le meilleur compromis

3. Visualiser :
    - courbe ROC / AUC ROC
    - importance des features
    - résidus (régression)

## 7. Cas spéciaux

| Situation | Réponse attendue |
|-----------|------------------|
| Classes déséquilibrées | stratified CV + `class_weight` + FA/Recall |
| Bcp de variables | ACP | sélection de features / Lasso |
| Peu de données | modèles simples + régularisation forte |
| Temps de calcul critique | préférer un modèle linéaire ou Random Forest |
| Besoin d'interprétabilité | régression logistique, arbres |
| Surapprentissage suspecté | learning curve, réduction complexité, early stopping |








## 3. Prétraitement des données

## 4. Définition du protocole expérimental

## 5. Choix des métriques

## 6. Tuning des hyperparamètres

## 7. Validation et interprétation

## 8. Cas particuliers
