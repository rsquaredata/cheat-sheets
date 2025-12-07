<!--
Title: "protocole mise en situation"
Author: rsquaredata
Last updated: 2025-12-04
-->

## 1. identification du problème

### 1.1. Supervisé ou non supervisé ?

#### 1.1.1. Principe fondamental

| Type | Données disponibles | Objectif global |
|------|---------------------|-----------------|
| **Supervisé** | J'ai une **variable cible** $y$ à prédire | Apprendre une relation $y=f(x)$ |
| **Non supervisé** | Je n'ai **pas de variable cible** | Explorer la structure des données (groupes, axes, anomalies, etc.) |

#### 1.1.2. Indices dans le dataset

| Indice | Type | Interprétation |
|--------|------|----------------|
| Présence d'une variable "target", "y", "class", "price", "diagnosis", etc. | Supervisé | On veut prédire cette variable |
| Pas de variable cible, juste des caractéristiques $X_1, X_2, \ldots, X_p$ | Non supervisé | On veut segmenter, réduire ou repérer des schémas |
| Objectif formulé en "prédire", "estimer", "classer", "régression" | Supervisé | ex.: "Prédire le revenu selon le diplôme" |
| Objectif forumé en "découvrir", "analyser", "regrouper", "réduire" | Non supervisé | ex.: "Identifier des profils types d'utilisateurs" |

#### 1.1.3. Démarche

1. **Examiner le jeu de données** :  j'ai une variable cible connue ? si oui, supervisé ; sinon, non supervisé
2. **Clarifer l'objectif métier ou scientifique** : **prédire** une valeur → supervisé ; **comprendre** les structures → non supervisé.
3. **Choisir la famille d'algorithmes adaptée** :

| Type | Sous-type | Exemples d'algorithmes | Sortie attendue |
|------|-----------|------------------------|-----------------|
| **Supervisé** | Classification | SVM, Logistic Regression, Random Forest, Boosting | Classe (catégorie) |
| | Régression | Linear Regression, Ridge, SVR, Gradient Boosting Regressor | Valeur numérique |
| **Non supervisé** | Clustering | k-means, k-medoirs, DBSCAN, HDBSCZN, Hierarchical | Groupes (étiquettes générées- |
| | Réduction de dimension | PCA, t-SNE, UMAP | Axes / projections |
| | Détection d'anomalies | Isolation Forest, Autoencoder, LOF | Score d'anomalie |

#### 1.1.4. Exemples

| Cas | Type | Justification |
|-----|------|---------------|
| prédire si un mail est un spam ou non | supervisé (classification) | $y$ = {spam, non spam} connu |
| prédire le prix d'un logement | supervisé (régression) | $y$ = prix, variable numérique |
| regrouper des clients selon leur comportement | non supervisé (clustering) | pas de $y$, on cherche des groupes naturels |
| identifier des variables corrélées dans un dataset de 100 features | non supervisé (réduction de dimension) | PCA, pas de $y$ |
| détecter des transactions sans étiquettes frauduleuses | non supervisé (anomalies) | on cherche les points rares |
| détecter le thème dominant dans des textes | non supervisé (topic modeling) | LDA, pas de $y$ |
| prédire la probabilité de défaut d'un client | supervisé (classification probabiliste) | $y$ = défaut ou non |

### 1.1.5. Cas limites (hybrides)

| Types | Description | Exemples |
|-------|-------------|----------|
| **semi-supervisé** | une partie des données a une étiquette $y^$, l'autre partie non | apprentissage sur 10% labellé + 90% non labellé |
| **auto-supervisé** | le modèle crée ses propres labels à partie des données | pré-entraînement d'un modèle de langage ou d'images |
| **apprentissage par renforcement** | pas de $y$ fixe, mais une récompense à maximiser | jeu, robotique, trading adaptif |

### 1.2. Classification ou Régression ?

#### 1.2.1. Étude de la variable cible $y$ pour déterminer la nature du problème

| $y$... | Exemple | Type de tâche |
|---------|---------|---------------|
| ...prend des valeurs **numériques continues** | prix, température, consommation, durée | Régression |
| ...prend des valeurs **catégorielles** | O/1, malade/sain, fraude/non-fraude, classes | Classification |
| ...n'est **pas connue** | on cherche à trouver une structure dans les données | Clustering (non supervisé) |
| ...est connue mais il y a **plusieurs labels par instance** | - | Classification multi-étiquette |
| ...est **ordinale** | faible/moyen/fort | Régression ordinale / Classification hiérarchique |

#### 1.2.2. Étude de la variable d'entrée $X$ (*features*)

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

1. Définir l'input et l'output : $X \in \mathbb{R}^{m \times d}$ ; $y \in \mathbb{R}^m$ ou $y \in \\{0,1\\}^m$

2. Formuler la tâche :
    - Régression : trouver $f : X \to \mathbb{R}$
    - Classification : trouver $f : X \to \\{0,1\\}$ ou $f : X \to \\{1, \ldots, K\\}$

3. Choisir l'objectif d'optimisation (minimiser une *loss*) :
  - Régression : MSE, RMSE, MAE
  - Classification : log loss, hinge loss, exponential loss

<small>"La surrogate loss **détermine le type de modèle** et le **comportement de l'optimisation**</small>

| Tâche | Loss idéale | Surrogate courante | Algorithme associé | Forme |
|-------|-------------|--------------------|--------------------|-------|
| **Classification binaire** | 0-1 loss | **Hinge loss** | SVM | $\ell \left( y, f(x) \right) = \max(0, 1-f(x))$ |
| | 0-1 loss | **Logistic loss** | Régression logistique, Boosting | $ell \left( y, f(x) \right) = \log(1 + e^{-y(f(x)})$ |
| | 0-1 loss | **Exponential loss** | AdaBoost | $ell \left( y, f(x) \right) = e^{-yf(x)}$ |
| Régression | $L_2$ | MSE/RMSE | Linéaire, Ridge, SVR | $\left( y-f(x) \right)^2$ |
| | $L_1$ | MAE | Lasso, quantile regression | $\vert y-f(x) \vert $ |

<small>Exemple de rédaction : "Les méthodes de marge large (SVM) reposent sur la hinge loss, qui est une surrogate convexifiant la 0-1 loss"</small>

## 2. Choix des modèles

### 2.1. Diagnostic exploratoire

#### 2.1.1. Linéarité (a.k.a. "vérifier la complexité intrinsèque du problème")

##### 2.1.1.1. Indices de non-linéarité

###### Frontière de décision

La **frontière de décision** sépare les zones de l'espace où le modèle prédit chaque classe.

**Visualiser la frontière** :
- cas 2 variables → tracer un nuage de points (`sns.scatterplot(x,y,hue=class)`) : si je peux tracer une *ligne droite* qui sépare proprement les classes, le problème est linéaire.
- cas > 2 variables → ACP pour réduire à 2 dimensions, ou pairplot (scatterplots pour toutes les paires de variables) : si aucune combinaison de 2 variables ne sépare bien, structure linéaire probable.

- frontière **linéaire** → modèle **linéaire** (SVM linéaire, régression logistique) suffisent.
- frontière **courbe/tordue** → approche à noyau (**SVM à noyau**) ou approche additive (**boosting non linéaire**)
    - indices de non-linéarité :
        - les frontières entre classes sont-elles "droites" ou "courbes" ?
        - y a-t-il des **interactions complexes** entre variables ?
        - les variables sont-elles fortement corrélées entre elles ?
     
###### Interactions entre variables

Une **interaction** se produit quand **l'effet d'une varaible dépend d'une autre variable** :
- formellement :
    - $y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 (x_1 x_2) + \beta_4 x_1^2 + \beta_5 x_2$, où $x_1 x_2$, $x_2, x_1$, $x_1^2$, x_2^2$ est un modèle polynomial du second degré où $x_1 x_2$ est le terme d'**interaction polynomiale**, $x_1^2$ et $x_2^2$ sont des termes **quadratiques** et $x_1$ et $x_2$ sont des termes **linéaires**[^1].
        - si $\beta_3 \gt 0$, l'effet de $x_1$ augmente quand $x_2$ augmente (et inversement) ;
        - si $\beta_3 \lt 0$, l'effet de $x_1$ diminue quand $x_2$ augmente (et inversement).
- exemple pratique : l'effet du sport sur la santé dépend de l'âge.


Les modèles linéaires ne capturent pas ces interactions, il faut :
- soit ajouter manuellement des termes croisées ($x_1 x_2$, x_1^2, \ldots) ;
- soit utiliser un modèle non linéaire (SVM à noyau, boosting, random forest...).

###### Variables fortement corrélées

1. calculer la **matrice de corrélation de Pearson** :
```python
corr df.corr()
sns.heatmap(corr, cmap='coolwarm', annot=True)
```
2. si on a une paire avec $\vert corr \vert \gt 0.8$ → redondance forte.
3. conséquence : variables corrélées → modèle linéaire instable (les coefficients explosent).
4. solutions : ACP ; supprimer une des deux variables de la paire ; modèle à régularisation (Ridge, Lasso).

##### 2.1.1.2. Tester l'hypothèse de linéarité

**Objectif** : vérifier si la relation entre les variables explicatives $X$ et la cible $y$ peut être correctement **modélisée par une fonction linéaire**, ou si elle nécessite un **modèle non linéaire**.

##### 2.1.1.1. Démarche expérimentale

**Protocole** :
1. **Fit un modèle linéaire simple**
    - si classification : régression logistique ou SVM linéaire.
    - si régression : régression linéaire simple ou ridge/lasso.
2. **Fit un modèle non linéaire** :
    - si classification : SVM RBF, arbre de décision, random forest, gradient boosting.
    - si régression : SVR RBF, RandomForectRegressor, XGBoostRegressor.
3. **Comparer les performances** :
    - même split train/test ;
    - même métrique ;
    - CV recommandée.
4. Si le modèle non-linéaire fait **significativement mieux** → la structure est non linéaire ; sinon, le modèle linéaire suffit.

**Indicateurs complémentaires visuels et statistiques** :

1. **Analyse des résidus (régression)** : tracer `y_pred`vs `y_true`ou `residuals = y_true - y_pred` ; si les résidus montrent une **courbe** ou une **structure** → non-linéarité ; si les résidus sont répartis aléatoirement autour de 0 → linéarité plausible.

<details>
    <summary>
        <span style="color:pink; font-weight:bold">Implémentation</span>
    </summary>

```python
sns.scatterplot(x=y_pred, y=y_test - y_pred)
plt.axhline(0, color='red', linestyle='--')
```
</details>

2. **Plairplot ou ACP pour visualiser les frontières** : pour on peut visualier les données (2D ou ACP 2D), séparation nette par une ligne droite → linéaire / frontière incurvée → non-linéaire.

<details>
    <summary>
        <span style="color:pink; font-weight:bold">Implémentation</span>
    </summary>

```python
sns.pairplot(df, hue="target")
```
</details>

3. **Termes d'interaction / polynômes** : ajouter des termes $x_i^2$, $x_i x_j$ avec `PolynomialFeatures` ; si les scores s'améliorent beaucoup → la relation de base n'était pas linéaire.

<details>
    <summary>
        <span style="color:pink; font-weight:bold">Implémentation</span>
    </summary>

```python
from sklearn.preprocessing import PolynomialFeatures
poly = PolynomialFeatures(degree=2)
X_poly = poly.fit_transform(X)
```
</details>


###### 2.1.1.2. Approche à noyau : Choisir un noyau (SVM/Kernel)

- On choisit un noyau *après* avoir sélectionné le modèle <u>SVM</u> et *avant* le tuning des hyperparamètres

| Situation | Type du noyau | Justification |
|-----------|---------------|---------------|
| Frontière simple/linéaire | **linéaire** | suffisant et rapide |
| Frontière courbe mais lisse (sans cassures brutales) | **RBF (Gaussien)** | bonne flexibilité, paramètre $\gamma$ à tuner |
| Frontière non lisse (découpée en rectangles orthogonaux) | | |
| Interaction polynomiale connue | **polynomial** | interprétable si faible degré |
| Volume de données élevé | **approximation RBF (RFF)** | gain de temps mémoire |
| Texte | **noyau de similarité cosinus** ou **TF-IDF kernel** | mesure de proximité sémantique |
| Graphe | **graph kernel** (Weisfeller-Lehman, shortest-path) | compare des sous-graphes structurels |
| Séquences / Bio-informatique | **Noyau de sous-chaînes** (string kernel) | compare des motifs communs |
| Images | **Noyaux RBF / CNN** (*features* préentraînées | capture des formes et textures |
| Données temporelles | **Dynamic Time Warping kernel** | tolère des décalages temporels |

- Toujours tester au moins deux noyaux  et justifier le choix par une **métrique adaptée** et un **compromis biais/variance**



###### 2.1.1.3. Approche additive : Choisir un Boosting

- on choisit le boosting *après* avoir choisi la famille d'<u>algorithmes d'ensemble</u> et *avant* le tuning.
- toujours tester au moins deux variantes de boosting

| Situation | Type de boosting | Justification |
|-----------|------------------|---------------|
| Petit dataset, peu de bruit | **AdaBoost** | simple, efficace, marge large[^2] |
| Données complexes[^3], bruit modéré | **Gradient Boosting** | apprentissage résiduel[^4], flexible |
| Dataset volumineux/*features* nombreuses | **XGBoost / LightGBM** | optimisé, parallèle, régularisé |
| Variables catégorielles dominantes | **CatBoost** | encodage intégré |
| Risque d'overfit fort | **↘ `learning_rate`**, ↗ `n_estimators` | meilleur compromis biais/variance |

#### 2.1.2 Déséquilibre des classes

- dataset équilibré →
- dataset déséquilibré → modèle **additif de type boosting / ensemble**

### 2.2. Choix de la famille de modèles

- SVM
- Boosting
- Forêt
- Réseau de neurones

### 2.3. Choix des modèles selon le type de tâche

#### 2.3.1. Problème de régression

| Algorithmes classiques | Avantages | Limites |
|------------------------|-----------|---------|
| Régression linéaire / Ridge / Lasso | Interprétable, rapide | Sensible à la colinéarité, sous-ajustement |
| SVR (linéaire / RBF) | Bonné généralisation, robuste | Paramètres $C$ et $\gamma$ à tuner |
| Random Forest Regressor | Gère la non-linéarité, pas de normalisation | Peu interprétatble, lent si gros dataset |
| Gradient Boosting / XGBoost | Très performant, gère les features hétérogènes | Sensible au surapprentissage |

#### 2.3.2. Problème de classification

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

**Règle heuristique rapide** :
- dataset équilibré → **Accuracy / AUC**
- dataset déséquilibré → **F1/Recall/MCC**
- priorité à "ne pas rater" → **Recall**
- priorité à "ne pas se tromper" → **Precision**

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

| Métrique | Formule | Définition | Avantages / Inconvénients | Pour données... |
|----------|---------|------------|---------------------------|-----------------|
| **MSE** (Mean squared Error) | $\frac{1}{n} \sum_i (y_i - \hat{y}_i)^2$ | Erreur quadratique moyenne.</br>Très sensible aux valeurs extrêmes | pénalise fortement les grandes erreurs, mais amplifie les outliers | homogènes |
| **RMSE** (Root MSE) | $\sqrt{MSE}$ | Même unité que la variable cible. | plus intuitif, mais même limites que MSE | homogènes |
**MAE* (Mean Absolute Error | $\frac{1}{n} \sum_i \vert y_i - \hat{y}_i \vert $ | Erreur absolue moyenne, plus robuste. | moins sensible aux gros écarts, mais moins différenciant | avec outliers |
| **$R^2$** (Coefficient de détermination | $1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y}_i)^2 }$ | Part de la variance expliquée (max = 1) | intuitif mais pas stable et varie peu | |

<u><str>NB</str></u> : si comparaison de modèles → toujours préciser l'unité (ex.: erreur moyenne de 5°C).

### 5.2. Pour les problèmes de classification

#### 5.2.1. Jeu équilibré

| Métrique | Formule | Définition / Usage | Avantages / Inconvénients | Rédaction |
|----------|---------|------------|-------------------------------|-----------|
| **Accuracy** | $\frac{TP+TN}{TP+TN+FP+FN}$ | pourcentage de bonnes prédictions | simple, mais trompeur si classes déséquilibrées | 
| **AUC ROC** (Area Under the Curve of the Receiver Operating Characteristic) | $AUC = \int_0^1 TPR(FR) \text{ d}(FPR)$</br>    $\simeq \sum_{i=1}^{n-1} (FPR_{i+1} - FPR_i) \times \frac{TPR_{i+1} + TPR_i}{2}$ | • capacité à discriminer les classes.</br>• maths : aire sous la courbe ROC (TPR vs FPR), calculée en pratique comme une somme de trapèzes</br>• stats : $AUC = \mathbb{P}(\text{score positif} \gt taxt{score négatif})$ | AUC = 0.5 → modèle aléatoire (aucun pouvoir discriminant)</br>AUC = 0.7-0.8 → correct</br>AUC = 0.8-0.9 → bon</br>AUC > 0.9 → excellent</br>AUC = 1.0 → parfait (souvent sur-apprentissage</br>L'AUC est **indépendante du seuil** → pratique mais peut **masquer** une mauvaise calibration des probabilités | La courbe ROC trace la sensibilité (TPR) en fonction du taux de faux positifs (FPR). L'aire sous la courve (AUC) mesure la capacité du modèle à classer un positif au-dessus d'un négatif. Une AUC = 1 correspond à un classifieur parfait, 0.5 à un modèle aléatoire. |
| **Log Loss / Cross Entropy** | $- \frac{1}{n} \sum \left[y_i \log(p_i) + (1-y_i) \log(1-p_i) \right]$ | pénalise les prédictions trop confiantes et fausses | bon pour calibration probabiliste | |

#### 5.2.2. Jeu déséquilibré

*Accuracy* devient inutile : un modèle qui prédit toujours la classe majoritaire peut avoir 95% de précision mais 0 en recall.

| Métrique | Formule | Sens intuitif | Quand utiliser |
|----------|---------|---------------|----------------|
| **Precision** | $\frac{TP}{TP+FP}$ | parmi les positifs prédits, combien sont vrais | faux positifs coûteux (ex.: spams, fraude) |
| **Recall (Sensibilité)** | \frac{TP}{TP+FN} | parmi les vrais positifs, combien ont été trouvés | faux négatifs coûteux (ex.: cancer, panne) |
| **F1-score** | $2 \times \frac{Precision \times Recall}{Precision + Recall}$ | compromis entre précision et rappel | bon résumé global pour classes rares |
| **Balanced Accurary** | $\frac{TPR + TNR}{2}$ | moyenne des taux de succès par classe | comparer des modèles sur dataset déséquilibré |
| **MCC** (Matthews Correlation Coefficient) | $\frac{TP \times TN - FP \times FN}{\sqrt{(TP+FP)(TP+FN)(TN+FP)(TN+FN)}}$ | corrélation entre prédictions et vérité terrain | robuste, stable même si classes très déséquilibrées |

### 5.3. Visualisation

| Outil | Ce qu'il montre | Utilité |
|-------|-----------------|---------|
| **Matrice de confusion** | vrais/faux positifs/négatifs | voir où le modèle se trompe |
| **Courbe AUC ROC** | TPR vs FPR | comparaison visuelle de modèles |
| **Courbe PR (Precision-Recall)** | Precision vs Recall | plus informative si classes rares |
| **Learning Curve** | Score train vs test selon le nb d'exemples | détecter un sur/sous-apprentissage |

## 6. Tuning des hyperparamètres

1. split train / validation / test
2. CV 5-fold
3. GridSearchCV ou RandomizedSearchCV
4. Comparer sur **métrique adaptée** (pas forcément *Accuracy*)
5. vérifier la **stabilité des scores** variance basse → modèle robuste)

| Modèle | Hyperparamètres clés | Comment / Pourquoi | Effet du réglage | Astuce / Pièges |
|---------|----------------------|--------------------|------------------|-----------------|
| **SVM (RBF)** | C (régularisation) ; γ (largeur du noyau RBF) | `GridSearchCV` sur C ∈ [0.1, 1, 10], γ ∈ [0.01, 0.1, 1] | C ↗ → surapprentissage ; γ ↗ → frontière trop fine | Toujours standardiser les *features* |
| **Random Forest** | `n_estimators` ; `max_depth` ; `min_samples_split` ; `max_features` | | Plus d'arbres → meilleure stabilité mais plus lent ; profondeur ↗ → surfit | Commencer par peu de profondeur |
| **Gradient Boosting** | `n_estimators` ; `learning_rate` ; `max_depth` ; `subsample` | Petit `learning_rate` → réduit le surapprentissage mais nécessite plus d'estimators | | Commencer avec `learning_rate = 0.1` |
| **Ridge / Lasso** | λ = α (force de régularisation) | Tuning par validation croisée | λ ↗ → coefficients plus petits → biais ↗, variance ↘ | |
| **k-NN** | k, distance | | Petit k → surfit ; grand k → biais fort | Garder k impair, scaling obligatoire |
| **NN (réseau de neurones)** | `learning_rate` ; `hidden_layers` ; `epochs` ; `batch_size` | | `learning_rate` trop haut → divergence | Surveiller la courbe de perte ; *early stopping* recommandé |
| **Arbre de décision** | `max_depth` ; `min_samples_split` ; `criterion` | | Profondeur ↗ → surfit | *Pruning* recommandé |
| **XGBoost** | `eta` ; `max_depth` ; `colsample_bytree` ; `lambda` | Paramètres très interdépendants | | Tuning itératif par bloc |

- Toujours normaliser les données pour SVM / k-NN / NN.
- Toujours vérifier la stabilité par $k$-fold (5-CV par défaut).
- tuning noyaux : $C$ (si SVM linéaire) ou $C$ et $\gamma$ (si SVM RBF)
- tuning boosting : `n_estimators`, `learning_rate`, `max_depth` et `subsample`, parfois `lambda` (régularisation) et `colsample_bytree` (échantillonnage des *features*).

## 7. Validation et interprétation

### 7.1. Comparer plusieurs modèles sur le même split

**Bonnes pratiques** :
- ** même preprocessing et même train/test** pour tous les modèles (sinon, comparaison biaisée).
- **même métrique** pour tous.
- **validation croisée §$k$-fold)** :
     - par défaut, $k = 5$.
     -  **stratifiée** si classification déséquilibrée.
     -  rapporter moyenne $\pm$ écart-type.
  
### 7.2. Choisir le modèle qui offre le meilleur compromis

**Trois axes de comparaison** :

| Axe | Exemples de critères | Objectif |
|-----|----------------------|----------|
| **Performance** | AUC, F1, RMSE, R²... | précision globale |
| **Complexité** | nb de paramères, temps d'apprentissage | efficacité computationnelle |
| **Robustesse** | stabilité des résultats (variance CV), stabilité des *features* | fiabilité sur échantillons nouveaux |

### 7.3. Visualiser les résultats

| Type de tâche | Visualisation | Ce qu'elle montre | Interprétation attendue | Exemple de rédaction |
|---------------|---------------|-------------------|-------------------------|----------------------|
| **Classification** | **Courbe ROC / AUC** | compromis entre rappel (TPR) et faux positifs (FPR) pour tous les seuils | AUC proche de 1 → excellente discrimination entre classes | <small>"Le SVM à noyau RBF atteint une AUC de 0,93, traduisant une forte capacité de discrimination."</small> |
| | **Courbe PR (Precision-Recall)** | compromis entre précision et rappel, utile si classes déséquilibrées | courbe proche du coin supérieur droit = bon équilibre précision/rappel | <small>"Le modèle conserve une précision stable jusqu’à un rappel de 0,8, indiquant une gestion correcte du déséquilibre."</small> |
| | **Matrice de confusion** | répartition des prédictions correctes et erreurs par classe | permet de repérer les classes sous-prédites | <small>"La classe minoritaire est mal détectée (rappel 0,62), ce qui justifie un rééquilibrage ou un seuil adapté.".</small> |
| **Courbes d'apprentissage** | évolution du score train/validation selon la taille de l'échantillon | écart large = overfit ; écart faible = bon compromis | <small>"Le modèle surapprend : le score train reste élevé tandis que le score validation stagne."</small> |
| **Importance des features** (forêts, boosting) | contribution de chaque variable à la prédiction | identifie les variables déterminantes ; attention aux corrélations | <small>"Les variables de revenu et d’âge expliquent 70 % de la variance prédictive."</small> |
| **Régression** | **Graphique des résifus vs valeurs prédites** | qualité d'ajustemnet : résidus centrés autour de 0 ? | répartition aléatoire = bon modèle ; structure = biais / non-linéarité | <small>"Les résidus sont homogènes, aucune structure apparente : l’ajustement est correct."</small> |
| **Histogramme ou QQ-plot des résidus** | vérifie la normalité et la symétrie des errurs | distribution centrée et gaussienne = modèle conforme aux hypothèses | <small>"Les résidus suivent une distribution quasi normale, validant l’hypothèse de linéarité."</small> |
| | **Résidus standardisés ou Cook's distance** | détexte les points influents / outliers | valeurs extrêmes → observation atypique à évaluer | <small>"Deux points présentent une distance de Cook > 1, suggérant une influence excessive." | | **Courbe de calibration ($\hat{y}$ vs $y$ réel)** | fidélité du modèle : les prédicitions suivent-elles la diagonale idéale ? | points proches de la diagonale → bon calibrage | <small>"La courbe de calibration est proche de la diagonale, le modèle prédit sans biais systématique"</small> |
| | **Courbes d'apprentissage (train/test)** | sur- ou sous-apprentissage selon taille de l'échantillon | écart large → surfit ; scores faibles → underfit | <small>"Le modèle est trop complexe : la perte test augmente après 200 itérations."</small> |

<details >
    <summary><span style="color:pink; font-weight:bold">Implémentation</span></summary>

En résumé :
- `sklearn.metrics` $\rightarrow$ pour ROC, PR, AUC, confusion, displays.
- `sklearn.model_selection` $\rightarrow$ pour `learning_curve`.
- `matplotlib`/`seaborn` $\rightarrow$ pour tracer le reste.
- `statsmodels`/`scipy`$\rightarrow$ pour les résidus et diagnostics avancés.


#### Courbe ROC / AUC (classification)

Version qui fonctionne pour tous les classifieurs ayant `predict_proba()` ou `decision_function()` :

```python
# Librairies
from sklearn.metrics import RocCurveDisplay
import matplotlib.pyplot as plt

# Affichage automatique à partir d’un modèle sklearn
RocCurveDisplay.from_estimator(model, X_test, y_test)

# Personnalisation
plt.title("Courbe ROC")
plt.grid(True)
plt.show()
```

Version manuelle :

```python
from sklearn.metrics import roc_curve, auc
fpr, tpr, _ = roc_curve(y_test, model.predict_proba(X_test)[:,1])
roc_auc = auc(fpr, tpr)
plt.plot(fpr, tpr, label=f"AUC = {roc_auc:.2f}")
plt.legend(); plt.show()
```

#### Courbe  Precision-Recall (classification)

```python
# Librairies
from sklearn.metrics import PrecisionRecallDisplay
import matplotlib.pyplot as plt

# Tracé automatique
PrecisionRecallDisplay.from_estimator(model, X_test, y_test)

# Personnalisation
plt.title("Courbe Precision–Recall")
plt.grid(True)
plt.show()
```

#### Matrice de confusion

```python
# Librairies
from sklearn.metrics import ConfusionMatrixDisplay
import matplotlib.pyplot as plt

# Affichage direct
ConfusionMatrixDisplay.from_estimator(model, X_test, y_test, cmap="Blues")

plt.title("Matrice de confusion")
plt.show()
```

Pour récupérer les valeurs :

```python
from sklearn.metrics import confusion_matrix
cm = confusion_matrix(y_test, model.predict(X_test))
print(cm)
```

#### Courbe d'apprentissage (classification ou régression)

```python
# Librairies
from sklearn.model_selection import learning_curve
import matplotlib.pyplot as plt
import numpy as np

# Calcul des scores pour différentes tailles d'échantillon
train_sizes, train_scores, val_scores = learning_curve(model, X, y, cv=5)

# Moyenne et intervalle de confiance
plt.plot(train_sizes, np.mean(train_scores, axis=1), label="train")
plt.plot(train_sizes, np.mean(val_scores, axis=1), label="validation")
plt.xlabel("Taille de l'échantillon")
plt.ylabel("Score (AUC / R² / F1)")
plt.title("Courbe d'apprentissage")
plt.legend()
plt.grid(True)
plt.show()
```

### Importance des *features* (arbres, forêts, boosting)

Pour `RandomForest`, `GradientBoosting`, `XGBoost`, `LightGBM` :

```python
# Librairies
import matplotlib.pyplot as plt
import pandas as pd

# Récupération des importances
importances = model.feature_importances_
features = X.columns

# Classement et affichage
imp_df = pd.DataFrame({'Feature': features, 'Importance': importances})
imp_df.sort_values(by='Importance', ascending=True, inplace=True)

plt.barh(imp_df['Feature'], imp_df['Importance'])
plt.title("Importance des variables")
plt.xlabel("Importance")
plt.show()
```

Pour SVM linéaire → utiliser `model.coef_`.

#### Graphique des résidus (régression)

```python
# Librairies
import matplotlib.pyplot as plt
import seaborn as sns

# Calcul des résidus
y_pred = model.predict(X_test)
residuals = y_test - y_pred

# Tracé des résidus
sns.scatterplot(x=y_pred, y=residuals)
plt.axhline(0, color='red', linestyle='--')
plt.xlabel("Valeurs prédites")
plt.ylabel("Résidus")
plt.title("Analyse des résidus")
plt.show()
```

→ résidus aléatoires autour de 0 = bon ajustement.  
→ forme en courbe = modèle mal spécifié (non-linéarité non captée).

#### Histogramme / QQ-plot des résidus (régression)

```python
# Librairies
import seaborn as sns
import matplotlib.pyplot as plt
from scipy import stats

# Distribution des résidus
sns.histplot(residuals, kde=True)
plt.title("Distribution des résidus")
plt.show()

# QQ-plot (normalité)
stats.probplot(residuals, plot=plt)
plt.title("QQ-plot des résidus")
plt.show()
```

#### Courbe de calibration (régression)

```python
# Librairies
import matplotlib.pyplot as plt

# y_pred et y_test déjà calculés
plt.scatter(y_test, y_pred)
plt.plot([y_test.min(), y_test.max()], [y_test.min(), y_test.max()], 'r--')

plt.xlabel("Valeurs réelles")
plt.ylabel("Valeurs prédites")
plt.title("Courbe de calibration (ŷ vs y réel)")
plt.show()
```

#### Courbes d'apprentissage (régression)

*(identique à la version classification, mais on évalue R² ou RMSE)*

```python
# Même code que plus haut
train_sizes, train_scores, val_scores = learning_curve(model, X, y, cv=5, scoring='r2')

plt.plot(train_sizes, np.mean(train_scores, axis=1), label="train")
plt.plot(train_sizes, np.mean(val_scores, axis=1), label="validation")
plt.ylabel("Score R²")
plt.title("Courbe d'apprentissage (régression)")
plt.legend()
plt.show()
```

</details>

## 7. Cas spéciaux

| Situation | Réponse attendue |
|-----------|------------------|
| Classes déséquilibrées | stratified CV + `class_weight` + FA/Recall |
| Bcp de variables | ACP | sélection de features / Lasso |
| Peu de données | modèles simples + régularisation forte |
| Temps de calcul critique | préférer un modèle linéaire ou Random Forest |
| Besoin d'interprétabilité | régression logistique, arbres |
| Surapprentissage suspecté | learning curve, réduction complexité, early stopping |



---
[^1] Mathématiquement, $x_1 x_2 = x_2 x_1$.  
[^2] AdaBoost (comme SVM) cherche à **maximiser la marge**, càd la **séparation moyenne entre les classes**. Chaque faible classifieur (arbre de profondeur 1) est pondéré pour **augmenter la confiance dans les échantillons bien classés** et **corriger les erreurs**. Résultat : le modèle final a une **grande marge effective** → meilleure généralisation, moins de variance.
[^3] Données complexes = non linéaires, bruitées, variables dépendantes (ex.: prévisions financières, médicales).
[^4] Apprentissage rési
