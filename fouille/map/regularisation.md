<!--
Title: "Concepts fondamentaux de ML - Optimisation et régularisation - Régularisation (L1, L2)"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Régularisation L1 et L2

La régularisation est une composante essentielle de l'**optimisation** en Machine Learning (ML), nécessaire pour garantir la capacité d'un modèle à bien se généraliser aux données inconnues, un concept désigné sous le nom de **minimisation du risque empirique Régularisé (RRM)**.

Le problème d'optimisation fondamental s'écrit généralement sous la forme:

$$
\min_{w \in \mathbb{R}^d} \frac{1}{m} \sum_{i=1}^m \ell(w, x_i) + \lambda f(w),
$$

où $f(w)$ est le terme de régularisation, souvent basé sur une norme (L1 ou L2), et $\lambda$ est l'hyperparamètre de régularisation.

Il existe deux fonctions de régularisation principales, basées sur les normes $L_1$ et $L_2$, chacune ayant un impact distinct sur le modèle appris.

## 1. La régularisation par la norme $L_2$ (Ridge)

La régularisation utilisant la norme $L_2$ est la plus couramment rencontrée et est souvent associée à la **Ridge Regression**.

### Forme et usage

Dans le cadre de l'optimisation, la fonction $f(w)$ est définie par :

$$
f: w \mapsto \Vert w \Vert_2 \text{ ou } \Vert w \Vert_2^2.
$$

- **Séparateurs à Vaste Marge (SVM)** : La régularisation $L_2$ est intrinsèque à la formulation du problème du **Soft Margin SVM**. Le terme minimisé est $\frac{1}{2} \Vert w \Vert_2^2$.
- **Objectif en SVM** : Minimiser $\Vert w \Vert_2^2$ revient à **maximiser la marge** $\gamma = \frac{2}{\Vert w \Vert_2}$ entre les deux classes.
- **Autres modèles** : La norme $L_2$ est utilisée en régression logistique et dans les modèles linéaires de régression.

### Impact sur le mmdèle

Le rôle de la norme $L_2$ est d'empêcher que certains paramètres $w$ ne prennent des **valeurs trop importantes** comparativement aux autres paramètres du modèle. En d'autres termes, elle vise à contrôler l'amplitude des poids.

### Lien avec la généralisation (stabilité)

La régularisation $L_2$ joue un rôle essentiel dans la théorie de la généralisation basée sur la **stabilité uniforme** .
- La constante de stabilité uniforme ($\beta$) est inversement proportionnelle à $\lambda$.
- L'expression de la borne de généralisation $R_{\ell(\theta_S) \le R_{\ell}^S}(\theta_S) + 2 \beta + (4m \beta + K) \sqrt{\ln(\delta^{-1})/2m}$ montre que plus β$\beta$ est petit, plus la garantie de généralisation est forte.
- Par conséquent, une augmentation de l'hyperparamètre $\lambda$ (régularisation plus forte) rend l'algorithme plus stable (diminution de $\beta$) et produit une **borne de généralisation plus serrée**.

## 2. La régularisation par la norme $L_1$ (Lasso)

La régularisation utilisant la norme $L_1$ est définie par : $f: w \mapsto \Vert w \Vert_1$. Lorsque cette norme est utilisée pour la régression linéaire, on parle de **Lasso Regression**.

### Impact sur le modèle (parcimonie)

Le principal impact de la norme $L_1$ est d'induire la **parcimonie** (*sparsity*) dans l'hypothèse apprise.
- La régularisation $L_1$ tend à forcer un **minimum de paramètres** à être non nuls.
- Elle est particulièrement utile dans les m**odèles de haute dimension** (c'est-à-dire avec un grand nombre de variables, comme en génétique) pour la **sélection de variable**s, car elle met en évidence les attributs les plus importants pour la tâche de prédiction.

### Problème d'optimisation

Contrairement à la norme $L_2^2$, la norme $L_1$ présente un inconvénient majeur : le terme de régularisation est **non-différentiable** à tous les points, ce qui complique l'application des méthodes d'optimisation standards comme la descente de gradient classique.

## 3. Le rôle de l'Hyperparamètre $\lambda$

Dans le cadre du RRM, l'hyperparamètre $\lambda$ (souvent remplacé par $C$ dans la formulation primal du SVM, où $\lambda = 1/2C$) est le paramètre clé qui doit être ajusté (*tuned*).
- Il contrôle le **compromis** (*trade-off*) entre l'ajustement aux données (minimisation du risque empirique) et la simplicité/complexité du modèle (régularisation).
- Si $\lambda$ est petit, l'ajustement aux données est privilégié, augmentant le risque de **surapprentissage** (*over-fitting*).
- Si $\lambda$ est grand, la simplicité du modèle est privilégiée, ce qui peut mener à un **sous-apprentissage** (*under-fitting*).
- Le réglage de $\lambda$ est généralement effectué par des méthodes comme la validation **croisée $k$-fold** sur l'ensemble d'entraînement pour trouver la valeur qui donne les meilleures performances moyennes.


