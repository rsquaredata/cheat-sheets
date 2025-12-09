<!--
Title: "protocole mise en situation"
Author: rsquaredata
Last updated: 2025-12-04^8
-->

## Table des matières

- [Table des matières](#table-des-matières)
- [1. identification du problème](#1-identification-du-problème)
  - [1.1. Supervisé ou non supervisé ?](#11-supervisé-ou-non-supervisé-)
    - [1.1.1. Principe fondamental](#111-principe-fondamental)
    - [1.1.2. Indices dans le dataset](#112-indices-dans-le-dataset)
    - [1.1.3. Démarche](#113-démarche)
    - [1.1.4. Exemples](#114-exemples)
  - [1.1.5. Cas limites (hybrides)](#115-cas-limites-hybrides)
  - [1.2. Classification ou Régression ?](#12-classification-ou-régression-)
    - [1.2.1. Étude de la variable cible $y$ pour déterminer la nature du problème](#121-étude-de-la-variable-cible-y-pour-déterminer-la-nature-du-problème)
    - [1.2.2. Étude de la variable d'entrée $X$ (*features*)](#122-étude-de-la-variable-dentrée-x-features)
  - [1.3. Cadrage expérimental](#13-cadrage-expérimental)
  - [1.4. Formuler le problème](#14-formuler-le-problème)
- [2. Choix des modèles](#2-choix-des-modèles)
  - [2.1. Diagnostic exploratoire](#21-diagnostic-exploratoire)
    - [2.1.1. Linéarité (a.k.a. "vérifier la complexité intrinsèque du problème")](#211-linéarité-aka-vérifier-la-complexité-intrinsèque-du-problème)
      - [2.1.1.1. Indices de non-linéarité](#2111-indices-de-non-linéarité)
        - [Frontière de décision](#frontière-de-décision)
        - [Interactions entre variables](#interactions-entre-variables)
        - [Variables fortement corrélées](#variables-fortement-corrélées)
      - [2.1.1.2. Tester l'hypothèse de linéarité](#2112-tester-lhypothèse-de-linéarité)
      - [2.1.1.3. Démarche expérimentale](#2113-démarche-expérimentale)
        - [Protocole](#protocole)
        - [Indicateurs complémentaires visuels et statistiques](#indicateurs-complémentaires-visuels-et-statistiques)
        - [Critères quantitatifs à surveiller](#critères-quantitatifs-à-surveiller)
    - [2.1.2. Approche à noyau : Choisir un noyau (SVM/Kernel)](#212-approche-à-noyau--choisir-un-noyau-svmkernel)
    - [2.1.3. Approche additive : Choisir un Boosting](#213-approche-additive--choisir-un-boosting)
    - [2.1.4 Déséquilibre des classes](#214-déséquilibre-des-classes)
  - [2.2. Choix de la famille de modèles](#22-choix-de-la-famille-de-modèles)
    - [Objectif](#objectif)
    - [Tableau comparatif synthétique](#tableau-comparatif-synthétique)
    - [2.2.1 SVM (Support Vector Machines)](#221-svm-support-vector-machines)
    - [2.2.2. Boosting (AdaBoost, GradientBoosting, XGBoost, LightGBM, CatBoost)](#222-boosting-adaboost-gradientboosting-xgboost-lightgbm-catboost)
    - [2.2.3. Forêts aléatoires (Random Forest)](#223-forêts-aléatoires-random-forest)
  - [2.2.4. Réseaux de neurones (MLP / CNN / RNN)](#224-réseaux-de-neurones-mlp--cnn--rnn)
  - [2.3. Choix des modèles selon le type de tâche](#23-choix-des-modèles-selon-le-type-de-tâche)
    - [2.3.1. Problème de régression](#231-problème-de-régression)
    - [2.3.2. Problème de classification](#232-problème-de-classification)
- [3. Prétraitement des données](#3-prétraitement-des-données)
- [4. Définir le protocole expérimental](#4-définir-le-protocole-expérimental)
- [5. Choix des métriques](#5-choix-des-métriques)
  - [5.1. Pour les problèmes de régression](#51-pour-les-problèmes-de-régression)
  - [5.2. Pour les problèmes de classification](#52-pour-les-problèmes-de-classification)
    - [5.2.1. Jeu équilibré](#521-jeu-équilibré)
    - [5.2.2. Jeu déséquilibré](#522-jeu-déséquilibré)
  - [5.3. Visualisation](#53-visualisation)
- [6. Tuning des hyperparamètres](#6-tuning-des-hyperparamètres)
    - [Validation imbriquée (Nested Cross-Validation)](#validation-imbriquée-nested-cross-validation)
- [7. Validation et interprétation](#7-validation-et-interprétation)
  - [7.1. Comparer plusieurs modèles sur le même split](#71-comparer-plusieurs-modèles-sur-le-même-split)
  - [7.2. Choisir le modèle qui offre le meilleur compromis](#72-choisir-le-modèle-qui-offre-le-meilleur-compromis)
  - [7.3. Visualiser les résultats](#73-visualiser-les-résultats)
    - [Courbe ROC / AUC (classification)](#courbe-roc--auc-classification)
    - [Courbe  Precision-Recall (classification)](#courbe--precision-recall-classification)
    - [Matrice de confusion](#matrice-de-confusion)
    - [Courbe d'apprentissage (classification ou régression)](#courbe-dapprentissage-classification-ou-régression)
  - [Importance des *features* (arbres, forêts, boosting)](#importance-des-features-arbres-forêts-boosting)
    - [Graphique des résidus (régression)](#graphique-des-résidus-régression)
    - [Histogramme / QQ-plot des résidus (régression)](#histogramme--qq-plot-des-résidus-régression)
    - [Courbe de calibration (régression)](#courbe-de-calibration-régression)
    - [Courbes d'apprentissage (régression)](#courbes-dapprentissage-régression)
- [8. Cas spéciaux](#8-cas-spéciaux)

---

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
| **semi-supervisé** | une partie des données a une étiquette $y$, l'autre partie non | apprentissage sur 10% labellé + 90% non labellé |
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

1. Définir l'input et l'output : $X \in \mathbb{R}^{m \times d}$ ; $y \in \mathbb{R}^m$ ou $y \in \{0,1\}^m$[^1]

2. Formuler la tâche :
    - Régression : trouver $f : X \to \mathbb{R}$
    - Classification : trouver $f : X \to \{0,1\}$ ou $f : X \to \{1, \ldots, K\}$

3. Choisir l'objectif d'optimisation (minimiser une *loss*) :
  - Régression : MSE, RMSE, MAE
  - Classification : log loss, hinge loss, exponential loss

<small>"La surrogate loss **détermine le type de modèle** et le **comportement de l'optimisation**</small>

| Tâche | Loss idéale | Surrogate courante | Algorithme associé | Forme |
|-------|-------------|--------------------|--------------------|-------|
| **Classification binaire** | 0-1 loss | **Hinge loss** | SVM | $\ell \left( y, f(x) \right) = \max(0, 1- y f(x))$ |
| | 0-1 loss | **Logistic loss** | Régression logistique, Boosting | $\ell \left( y, f(x) \right) = \log(1 + e^{-y(f(x)})$ |
| | 0-1 loss | **Exponential loss** | AdaBoost[^1] | $\ell \left( y, f(x) \right) = e^{-yf(x)}$ |
| **Régression** | $L_2$ | MSE/RMSE | Linéaire, Ridge, SVR | $\left( y-f(x) \right)^2$ |
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
- cas > 2 variables → ACP (pour réduire à 2 dimensions) + pairplot (scatterplots pour toutes les paires de variables) : si aucune combinaison de 2 variables ne sépare bien, structure linéaire probable.

- frontière **linéaire** → modèle **linéaire** (SVM linéaire, régression logistique) suffisent.
- frontière **courbe/tordue** → approche à noyau (**SVM à noyau**) ou approche additive (**boosting non linéaire**)
     
###### Interactions entre variables

Une **interaction** se produit quand **l'effet d'une variable dépend d'une autre variable** :
- formellement :
    - $y = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 (x_1 x_2) + \beta_4 x_1^2 + \beta_5 x_2$ est un modèle polynomial du second degré où $x_1 x_2$ est le terme d'**interaction polynomiale**, $x_1^2$ et $x_2^2$ sont des termes **quadratiques** et $x_1$ et $x_2$ sont des termes **linéaires**[^2].
        - si $\beta_3 \gt 0$, l'effet de $x_1$ augmente quand $x_2$ augmente (et inversement) ;
        - si $\beta_3 \lt 0$, l'effet de $x_1$ diminue quand $x_2$ augmente (et inversement).
- exemple pratique : l'effet du sport sur la santé dépend de l'âge.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.preprocessing import PolynomialFeatures
poly = PolynomialFeatures(degree=2)
X_poly = poly.fit_transform(X)

# si le score augmente nettement, la relation est non-linéaire.
```
</details>

Les modèles linéaires ne capturent pas ces interactions, il faut :
- soit ajouter manuellement des termes croisées ($x_1 x_2$, x_1^2, \ldots) ;
- soit utiliser un modèle non linéaire (SVM à noyau, boosting, random forest...).

###### Variables fortement corrélées

1. calculer la **matrice de corrélation de Pearson** :

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
corr df.corr()
sns.heatmap(corr, cmap='coolwarm', annot=True)
```
</details>

2. si on a une paire avec $\vert corr \vert \gt 0.8$ → redondance forte.
3. conséquence : variables corrélées → modèle linéaire instable (les coefficients explosent)[^3].
4. solutions : ACP ; supprimer une des deux variables de la paire ; modèle à régularisation (Ridge, Lasso).

##### 2.1.1.2. Tester l'hypothèse de linéarité

**Objectif** : vérifier si la relation entre les variables explicatives $X$ et la cible $y$ peut être correctement **modélisée par une fonction linéaire**, ou si elle nécessite un **modèle non linéaire**.

##### 2.1.1.3. Démarche expérimentale

###### Protocole
1. **Fit un modèle linéaire simple**
    - si classification : régression logistique ou SVM linéaire.
    - si régression : régression linéaire simple ou ridge/lasso.
2. **Fit un modèle non linéaire** :
    - si classification : SVM RBF, arbre de décision, random forest, gradient boosting.
    - si régression : SVR RBF, RandomForestRegressor, XGBoostRegressor.
3. **Comparer les performances** :
    - même split train/test ;
    - même métrique ;
    - CV recommandée.
4. Si le modèle non-linéaire fait **significativement mieux** → la structure est non linéaire ; sinon, le modèle linéaire suffit.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.model_selection import cross_val_score
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC

lin = LogisticRegression(max_iter=1000)
rbf = SVC(kernel="rbf")

print(cross_val_score(lin, X, y, cv=5, scoring="roc_auc").mean())
print(cross_val_score(rbf, X, y, cv=5, scoring="roc_auc").mean())

# si RBF >> linéaire → non-linéarité confirmée.
```
</details>

###### Indicateurs complémentaires visuels et statistiques

1. **Analyse des résidus (régression)** : tracer `y_pred`vs `y_true`ou `residuals = y_true - y_pred` ; si les résidus[^4] montrent une **courbe** ou une **structure** → non-linéarité ; si les résidus sont répartis aléatoirement autour de 0 → linéarité plausible.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
sns.scatterplot(x=y_pred, y=y_test - y_pred)
plt.axhline(0, color='red', linestyle='--')
```
</details>

2. **Pairplot ou ACP pour visualiser les frontières** : pour on peut visualier les données (2D ou ACP 2D), séparation nette par une ligne droite → linéaire / frontière incurvée → non-linéaire.

<details>
    <summary>
        <span style="color:pink; font-weight:bold">Python</span>
    </summary>

```python
sns.pairplot(df, hue="target")
```
</details>

3. **Termes d'interaction / polynômes** : ajouter des termes $x_i^2$, $x_i x_j$ avec `PolynomialFeatures` ; si les scores s'améliorent beaucoup → la relation de base n'était pas linéaire.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.preprocessing import PolynomialFeatures
poly = PolynomialFeatures(degree=2)
X_poly = poly.fit_transform(X)
```
</details>

###### Critères quantitatifs à surveiller

- **AUC/R²** non-linéaire $\approx$ linéaire → relation linéaire suffisante → modèle linéaire ok.
- **AUC/R²** non-linéaire $gg$ linéaire → relation non-linéaire → tester RBF, arbres, boosting.
- **résidus** courbés/hétéroscédastiques[^12] → relation non-linéaire → transformation des variables ou modèle flexible.
    - si variance des résidus ↗ avec la valeur prédite → transformer la cible (log, sqrt, Box-Cox)
    - si effet d'une variable manquante -> ajouter un terme polynomial ou un terme di'nteraction
    - si bruit très inégal selon les classes → utiliser un modèle robuste (Random Forest, Gradient Boosting).
    - si résidus concentrés sur certains groupes → utiliser régression pondérée (Weighted Least Squares).
- **importances** très concentrées sur quelques variables → possible effet d'interaction → envisager termes croisés.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
# --- Tester l'homoscédasticité ---

## méthode visuelle: tracer les résidus en fonction des valeurs prédites.
## si on voit un effet de cône (dispersion croissant ou décroissante) → hétéroscédascticité.
import seaborn as sns
import matplotlib.pyplot as plt

y_pred = model.predict(X_test)
residuals = y_test - y_pred

sns.scatterplot(x=y_pred, y=residuals)
plt.axhline(0, color="red", linestyle="--")
plt.xlabel("Valeurs prédites")
plt.ylabel("Résidus")
plt.title("Diagnostic d'hétéroscédasticité")
plt.show()

## méthode statistique : test de Breusch-Pagan
## p-value < 0.05 → hétéroscédasticité ; p-value > 0.05 → homoscédasticité.
from statsmodels.stats.diagnostic import het_breuschpagan
import statsmodels.api as sm

X_sm = sm.add_constant(X_test)
test = het_breuschpagan(residuals, X_sm)
print(f"p-value = {test[1]:.4f}")
```
</details>

#### 2.1.2. Approche à noyau : Choisir un noyau (SVM/Kernel)

- On choisit un noyau *après* avoir sélectionné le modèle <u>SVM</u> et *avant* le tuning des hyperparamètres

| Situation | Type du noyau | Justification |
|-----------|---------------|---------------|
| Frontière simple/linéaire | **linéaire** | suffisant et rapide |
| Frontière courbe mais lisse (sans cassures brutales) | **RBF (Gaussien)** | bonne flexibilité, paramètre $\gamma$ à tuner |
| Frontière non lisse/polynomiale / Interaction polynomiale connue | **polynomial** | interprétable si petit degré |
| Volume de données élevé | **approximation RBF (RFF)** | gain de temps mémoire |
| Texte | **noyau de similarité cosinus** ou **TF-IDF kernel** | mesure de proximité sémantique |
| Graphe | **Graph kernel** (Weisfeller-Lehman, shortest-path) | compare des sous-structures |
| Séquences / Bio-informatique | **Noyau de sous-chaînes** (string kernel) | compare des motifs communs |
| Images | **Noyaux RBF / CNN** (*features* préentraînées | capture des formes et textures |
| Séries temporelles | **Dynamic Time Warping (DTW) kernel** | tolère des décalages temporels |

- Toujours tester au moins deux noyaux  et justifier le choix par une **métrique adaptée** et un **compromis biais/variance** :

| Cas | Métrique adaptée |
|-----|------------------|
| **Classification équilibrée** | AUC-ROC |
| **Classification déséquilibrée** | F1 / AUC-PR |
| **Régression** | R² / RMSE

#### 2.1.3. Approche additive : Choisir un Boosting

- on choisit le boosting *après* avoir choisi la famille d'<u>algorithmes d'ensemble</u> et *avant* le tuning.

| Situation | Type de boosting | Justification |
|-----------|------------------|---------------|
| Petit dataset, peu de bruit | **AdaBoost** | simple, efficace, marge large[^2] |
| Données complexes[^5], bruit modéré | **Gradient Boosting** | apprentissage résiduel[^13], flexible |
| Dataset volumineux/*features* nombreuses | **XGBoost / LightGBM** | optimisé, parallèle, régularisé |
| Variables catégorielles dominantes | **CatBoost** | encodage intégré |
| Risque d'overfit fort | **↘ `learning_rate`**, ↗ `n_estimators` | meilleur compromis biais/variance |



| Cas | Métrique adaptée |
|-----|------------------|
| **Classification équilibrée** | AUC-ROC / Accuracy |
| **Classification déséquilibrée** | F1 / Balanced Accuracy |
| **Régression** | RMSE / MAE / R² |
| **Autres critères** | Variance CV, temps d'apprentissage |

#### 2.1.4 Déséquilibre des classes

- dataset équilibré → Accuracy, AUC, CrossEntropy.
- dataset déséquilibré → `class_weight='balanced'` ou **SMOTE + F1/Recall** ou modèle **additif de type boosting / ensemble**[^6]

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from imblearn.over_sampling import SMOTE
X_res, y_res = SMOTE().fit_resample(X, y)
```
</details>

### 2.2. Choix de la famille de modèles

#### Objectif
Choisir une **famille algorithmique en fonction de :
- la **nature du problème** (linéaire / non-linéaire),
- la **taille / bruit / structure** des données,
- la *priorité** (interprétabilité, perfomance, rapidité, etc.)

<details><summary><span style="color:lightblue; font-weight:bold">Résumé heuristique</span></summary>

| Contrainte principale | Famille recommandée |
|-----------------------|---------------------|
| Peu de données, frontière simple | **SVM linéaire** |
| Données bruitées ou non linéaires | **SVM RBF** ou **Random Forest** |
| Données volumineuses, variables hétérogènes | **XGBoost / LightGBM** |
| Données séquentielles, image, texte | **Réseaux de neurones** |
| Priorité à l'interprétabilité | **Régression logistique / Arbres** |
| Priorité à la performance brute | **Gradient Boosting** |

<small><u>Exemple de rédaction</u> : "J’ai choisi un modèle de type boosting car il gère les interactions non linéaires, les données déséquilibrées et fournit des mesures d’importance de variables, sans nécessiter de normalisation préalable."</small>

</details>

#### Tableau comparatif synthétique

| Famille | Cas d’usage idéal | Forces | Faiblesses | Prétraitement | Métriques adaptées |
|----------|------------------|---------|-------------|----------------|--------------------|
| **SVM** (linéaire ou à noyau) | Peu de features, frontière nette, données propres | Excellente généralisation, robuste aux outliers | Lent si dataset volumineux, tuning sensible | Standardisation obligatoire | AUC / F1 / R² |
| **Boosting** (Ada, GB, XGB, LGBM, CatBoost) | Données tabulaires, patterns complexes, légère non-linéarité | Très performant, gère le déséquilibre, peu de tuning initial | Risque d’overfit si trop d’estimators, training lent | Peu sensible au scaling | AUC / RMSE / F1 |
| **Forêts aléatoires** | Dataset hétérogène, bruit modéré, besoin de robustesse | Facile à calibrer, peu sensible aux outliers, importance des features | Peu interprétable, moins précis que boosting | Pas de normalisation nécessaire | Accuracy / R² / F1 |
| **Réseaux de neurones** | Données massives ou non tabulaires (image, texte, séquences) | Très flexible, capture des interactions complexes | Très lent à converger, tuning lourd, overfit facile | Normalisation indispensable | Accuracy / CrossEntropy / RMSE |

#### 2.2.1 SVM (Support Vector Machines)

- Recommandé si **frontière nette** ou **petit nombre de features**.  
- Permet de choisir entre linéaire et RBF selon la complexité.  
- Attention au scaling et aux hyperparamètres $C$ et $\gamma$.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.svm import SVC
model = SVC(kernel='rbf', C=1, gamma=0.1, probability=True)
model.fit(X_train_scaled, y_train
```
</details>

#### 2.2.2. Boosting (AdaBoost, GradientBoosting, XGBoost, LightGBM, CatBoost)

- Recommandé pour **jeux tabulaire** avec forte variabilité.
- Apprend résidu par résidu → bon compromis biais/variance.
- Évite souvent la normalisation.
- XGBoost/LightGBM = top sur Kaggle-type datasets.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from xgboost import XGBClassifier
model = XGBClassifier(
    n_estimators=300,
    learning_rate=0.1,
    max_depth=5,
    subsample=0.8,
    colsample_bytree=0.8,
    eval_metric='auc'
)
model.fit(X_train, y_train)
```
</details>

#### 2.2.3. Forêts aléatoires (Random Forest)

- Bon **point de départ** pour tout jeu tabulaire.
- Gère les features mixtes (numériques / catégorielles).
- Moins de tuning que le boosting, mais plus lent à scorer.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(
    n_estimators=200,
    max_depth=None,
    class_weight='balanced',
    random_state=42
)
model.fit(X_train, y_train)
```
</details>

### 2.2.4. Réseaux de neurones (MLP / CNN / RNN)

- À réserver aux **données nombreuses** ou non tabulaires.
- Tuning exigeant : `learning_rate`, `epochs`, `batch_size`, `hidden_layers`.
- Requiert **scaling** et parfois **early stopping**.

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.neural_network import MLPClassifier
model = MLPClassifier(
    hidden_layer_sizes=(64, 32),
    activation='relu',
    learning_rate_init=0.001,
    max_iter=500,
    early_stopping=True
)
model.fit(X_train_scaled, y_train)
```
</details>


### 2.3. Choix des modèles selon le type de tâche

#### 2.3.1. Problème de régression

| Algorithmes classiques | Avantages | Limites |
|------------------------|-----------|---------|
| Régression linéaire / Ridge / Lasso | Interprétable, rapide | Sensible à la colinéarité, sous-ajustement |
| SVR (linéaire / RBF) | Bonné généralisation, robuste | Paramètres $C$ et $\gamma$ à tuner |
| Random Forest Regressor | Gère la non-linéarité, pas de normalisation | Peu interprétatble, lent si gros dataset |
| Gradient Boosting / XGBoost | Très performant, gère les features hétérogènes | Sensible au surapprentissage[^7] |

#### 2.3.2. Problème de classification

| Algorithmes classiques | Avantages | Limites |
|------------------------|-----------|---------|
| Régression logistique | Interprétable, baseline solide | Frontière linéaire uniquement |
| SVM (linéaire / RBF) | Performant sur les données peu bruitées | Sensible au scaling / $C$ / $\gamma$ |
| k-NN | Simple, non-paramétrique | Coût élevé en test, choix du $k$ |
| Random Forest | Gère les données mixtes, importance des variables ; stable | Peu explicatif sur les décisions |
| Gradient Boosting | Efficace sur déséquilibre | Long à entraîner, long à tuner[^8] |
| Réseaux de neurones | Très flexible[^9] | Requiert bcp de données + tuning lourd |

## 3. Prétraitement des données

1. Valeurs manquantes → imputation (moyenne / médiane / mode)
2. Données hétérogènes → encodage (one-hot, label encoding)
3. Variables corrélées → réduction (ACP, sélection de features)
4. Classes déséquilibrées → `class_weight`, oversampling/undersampling, SMOTE

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.preprocessing import StandardScaler, OneHotEncoder
from sklearn.compose import ColumnTransformer

preprocess = ColumnTransformer([
    ("num", StandardScaler(), num_features),
    ("cat", OneHotEncoder(), cat_features)
])
```
</details>

## 4. Définir le protocole expérimental

1. Split : Train / Validation / Test → 70/15/15 ou $k$-fold CV (5-CV)
2. Choix de la métrique principale
3. Comparaison des modèles selon performance + temps d'apprentissage
4. Interprétation / validation finale

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.model_selection import train_test_split
X_train, X_temp, y_train, y_temp = train_test_split(X, y, test_size=0.3, stratify=y)
X_val, X_test, y_val, y_test = train_test_split(X_temp, y_temp, test_size=0.5)
```
</details>

## 5. Choix des métriques

**Règle heuristique rapide** :
- dataset équilibré → **Accuracy / AUC**
- dataset déséquilibré → **F1/Recall/MCC**
- priorité à "ne pas rater" → **Recall**
- priorité à "ne pas se tromper" → **Precision**

<details><summary><span style="color:lightblue; font-weight:bold">Détails</span></summary>

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
</details>

### 5.1. Pour les problèmes de régression

| Métrique | Formule | Définition | Avantages / Inconvénients | Pour données... |
|----------|---------|------------|---------------------------|-----------------|
| **MSE** (Mean squared Error) | $\frac{1}{n} \sum_i (y_i - \hat{y}_i)^2$ | Erreur quadratique moyenne.</br>Très sensible aux valeurs extrêmes | pénalise fortement les grandes erreurs, mais amplifie les outliers | homogènes |
| **RMSE** (Root MSE) | $\sqrt{MSE}$ | Même unité que la variable cible. | plus intuitif, mais mêmes limites que MSE | homogènes |
| **MAE** (Mean Absolute Error | $\frac{1}{n} \sum_i \vert y_i - \hat{y}_i \vert $ | Erreur absolue moyenne, plus robuste. | moins sensible aux gros écarts, mais moins différenciant | avec outliers |
| **$R^2$** (Coefficient de détermination | $1 - \frac{\sum (y_i - \hat{y}_i)^2}{\sum (y_i - \bar{y}_i)^2 }$ | Part de la variance expliquée (max = 1) | intuitif mais pas stable et varie peu | |

<u><str>NB</str></u> : si comparaison de modèles → toujours préciser l'unité (ex.: erreur moyenne de 5°C).

### 5.2. Pour les problèmes de classification

#### 5.2.1. Jeu équilibré

| Métrique | Formule | Définition / Usage | Avantages / Inconvénients | Rédaction |
|----------|---------|------------|-------------------------------|-----------|
| **Accuracy** | $\frac{TP+TN}{TP+TN+FP+FN}$ | pourcentage de bonnes prédictions | simple, mais trompeur si classes déséquilibrées | 
| **AUC ROC** (Area Under the Curve of the Receiver Operating Characteristic) | $AUC = \int_0^1 TPR(FPR) \text{ d}(FPR)$</br>    $\simeq \sum_{i=1}^{n-1} (FPR_{i+1} - FPR_i) \times \frac{TPR_{i+1} + TPR_i}{2}$ | • capacité à discriminer les classes.</br>• maths : aire sous la courbe ROC (TPR vs FPR), calculée en pratique comme une somme de trapèzes</br>• stats : $AUC = \mathbb{P}(\text{score positif} \gt text{score négatif})$ (probabilit qu'un positif ait un score supérieur à un négatif) | AUC = 0.5 → modèle aléatoire (aucun pouvoir discriminant)</br>AUC = 0.7-0.8 → correct</br>AUC = 0.8-0.9 → bon</br>AUC > 0.9 → excellent</br>AUC = 1.0 → parfait (souvent sur-apprentissage</br>L'AUC est **indépendante du seuil** → pratique mais peut **masquer** une mauvaise calibration des probabilités | La courbe ROC trace la sensibilité (TPR) en fonction du taux de faux positifs (FPR). L'aire sous la courve (AUC) mesure la capacité du modèle à classer un positif au-dessus d'un négatif. Une AUC = 1 correspond à un classifieur parfait, 0.5 à un modèle aléatoire. |
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

0. toujours tuner `random_state`ou fixer la graine pour reproductibilité
1. split train / validation / test
2. CV 5-fold
3. GridSearchCV ou RandomizedSearchCV
4. Comparer sur **métrique adaptée** (pas forcément *Accuracy*)
5. vérifier la **stabilité des scores** variance basse → modèle robuste)

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.model_selection import GridSearchCV, cross_val_score, KFold
from sklearn.svm import SVC

# Boucle interne : tuning
inner_cv = KFold(n_splits=3, shuffle=True, random_state=42)
param_grid = {'C': [0.1, 1, 10], 'gamma': [0.01, 0.1, 1]}
grid = GridSearchCV(SVC(kernel='rbf'), param_grid, cv=inner_cv, scoring='roc_auc')

# Boucle externe : évaluation non biaisée
outer_cv = KFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(grid, X, y, cv=outer_cv, scoring='roc_auc')

print(f"AUC moyenne : {scores.mean():.3f} ± {scores.std():.3f}")
```
</details>

| Modèle | Hyperparamètres clés | Comment / Pourquoi | Effet du réglage | Astuce / Pièges |
|---------|----------------------|--------------------|------------------|-----------------|
| **SVM (RBF)** | C (régularisation) ; γ (largeur du noyau RBF) | `GridSearchCV` sur C ∈ [0.1, 1, 10], γ ∈ [0.01, 0.1, 1] | C ↗ → surapprentissage ; γ ↗ → frontière trop fine | Toujours standardiser les *features* |
| **Random Forest** | `n_estimators` ; `max_depth` ; `min_samples_split` ; `max_features` | | Plus d'arbres → meilleure stabilité mais plus lent ; profondeur ↗ → surfit | Commencer par peu de profondeur |
| **Gradient Boosting** | `n_estimators` ; `learning_rate` ; `max_depth` ; `subsample` | Petit `learning_rate` → réduit le surapprentissage mais nécessite plus d'estimators | | Commencer avec `learning_rate = 0.1` |
| **Ridge / Lasso** | λ = α (force de régularisation) | Tuning par validation croisée | λ ↗ → coefficients plus petits → biais ↗, variance ↘ [^10] | |
| **k-NN** | k, distance | | Petit $k$ → surfit ; grand $k$ → biais fort | Garder $k$ impair, scaling obligatoire |
| **NN (réseau de neurones)** | `learning_rate` ; `hidden_layers` ; `epochs` ; `batch_size` | | `learning_rate` trop haut → divergence | Surveiller la courbe de perte ; *early stopping* recommandé |
| **Arbre de décision** | `max_depth` ; `min_samples_split` ; `criterion` | | Profondeur ↗ → surfit | *Pruning* recommandé |
| **XGBoost** | `eta` ; `max_depth` ; `colsample_bytree` ; `lambda` | Paramètres très interdépendants | | Tuning itératif par bloc |

- Toujours normaliser les données pour SVM / k-NN / NN.
- Toujours vérifier la stabilité par $k$-fold (5-CV par défaut).
- tuning noyaux : $C$ (si SVM linéaire) ou $C$ et $\gamma$ (si SVM RBF)
- tuning boosting : `n_estimators`, `learning_rate`, `max_depth` et `subsample`, parfois `lambda` (régularisation) et `colsample_bytree` (échantillonnage des *features*).

<details><summary><span style="color:pink; font-weight:bold">Python</span></summary>

```python
from sklearn.model_selection import GridSearchCV

grid = GridSearchCV(SVC(), {
    "C": [0.1, 1, 10],
    "gamma": [0.01, 0.1, 1]
}, cv=5, scoring="roc_auc")
grid.fit(X_train, y_train)
```
</details>

#### Validation imbriquée (Nested Cross-Validation)

Une **validation imbriquée** (*nested CV*) combine deux boucles de validation croisée :
- une **boucle interne** pour le **tuning des hyperparamètres** (`GridSearchCV` ou `RandomizedSearchCV`)
- une **boucle externe** pour **évaluer la performance réelle** du modèle optimisé.

Cette approche fournit une **estimation non biaisée** de la performance finale, car le jeu de test externe n’a **jamais été vu** pendant le tuning.

<small>Exemple : 5-fold externe × 3-fold interne → 15 fits totaux.</small>

## 7. Validation et interprétation

### 7.1. Comparer plusieurs modèles sur le même split

**Bonnes pratiques** :
- **même preprocessing et même train/test** pour tous les modèles (sinon, comparaison biaisée).
- **même métrique** pour tous.
- **validation croisée §$k$-fold)** :
     - par défaut, $k = 5$.
     -  **stratifiée**[^14] si classification déséquilibrée.
     -  rapporter moyenne $\pm$ écart-type.
  
### 7.2. Choisir le modèle qui offre le meilleur compromis

**Trois axes de comparaison** :

| Axe | Exemples de critères | Objectif |
|-----|----------------------|----------|
| **Performance** | AUC, F1, RMSE, R²... | précision globale |
| **Complexité** | nb de paramètres, temps d'apprentissage | efficacité computationnelle |
| **Robustesse** | stabilité des résultats (variance CV), stabilité des *features* | fiabilité sur échantillons nouveaux |

### 7.3. Visualiser les résultats

| Type de tâche | Visualisation | Ce qu'elle montre | Interprétation attendue | Exemple de rédaction |
|---------------|---------------|-------------------|-------------------------|----------------------|
| **Classification** | **Courbe ROC / AUC** | compromis entre rappel (TPR) et faux positifs (FPR) pour tous les seuils | AUC proche de 1 → excellente discrimination entre classes | <small>"Le SVM à noyau RBF atteint une AUC de 0,93, traduisant une forte capacité de discrimination."</small> |
| **Classification déséquilibrée** | **Courbe PR (Precision-Recall)** | compromis entre précision et rappel, utile si classes déséquilibrées | courbe proche du coin supérieur droit = bon équilibre précision/rappel | <small>"Le modèle conserve une précision stable jusqu’à un rappel de 0,8, indiquant une gestion correcte du déséquilibre."</small> |
| **Classification** | **Matrice de confusion** | répartition des prédictions correctes et erreurs par classe | permet de repérer les classes sous-prédites | <small>"La classe minoritaire est mal détectée (rappel 0,62), ce qui justifie un rééquilibrage ou un seuil adapté.".</small> |
| **Tous** | **Courbes d'apprentissage** | surfit/sous-fit : évolution du score train/validation selon la taille de l'échantillon | écart large = overfit ; écart faible = bon compromis | <small>"Le modèle surapprend : le score train reste élevé tandis que le score validation stagne."</small> |
| **Modèles d'arbres | **Importance des features** (forêts, boosting) | contribution de chaque variable à la prédiction | identifie les variables dominantes ; attention aux corrélations dans l'interprétation | <small>"Les variables de revenu et d’âge expliquent 70 % de la variance prédictive."</small> |
| **Régression** | **Graphique des résidus vs valeurs prédites** | qualité d'ajustemnet : résidus centrés autour de 0 ? | répartition aléatoire = bon modèle ; structure = biais / non-linéarité | <small>"Les résidus sont homogènes, aucune structure apparente : l’ajustement est correct."</small> |
| **Régression** | **Histogramme ou QQ-plot des résidus** | vérifie la normalité et la symétrie des errurs | distribution centrée et gaussienne = modèle conforme aux hypothèses | <small>"Les résidus suivent une distribution quasi normale, validant l’hypothèse de linéarité."</small> |
| **Régression** | **Résidus standardisés ou Cook's distance** | détexte les points influents / outliers | valeurs extrêmes → observation atypique à évaluer | <small>"Deux points présentent une distance de Cook > 1, suggérant une influence excessive." | | **Courbe de calibration ($\hat{y}$ vs $y$ réel)** | fidélité du modèle : les prédicitions suivent-elles la diagonale idéale ? | points proches de la diagonale → bon calibrage | <small>"La courbe de calibration est proche de la diagonale, le modèle prédit sans biais systématique"</small> |
| | **Courbes d'apprentissage (train/test)** | sur- ou sous-apprentissage selon taille de l'échantillon | écart large → surfit ; scores faibles → underfit | <small>"Le modèle est trop complexe : la perte test augmente après 200 itérations."</small> |

<details ><summary><span style="color:pink; font-weight:bold">Python</span></summary>

En résumé :
- `sklearn.metrics` $\rightarrow$ pour ROC, PR, AUC, confusion, displays.
- `sklearn.model_selection` $\rightarrow$ pour `learning_curve`.
- `matplotlib`/`seaborn` $\rightarrow$ pour tracer le reste.
- `statsmodels`/`scipy`$\rightarrow$ pour les résidus et diagnostics avancés.


#### Courbe ROC / AUC (classification)

Version qui fonctionne pour tous les classifieurs ayant `predict_proba()` ou `decision_function()`[^11] :

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

## 8. Cas spéciaux

| Situation | Réponse attendue |
|-----------|------------------|
| Classes déséquilibrées | stratified CV + `class_weight` + FA/Recall |
| Bcp de variables | ACP | sélection de features / Lasso |
| Peu de données | modèles simples + régularisation forte |
| Temps de calcul critique | préférer un modèle linéaire ou Random Forest |
| Besoin d'interprétabilité | régression logistique, arbres |
| Surapprentissage suspecté | learning curve, réduction complexité, early stopping |



---
[^1]: AdaBoost (comme SVM) cherche à **maximiser une marge moyenne**, càd la **séparation moyenne entre les classes**. Chaque faible classifieur ou *weak learner* (arbre de profondeur 1) est pondéré pour **augmenter la confiance dans les échantillons bien classés** et **corriger les erreurs**. Résultat : le modèle final a une **grande marge effective** → meilleure généralisation, moins de variance.
[^2]: Mathématiquement, $x_1 x_2 = x_2 x_1$ (interaction symétrique).
[^3]: Explosion des coefficients en cas de colinéarité : colinéarité → coefficients instables → Ridge ou ACP nécessaire.
[^4]: Résidus : erreurs du modèles représentant les différences entre $y$ réel et $y$ prédit ($e_i = y_i - \hat{y}_i$).
[^5]: Données complexes = structures non linéaires, bruit, variables fortement dépendantes (ex.: prévisions financières, médicales).
[^6]: Algorithmes d'ensemble : combinaison pondérées de modèles faibles (bagging, boosting).
[^7]: Gradient Boosting/XGBoost sont sensibles au surfit : surfit si trop trop d'itérations ou si `learning_rate` trop haut).
[^8]: Gradient Boosting nécessite un réglage (*tuning*) fin cat il est sensible aux hyperparamètres.
[^9]: Flexibilité d'un modèle : capacité à approximer des fonctions complexes.
[^10]: Biais et variance : biais ↗ = modèle simple (risque de sous-apprentissage) ; variance ↗ = modèle complexe (risque de surapprentissage).
[^11]: `RocCurveDisplay.from_estimator()` fonctionne avec tout modèle ayant `predict_proba()` ou `decision_function()`.
[^12]: Hétéroscédasticité : On parle d'**hétéroscédasticité** quand la variance des résidus **n'est pas constante** : les erreurs sont plus grandes pour certaines valeurs de $\hat{y}$ que pour d'autres. Autrement dit, si la dispersion des résidus augmante ou diminue avec la valeur prédite → variance non constante/hérétoscédasticité / si les résidus ont **variance stablë¨(bande homogène autour de 0) → homoscédasticité. Intérêt = **violation des hypothèses du modèle linéaire** → la régression linéaire suppose l'homoscédasticité ; si les résidus n'ont pas une variance constante, les coefficient du modèle linéaire restent valide **mais** les **tests statistiques** (t, F) deviennent biaisés, les **intervalles de confiance** ne sont plus fiables, et cela indique souvent que le modèle est **mal spécifié** (il manque un terme non-linéaire ou une transformation).
[^13]: Apprentissage résiduel : le fait d'entraîner chaque nouveau modèle non pas sur les **valeurs cibles** $y$ mais sur les **erreurs (résidus)** du modèle précédent ; autrement dit, fait d'apprendre à **corriger les erreurs des modèles précédents**. En pratique : le boosting apprend **séquentiellement** des modèles faibles (petits arbres), chaque modèle suivant essayant de **prédire les résidus** du modèle précédent.
[^14]: Validation croisée stratifiée : dans la classification, la **stratification** garantit que chaque fold de validation conserve la même proportion de classes que le jeu de données complet. Cela évite que certains folds soient déséquilibrés, ce qui peut biaiser l'évaluation du modèle.
