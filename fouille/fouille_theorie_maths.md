<!--<!--
Title: "fouille_theorie_math"
Author: rsquaredata
Last updated: 2025-12-09
-->

# Fouille de données massives - Théorie & Mathématiques  
---

## Table des matières

1. [Notions générales](#1-notions-générales)
2. [Apprentissage supervisé vs non-supervisé](#2-apprentissage-supervisé-vs-non-supervisé)
3. [Surrogate loss & optimisation](#3-surrogate-loss--optimisation)
4. [SVM linéaire](#4-svm-linéaire)
5. [Problème dual & vecteurs de support](#5-problème-dual--vecteurs-de-support)
6. [SVM à noyau - Kernel Trick](#6-svm-à-noyau---kernel-trick)
7. [Méthodes à noyaux - types et propriétés](#7-méthodes-à-noyaux---types-et-propriétés)
8. [Boosting et Gradient Boosting](#8-boosting-et-gradient-boosting)
9. [Big Data & Machine Learning scalable](#9-big-data--machine-learning-scalable)
10. [Résumé formules clés](#10-résumé-formules-clés)

---

## 1. Notions générales

| Terme | Définition courte | Formule / Interprétation |
|-------|--------------------|--------------------------|
| **Observation** | Instance de données, notée $x_i \in \mathbb{R}^d$ | vecteur ligne |
| **Feature (variable explicative)** | composante de $x_i$ | $x_i = [x_{i1}, \dots, x_{id}]$ |
| **Cible** | valeur à prédire | $y_i \in \{-1, +1\}$ (classif) ou $y_i \in \mathbb{R}$ (régr.) |
| **Modèle** | fonction $f : X \to Y$ estimée sur un échantillon | $f(x) = \hat{y}$ |
| **Hypothèse** | forme paramétrique de $f$ | ex. linéaire : $f(x) = w^\top x + b$ |
| **Risque empirique** | erreur moyenne sur l’échantillon | $R(f) = \frac{1}{n}\sum_i L(y_i, f(x_i))$ |
| **Risque attendu** | espérance du risque sur la vraie distribution | $E_{(x,y)}[L(y,f(x))]$ |
| **Biais / Variance** | décomposition de l’erreur | Erreur = Biais² + Variance + bruit irréductible |

---

## 2. Apprentissage supervisé vs non-supervisé

| Type | Objectif | Exemples |
|------|-----------|----------|
| **Supervisé** | prédire $y$ à partir de $x$ | régression, classification |
| **Non-supervisé** | explorer la structure de $X$ sans $y$ | clustering, ACP, détection anomalies |
| **Semi-supervisé** | partiellement étiqueté | pseudo-labelling, auto-encoder |
| **Auto-supervisé** | labels générés à partir des données | masquage (BERT), contrastif |
| **Renforcement** | apprendre une politique via récompense | RL, bandits multi-bras |

---

## 3. Surrogate loss & optimisation

### 3.1. Idée clé
- La **0-1 loss** ($L(y,f(x)) = \mathbb{1}_{(yf(x) \lt 0)}$) n’est pas dérivable → on la **remplace** par une *surrogate loss* convexe.  
- Chaque *surrogate loss* conduit à un **modèle différent**.

| Tâche | Loss idéale | Surrogate courante | Algorithme | Expression |
|-------|--------------|--------------------|-------------|-------------|
| Classif. binaire | 0-1 loss | Hinge loss | **SVM** | $\max(0, 1 - y f(x))$ |
| Classif. proba | 0-1 loss | Log loss | **Régr. logistique, Boosting** | $\log(1+e^{-y f(x)})$ |
| AdaBoost | 0-1 loss | Exponential loss | **AdaBoost** | $e^{-y f(x)}$ |
| Régression | L₂ | MSE | Régr. linéaire / Ridge | $(y - f(x))^2$ |
| Régression robuste | L₁ | MAE | Lasso | $\lvert y - f(x) \rvert$ |

### 3.2. Fonction de coût globale

$$
J(w) = \frac{1}{n}\sum_{i=1}^{n} L(y_i, f(x_i)) + \lambda \, \|w\|^2
$$

- Premier terme : **ajustement** au jeu d’apprentissage  
- Deuxième terme : **régularisation** (contrôle de la complexité)

---

## 4. SVM linéaire

### 4.1. Principe
Trouver un **hyperplan séparateur** de marge maximale entre deux classes.

$$
\min_{w,b} \frac{1}{2}\|w\|^2 \quad \text{s.c.} \quad y_i(w^\top x_i + b) \ge 1
$$

### 4.2. Cas non séparable
On autorise des erreurs via les **variables d’écart** $\xi_i$ :

$$
\min_{w,b,\xi} \frac{1}{2}\|w\|^2 + C\sum_i \xi_i
\quad \text{s.c. } y_i(w^\top x_i + b) \ge 1 - \xi_i, \, \xi_i \ge 0
$$

- $C$ : paramètre de pénalisation (grande $C$ → peu d’erreurs mais surfit)
- La **marge** vaut $\frac{2}{ \vert w \vert }$.

### 4.3. Interprétation géométrique
- Hyperplan : $w^\top x + b = 0$
- Classes : $\text{sign}(w^\top x + b)$
- Vecteurs supports : points sur la marge $y_i(w^\top x_i + b) = 1$

---

## 5. Problème dual & vecteurs de support

### 5.1. Construction du dual (cas linéaire séparable)
On introduit les multiplicateurs de Lagrange $\alpha_i \ge 0$.

$$
L(w,b,\alpha) = \frac{1}{2}\|w\|^2 - \sum_i \alpha_i[y_i(w^\top x_i + b) - 1]
$$

Conditions :

$$
\begin{cases}
\nabla_w L = 0 \Rightarrow w = \sum_i \alpha_i y_i x_i \\
\nabla_b L = 0 \Rightarrow \sum_i \alpha_i y_i = 0
\end{cases}
$$

→ on obtient le **problème dual** :

$$
\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j (x_i^\top x_j)
\quad \text{s.c. } \alpha_i \ge 0, \; \sum_i \alpha_i y_i = 0
$$

### 5.2. Décision finale

$$
f(x) = \text{sign}\left(\sum_i \alpha_i y_i (x_i^\top x) + b \right)
$$

Seuls les points avec $\alpha_i > 0$ influencent $f$ → ce sont les **vecteurs de support**.

---

## 6. SVM à noyau - *Kernel Trick*

### 6.1. Motivation
Quand les données ne sont **pas séparables linéairement**, on projette $x$ dans un **espace de plus grande dimension** $\phi(x)$.

Mais au lieu de calculer $\phi(x)$ explicitement, on utilise une **fonction noyau** :

$$
K(x_i, x_j) = \langle \phi(x_i), \phi(x_j) \rangle
$$

### 6.2. Dual à noyau

$$
\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j K(x_i, x_j)
$$

et  

$$
f(x) = \text{sign}\left(\sum_i \alpha_i y_i K(x_i,x) + b\right)
$$

### 6.3. Conditions de validité d’un noyau
- Symétrie : $K(x_i, x_j) = K(x_j, x_i)$  
- Matrice de Gram $[K(x_i,x_j)]$ semi-définie positive (critère de Mercer)

<details><summary><span style="color:lightblue">Matrice semi-définie positive</span></summary>

**ELI5** :
- Imaginons une matrice $K$ qui contient toutes les “similarités” entre les points : $K_{ij} = K(x_i, x_j)$
  - Si deux points sont très proches → grande valeur
  - Si deux points sont différents → petite valeur
- On veut que cette matrice soit "cohérente", c’est-à-dire qu’elle ne puisse pas donner des distances négatives ni des comportements absurdes (comme "la similarité de moi avec moi est négative").
-  matrice **semi-définie positive** garantit ça : **aucune combinaison linéaire de lignes/colonnes ne produit une valeur négative**.
- En image : l’espace défini par le noyau **ne courbe pas l’espace dans un sens "non physique"** ; "l’énergie associée à toute combinaison de points est toujours positive ou nulle".

**Définition mathématique** :
Une matrice $K \in \mathbb{R}^{n \times n}$ est dite **semi-définie positive (SDP)** si : $\forall v \in \mathbb{R}^\top, \quad  v^\top Kv \ge 0$.  
Autrement dit :
- si on prend n’importe quel vecteur $v$ (de même taille que $K$),
- qu'on multiplie $v^\top K v$ (c’est un produit scalaire pondéré),
- le résultat doit **toujours être positif ou nul**.

**Interprétaion géométrique** :
- Une matrice SDP représente une **forme quadratique** qui **ne change pas le signe** du vecteur.
- Si elle est strictement positive ($v^\top K v > 0$ pour tout $v \ne 0$), elle est **définie positive**.
- Si elle peut valoir 0 pour certains $v \ne 0$, elle est **semi-définie positive**.

**Application au SVM et aux noyaux** :
- Dans un SVM à noyau, on remplace le produit scalaire $x_i^\top x_j$ par $K(x_i,x_j)$, ce qui revient à construire une matrice noyau :  

$$
K = \begin{pmatrix}
K{x_1,x_1} & K{x_1,x_2} & \ldots & K{x_1,x_n} \\
K{x_2,x_1} & K{x_2,x_2} & \ldots & K{x_2,x_n} \\
\vdots &  & \ddots & \vdots \\
K{x_n,x_1} & K{x_n,x_2} & \ldots & K{x_n,x_n}
\end{pmatrix}
$$

- Pour que le noyau soit valide, il faut que cette matrice soit **symétrique et semi-définie positive** : c’est la condition de Mercer. Sinon, le SVM pourrait se comporter de manière instable (marge négative, énergie impossible, etc.).

**Comment vérifier q'une matrice est SDP** :  
1. **Via les valeurs propres** : $K$ est SDP \Leftrightarrow toutes ses valeurs propres $\lambda_i \ge 0$.

```python
import numpy as np

K = np.array([[2, -1], [-1, 2]])

eigvals = np.linalg.eigvals(K)
print(eigvals)   # [3. 1.] → toutes positives ⇒ SDP
```

2. **Via la condition $v^\top Kv \ge 0$** :

```python
v = np.random.randn(K.shape[0])
print(v.T @ K @ v)  # doit être ≥ 0
```

3. **Via `np.allclose` sur la symétrie** :

```python
np.allclose(K, K.T)   # doit être True
```

</details>

---

## 7. Méthodes à noyaux - types et propriétés

| Type | Expression | Paramètres | Usage typique |
|-------|-------------|-------------|---------------|
| **Linéaire** | $K(x,x') = x^\top x'$ | – | données linéaires |
| **Polynomial** | $(x^\top x' + c)^d$ | degré $d$, cste $c$ | interactions polynomiales |
| **RBF / Gaussien** | $\exp(-\gamma \|x - x'\|^2)$ | $\gamma$ (largeur du noyau) | frontières lisses non linéaires |
| **Sigmoïde** | $\tanh(\kappa x^\top x' + c)$ | $\kappa, c$ | inspiration réseaux de neurones |
| **DTW / string / graph kernels** | spécifiques au domaine | - | séries, textes, graphes |

→ Choix du noyau = compromis entre **flexibilité** et **surapprentissage**.

---

## 8. Boosting et Gradient Boosting

### 8.1. AdaBoost (Adaptive Boosting)
Principe : combiner plusieurs *weak learners* (arbres peu profonds) entraînés séquentiellement.

1. Initialiser des poids $w_i = 1/n$  
2. Pour chaque itération $t$ :
   - entraîner $h_t(x)$ sur les données pondérées  
   - calculer l’erreur pondérée $\varepsilon_t = \sum_i w_i \mathbb{1}(h_t(x_i) \ne y_i)$  
   - pondérer le modèle : $\alpha_t = \frac{1}{2}\log\frac{1-\varepsilon_t}{\varepsilon_t}$  
   - mettre à jour les poids : $w_i \leftarrow w_i e^{-\alpha_t y_i h_t(x_i)}$ puis normaliser  

Prédiction finale :

$$
F(x) = \text{sign}\left(\sum_t \alpha_t h_t(x)\right)
$$

→ Maximisation de la **marge moyenne**, améliore la robustesse.

---

### 8.2. Gradient Boosting

- Vue comme une **descente de gradient fonctionnelle** :
  chaque nouveau modèle apprend à **corriger les résidus** du précédent.

$$
F_0(x) = \arg\min_\gamma \sum_i L(y_i, \gamma)
$$

$$
r_{im} = -\left[\frac{\partial L(y_i, F(x_i))}{\partial F(x_i)}\right]_{F=F_{m-1}}
$$

$$
F_m(x) = F_{m-1}(x) + \nu \, h_m(x)
$$

où $\nu$ = *learning rate*.

| Variante | Particularités |
|-----------|----------------|
| **XGBoost** | régularisation L₁/L₂, parallélisation, `colsample_bytree` |
| **LightGBM** | histogrammes, croissance feuille-par-feuille |
| **CatBoost** | encodage catégoriel natif |

---

## 9. Big Data & ML scalable

| Problème | Solution / Approche | Exemple |
|-----------|---------------------|----------|
| Données trop volumineuses pour la RAM | **Apprentissage incrémental** | `partial_fit()` (SGDClassifier) |
| Données distribuées | **MapReduce / Spark MLlib** | entraînement distribué |
| Données hétérogènes | **Feature hashing, encodage distribué** | NLP, logs |
| Modèle trop lent à tuner | **RandomizedSearch / Hyperband** | tuning adaptatif |
| Pipeline complet | **MLflow, Airflow, Dataiku, scikit-pipeline** | MLOps |

---

## 10. Résumé formules clés

| Concept | Formule / Expression | Interprétation |
|----------|---------------------|----------------|
| **Hyperplan SVM** | $w^\top x + b = 0$ | frontière de décision |
| **Marge** | $\frac{2}{\|w\|}$ | distance entre hyperplans supports |
| **Dual SVM** | $\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{ij}\alpha_i\alpha_j y_i y_j K(x_i,x_j)$ | optimisation sur les $\alpha$ |
| **Décision** | $f(x)=\text{sign}\left(\sum_i \alpha_i y_i K(x_i,x) + b\right)$ | classification |
| **RBF kernel** | $\exp(-\gamma\|x-x'\|^2)$ | projection implicite |
| **Gradient boosting** | $F_m(x)=F_{m-1}(x)+\nu h_m(x)$ | apprentissage résiduel |
| **AdaBoost poids** | $w_i \leftarrow w_i e^{-\alpha_t y_i h_t(x_i)}$ | ré-pondération des erreurs |
| **AUC ROC** | $AUC = \int_0^1 TPR(FPR)\,dFPR$ | pouvoir discriminant |
| **Biais–Variance** | $E[(y-\hat{f})^2] = (\text{biais})^2 + \text{variance} + \text{bruit}$ | compromis apprentissage |

---

**Exemples de questions typiques d'examen**  
*(et angle de réponse rapide)*  

| Sujet | Question possible | Réponse courte attendue |
|-------|--------------------|--------------------------|
| SVM | "Donner la forme du problème primal et du dual" | Primal : minimiser ½‖w‖² + C∑ξᵢ ; Dual : maximiser Σαᵢ – ½ΣΣαᵢαⱼyᵢyⱼK(xᵢ,xⱼ) |
| Boosting | "Quelle différence entre AdaBoost et Gradient Boosting ?" | AdaBoost : pondère les erreurs ; Gradient Boosting : apprend les résidus. |
| Kernel | "Pourquoi utiliser un noyau ?" | Pour capturer la non-linéarité sans calculer explicitement la projection φ(x). |
| Théorie | "Qu’est-ce qu’une surrogate loss ?" | Une fonction convexe qui approxime la 0-1 loss pour rendre l’optimisation dérivable. |
| Big Data | "Citer une méthode d’apprentissage scalable" | SGD incrémental / Spark MLlib / mini-batch learning. |

---

## Annexes - Mini-dérivations

### A. SVM linéaire → problème dual

1. **Étape 1 - problème primal** : On cherche l’hyperplan séparateur le plus large : $\min_{w,b} \frac{1}{2}\|w\|^2 \quad \text{s.c. } y_i(w^\top x_i + b) \ge 1$

2. **Étape 2 - Lagrangien** : On introduit les multiplicateurs de Lagrange $\alpha_i \ge 0$ : $\mathcal{L}(w,b,\alpha) = \frac{1}{2}\|w\|^2 - \sum_i \alpha_i[y_i(w^\top x_i + b) - 1]$

3. **Étape 3 - conditions de Karush-Kuhn-Tucker (KKT)** :

$$
\begin{cases}
\frac{\partial \mathcal{L}}{\partial w} = 0 \Rightarrow w = \sum_i \alpha_i y_i x_i \\
\frac{\partial \mathcal{L}}{\partial b} = 0 \Rightarrow \sum_i \alpha_i y_i = 0
\end{cases}
$$

4. **Étape 4 - remplacer dans le Lagrangien** :
    - $\Rightarrow \mathcal{L}(\alpha) = \sum_i \alpha_i - \frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j (x_i^\top x_j)$
    - → **Dual du SVM linéaire** : $\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j (x_i^\top x_j)
\quad \text{s.c. } \alpha_i \ge 0, \; \sum_i \alpha_i y_i = 0$

5. **Étape 5 - fonction de décision** :
    - $f(x) = \text{sign} \left(\sum_i \alpha_i y_i (x_i^\top x) + b \right)$
    - Les $\alpha_i > 0$ sont les **vecteurs de support**.

---

### B. Passage au noyau (Kernel Trick)

1. On remplace le produit scalaire $x_i^\top x_j$ par un **noyau** $K(x_i,x_j)$ : $K(x_i,x_j) = \phi(x_i)^\top \phi(x_j)$
2. → Nouveau dual : $\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j K(x_i, x_j)$
3. → Décision : $f(x) = \text{sign} \left(\sum_i \alpha_i y_i K(x_i, x) + b\right)$

---

### C. Gradient Boosting – dérivation conceptuelle

1. **Étape 1 - objectif général** : On cherche à minimiser la perte empirique : $J(F) = \sum_{i=1}^n L(y_i, F(x_i))$, avec $F(x)$ = modèle global (somme des arbres précédents).

2. **Étape 2 - descente de gradient fonctionnelle** :
    - On approxime la descente de gradient non pas sur des **paramètres**, mais sur des **fonctions** : $r_{im} = -\left[\frac{\partial L(y_i, F(x_i))}{\partial F(x_i)}\right]_{F=F_{m-1}}$
    - Ces $r_{im}$ sont les **pseudo-résidus**.

3. **Étape 3 - apprentissage d’un modèle sur les résidus** : On entraîne un modèle $h_m(x)$ tel que : $h_m(x_i) \approx r_{im}$

4. **Étape 4 - mise à jour du modèle global** : $F_m(x) = F_{m-1}(x) + \nu h_m(x)$, où $\nu \in (0,1]$ est le *learning rate*.
      → Chaque nouveau modèle apprend à **corriger les erreurs du précédent**.

---

### D. AdaBoost – dérivation simplifiée

1. **Étape 1 - pondération initiale** : $w_i^{(1)} = \frac{1}{n}$
2. **Étape 2 - erreur pondérée du classifieur $h_t$** : $\varepsilon_t = \sum_i w_i^{(t)} \mathbb{1}(h_t(x_i) \neq y_i)$
3. **Étape 3 - pondération du modèle** : $\alpha_t = \frac{1}{2}\ln\frac{1-\varepsilon_t}{\varepsilon_t}$
4. **Étape 4 - mise à jour des poids** : $w_i^{(t+1)} = w_i^{(t)} e^{-\alpha_t y_i h_t(x_i)}$  puis normalisation : $\sum_i w_i^{(t+1)} = 1$
5. **Étape 5 - combinaison finale** : $F(x) = \text{sign} \left(\sum_t \alpha_t h_t(x)\right)$

---

### E. Dualité, KKT et interprétation géométrique

| Élément | Interprétation intuitive |
|----------|--------------------------|
| **Dualité** | Permet d’exprimer le problème uniquement en fonction des produits scalaires (utile pour le kernel trick). |
| **KKT** | Conditions nécessaires d’optimalité reliant primal et dual. |
| **Vecteurs de support** | Points sur la marge : ils seuls influencent la position de l’hyperplan. |
| **Marge large** | Maximiser la distance aux classes → meilleure généralisation (biais ↘, variance ↘). |

---

### F. Liens entre AdaBoost, Gradient Boosting et SVM

| Point commun | Explication |
|---------------|-------------|
| **Marge large** | AdaBoost et SVM cherchent à maximiser une marge moyenne. |
| **Loss convexe** | Hinge loss (SVM) ↔ Exponential loss (AdaBoost). |
| **Itératif / Séquentiel** | Boosting ajoute des classifieurs faibles, SVM ajuste un hyperplan. |
| **Gradient view** | Boosting ≈ descente de gradient fonctionnelle. |

---

### G. Exemples de calculs rapides (type examen)

#### a. Calculer le nombre de vecteurs support
Si le dual donne 20 $\alpha_i > 0$ sur 200 points, alors la **marge** dépend uniquement de ces 20 points.

#### b. Produit scalaire et marge

$\text{Marge} = \frac{2}{\|w\|} = \frac{2}{\sqrt{w_1^2 + w_2^2 + \dots + w_d^2}}$

##### i. Produit scalaire dans un noyau RBF (gaussien)

$K(x_i, x_j) = \exp(-\gamma \|x_i - x_j\|^2)$  

Exemple : $x_i=(1,2)$, $x_j=(3,1)$, $\gamma=0.5$  
→ $\|x_i-x_j\|^2 = (1-3)^2+(2-1)^2=5$  
→ $K= e^{-0.5×5} = e^{-2.5} ≈ 0.082$

##### ii. Marge d'un SVM linéaire

$\text{Marge} = \frac{2}{\|w\|} = \frac{2}{\sqrt{w_1^2 + w_2^2 + \dots + w_d^2}}$  

Exemple :  $w=(2,1)$ → → $\|w\| = \sqrt{5}$ → marge $= 2/\sqrt{5} \approx 0.894$

##### iii. Fonction de décision du SVM

$f(x) = \text{sign} \left(\sum_i \alpha_i y_i K(x_i, x) + b \right)$  

Exemple :  
3 vecteurs support avec $\alpha y = [0.5, -0.3, 0.2]$, $K(x_i,x)=[1,0.5,0.1]$, $b=0.1$  
→ $f(x)=\text{sign}(0.5×1 -0.3×0.5 + 0.2×0.1 + 0.1)=\text{sign}(0.47)=+1$  


#### b. Exemple de kernel polynomial

$$
K(x,x') = (x^\top x' + 1)^2 = (x_1x'_1 + x_2x'_2 + 1)^2
$$

→ expansion = $x_1^2x_1'^2 + 2x_1x_2x'_1x'_2 + \dots$ (introduit automatiquement des interactions).

#### c. Vérifier la validité d’un noyau
- Matrice $K$ symétrique  
- $\forall v$, $v^\top K v \ge 0$

#### 7.5. Losses usuelles (à savoir écrire)
| Nom | Expression | Dérivée |
|------|-------------|----------|
| Hinge | $\max(0, 1 - yf(x))$ | $-y$ si $yf(x)<1$, 0 sinon |
| Log loss | $\log(1 + e^{-y f(x)})$ | $-\frac{y}{1+e^{y f(x)}}$ |
| MSE | $(y-f(x))^2$ | $-2(y-f(x))$ |

---

### H. Comprendre le problème **primal vs dual** (ELI5)

#### a. Contexte
Un SVM cherche une **frontière** (droite, plan ou hyperplan) qui sépare au mieux deux classes.
Cette frontière est définie par $w$ et $b$ dans l’équation $w^\top x + b = 0$.

#### i. Le problème primal
C’est la **formulation directe** : "Je veux trouver le meilleur $w$ et $b$ pour maximiser la marge entre les classes."

$$
\min_{w,b} \frac{1}{2}\|w\|^2
\quad \text{s.c.} \quad y_i(w^\top x_i + b) \ge 1
$$

- $w$ = vecteur normal à la frontière  
- $b$ = position de la frontière  
- Objectif = avoir une marge grande (petite norme de $w$)


#### ii. Le problème dual
C’est la **formulation inversée** : Au lieu d’optimiser directement $w$ et $b$, on donne à chaque point $x_i$ un **poids** $\alpha_i$ qui indique "combien ce point pousse sur la frontière".

$$
\max_{\alpha_i \ge 0} 
\sum_i \alpha_i - \frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j (x_i^\top x_j)
$$

Seuls les points **sur la marge** (les “points charnières”) ont $\alpha_i > 0$ :
ce sont les **vecteurs de support**.

#### b. Pourquoi on s’en sert
- Le problème dual **dépend uniquement des produits scalaires** $(x_i^\top x_j)$.
- On peut donc les **remplacer par un noyau** $K(x_i,x_j)$ pour gérer la non-linéarité : $f(x) = \text{sign}\left(\sum_i \alpha_i y_i K(x_i, x) + b\right)$.
- Le SVM devient ainsi **non linéaire sans jamais calculer la projection**.

**Résumé simple :**
| Vue | Ce qu’on fait | Image mentale |
|------|---------------|---------------|
| **Primal** | On cherche directement la droite séparatrice $w,b$ | "Je trace la meilleure frontière possible." |
| **Dual** | On exprime le problème en termes d’influences $\alpha_i$ | "Chaque point tire plus ou moins fort sur la frontière." |
| **Kernel** | On remplace les produits scalaires par un noyau $K$ | "Je tords l’espace pour séparer les points sans effort." |

---

## Formulaire express

### A. Généralités
- **Produit scalaire** : $x_i^\top x_j = \sum_k x_{ik} x_{jk}$
- **Distance euclidienne** : $\|x_i - x_j\| = \sqrt{\sum_k (x_{ik} - x_{jk})^2}$
- **Norme** : $\|w\| = \sqrt{w_1^2 + \dots + w_d^2}$  
- **Marge du SVM** : $\displaystyle \text{Marge} = \frac{2}{\|w\|}$  
- **Régularisation** :
  - L1 (Lasso) → $\|w\|_1 = \sum_i |w_i|$
  - L2 (Ridge) → $\|w\|_2^2 = \sum_i w_i^2$

### B. SVM linéaire

#### D. Problème primal :
$min_{w,b} \frac{1}{2}\|w\|^2 \quad \text{s.c. } y_i(w^\top x_i + b) \ge 1$

#### ii. Lagrangien :
$\mathcal{L}(w,b,\alpha) = \frac{1}{2}\|w\|^2 - \sum_i \alpha_i[y_i(w^\top x_i + b) - 1]$

#### iii. Conditions KKT :
$\frac{\partial \mathcal{L}}{\partial w} = 0 \Rightarrow w = \sum_i \alpha_i y_i x_i$  

$\frac{\partial \mathcal{L}}{\partial b} = 0 \Rightarrow \sum_i \alpha_i y_i = 0$  

#### ii. Problème dual :
$\max_\alpha \sum_i \alpha_i - \frac{1}{2}\sum_{i,j}\alpha_i\alpha_j y_i y_j (x_i^\top x_j)$  

#### iii.  Fonction de décision :
$f(x) = \text{sign} \left(\sum_i \alpha_i y_i (x_i^\top x) + b \right)$  

### C. Kernel Trick (SVM non linéaire)

Remplacer $x_i^\top x_j$ par $K(x_i, x_j)$ : $K(x_i, x_j) = \phi(x_i)^\top \phi(x_j)  

#### i. Exemples : 
| Noyau | Formule | Hyperparamètres |
|-------|----------|-----------------|
| Linéaire | $K(x,x') = x^\top x'$ | aucun |
| Polynomial | $K(x,x') = (x^\top x' + c)^d$ | $c$, $d$ |
| RBF (gaussien) | $K(x,x') = \exp(-\gamma \|x-x'\|^2)$ | $\gamma$ |
| Sigmoïde | $K(x,x') = \tanh(\beta x^\top x' + \theta)$ | $\beta$, $\theta$ |

#### ii. Décision finale :
$f(x) = \text{sign} \left(\sum_i \alpha_i y_i K(x_i, x) + b \right)$

### D. Boosting

#### i. AdaBoost :
1. Poids initiaux : $w_i^{(1)} = \frac{1}{n}$
2. Erreur pondérée : $\varepsilon_t = \sum_i w_i^{(t)} \mathbb{1}(h_t(x_i) \neq y_i)$
3. Poids du modèle : $\alpha_t = \frac{1}{2}\ln\frac{1-\varepsilon_t}{\varepsilon_t}$
4. Mise à jour : $w_i^{(t+1)} = w_i^{(t)} e^{-\alpha_t y_i h_t(x_i)}$
5. Modèle final : $F(x) = \text{sign} \left(\sum_t \alpha_t h_t(x) \right)$

#### ii. Gradient Boosting :
1. Objectif : $\displaystyle J(F) = \sum_i L(y_i, F(x_i))$
2. Pseudo-résidus : $r_{im} = -\frac{\partial L(y_i, F(x_i))}{\partial F(x_i)}$
3. Apprentissage : $h_m(x) \approx r_{im}$
4. Mise à jour : $F_m(x) = F_{m-1}(x) + \nu h_m(x)$

### E. Loss functions

| Nom | Formule | Dérivée |
|------|----------|----------|
| Hinge | $\max(0, 1 - y f(x))$ | $-y$ si $y f(x)<1$, 0 sinon |
| Logistique | $\log(1 + e^{-y f(x)})$ | $-\frac{y}{1 + e^{y f(x)}}$ |
| Exponentielle | $e^{-y f(x)}$ | $-y e^{-y f(x)}$ |
| MSE | $(y-f(x))^2$ | $-2(y-f(x))$ |
| MAE | $|y - f(x)|$ | $\text{sign} (f(x)-y)$ |

### F. Métriques de performance

#### i. Classification :
- $\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}$
- $\text{Precision} = \frac{TP}{TP + FP} \quad \text{Recall} = \frac{TP}{TP + FN}$
- $F1 = 2 \times \frac{Precision \times Recall}{Precision + Recall}$
- $AUC = \int_0^1 TPR(FPR) \, d(FPR)$

#### ii. Régression :
- $MSE = \frac{1}{n}\sum_i (y_i - \hat{y}_i)^2$
- $RMSE = \sqrt{MSE}$
- $MAE = \frac{1}{n}\sum_i \ vert y_i - \hat{y}_i \vert $
- $R^2 = 1 - \frac{\sum_i (y_i - \hat{y}_i)^2}{\sum_i (y_i - \bar{y})^2}$

### G. Rappels pratiques

| Cas | Modèle recommandé | Hyperparamètres clés |
|-----|-------------------|----------------------|
| Frontière linéaire | SVM linéaire, régression logistique | $C$ |
| Frontière courbe | SVM RBF, boosting | $C$, $\gamma$ |
| Dataset petit et bruité | Ridge, Lasso, AdaBoost | $\lambda$, $\alpha$ |
| Dataset volumineux | RandomForest, XGBoost | `n_estimators`, `max_depth` |
| Classes déséquilibrées | Boosting, SVM + `class_weight` | `learning_rate`, `class_weight` |

### g. Interprétation des modèles
- **SVM** → marge large, points support, robustesse.  
- **AdaBoost** → pondère les erreurs, renforce la marge moyenne.  
- **Gradient Boosting** → apprend sur les résidus (descente de gradient fonctionnelle).  
- **Régularisation** → contrôle la complexité, réduit la variance.  
- **Dualité** → exprime le problème en fonction des produits scalaires, utile pour les noyaux.

### H. Conditions KKT (rappel rapide)

$$
\begin{cases}
\alpha_i \ge 0 \\
y_i(w^\top x_i + b) - 1 \ge 0 \\
\alpha_i[y_i(w^\top x_i + b) - 1] = 0
\end{cases}
$$

---

## Règles rapides à retenir

| Concept | Règle |
|----------|-------|
| Données fortement corrélées | → instabilité du modèle linéaire |
| Variance des résidus non constante | → hétéroscédasticité |
| Résidus centrés et aléatoires | → modèle bien spécifié |
| AUC ≈ 0.5 | → modèle aléatoire |
| AUC > 0.9 | → très bon modèle |
| Small $C$ (SVM) | → marge large (régularisation forte) |
| Large $C$ | → marge fine, risque de surfit |
| Small $\gamma$ (RBF) | → frontière lisse |
| Large $\gamma$ | → frontière tortueuse |

**Dernière minute**
- Lire les contraintes du primal → penser "droite + marge".  
- Lire le dual → penser "combinaison de points".  
- Produit scalaire → penser "mesure de similarité".  
- Hinge loss → marge large.  
- Boosting → apprentissage résiduel.  
- Gradient Boosting → descente de gradient sur fonctions.

---

💡 **Conseil d’examen**

- Si l’énoncé demande d"expliquer le rôle de la régularisation", toujours mentionner :
  - elle **contrôle la complexité du modèle**,
  - elle **évite le surapprentissage**,
  - et elle **améliore la stabilité** des coefficients / marges.

- Si une question demande une *interprétation géométrique*, toujours mentionner la notion de **marge** et de **vecteurs support**.

- Si c’est une question sur *les algorithmes d’ensemble*, évoquer **le compromis biais/variance** et **l’apprentissage séquentiel des erreurs**.</small>

---
