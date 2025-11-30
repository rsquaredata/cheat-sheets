<!--
Title: "Apprentissage massif et données déséquilibrées - Concepts fondamentaux de ML - Risques et généralisation"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Généralisation : Risques, stabilité et complexité

Les concept de **risques** et de **généralisation** sont critiques dans le contexte des données massives où les modèles doivent être capables de maintenir leur performance sur des données futures inconnues.

La généralisation est la capacité d'une hypothèse apprise $h$ à partir d'un échantillon limité $S$ à performer de manière optimale sur de nouvelles données issues de la distribution inconnue $D$.

## 1. Définition des risques

L'apprentissage machine est formulé comme un problème d'optimisation visant à minimiser l'erreur du modèle. Cette erreur est mesurée par la notion de risque, qui se décline en deux formes principales :

### 1.1. Risque réel (True Risk, $R_{\ell}$)

Le risque réel $R_{\ell}(h)$ est l'espérance de l'erreur d'une hypothèse $h$ sur la distribution inconnue $D$ :

$$
R_{\ell}(h) = \mathbb{E}_{(x,y) \sim D} \left[\ell (h(x),y)\right]. 
$$

C'est la quantité que l'on cherche idéalement à minimiser.

### 1.2. Risque Empirique (Empirical Risk, $R_{\ell}^S$)

Puisque la distribution $D$ est toujours inconnue en pratique, on ne peut pas calculer le risque réel. On minimise à la place le risque empirique $R_{\ell}^S(h)$, qui est l'erreur moyenne de l'hypothèse $h$ sur l'échantillon d'entraînement $S$ :

$$
R_{\ell}^S(h) = \frac{1}{m} \sum_{i=1}^m \ell (h(x_i), y_i). 
$$

La minimisation du risque empirique est le critère d'optimisation principal de nombreux algorithmes (tels que les SVM).

## 2. Le problème de la généralisation

L'objectif de la théorie de la généralisation est de **borner l'écart** entre le risque réel et le risque empirique :

$$
\vert R_{\ell} - R_{\ell}^S \vert \le \varepsilon(\delta, m). 
$$

- Cette borne $\varepsilon(\delta, m)$ doit être **décroissante** en fonction du nombre d'exemples $m$ et dépend du niveau de confiance $1 - \delta$.
- Le fait de minimiser le risque empirique n'est pas suffisant pour garantir une bonne généralisation ; cela peut mener à l'**over-fitting** (surapprentissage).

Le phénomène de surapprentissage (over-fitting) se produit lorsque le modèle s'ajuste parfaitement aux données d'entraînement (faible risque empirique) mais est incapable de généraliser à de nouvelles données (risque réel élevé). Inversement, un modèle trop simple peut souffrir de **sous-apprentissage** (under-fitting).

Le but est de trouver un bon **compromis** entre la minimisation du risque empirique et la complexité du modèle, tel qu'illustré par l'évolution des deux risques en fonction de la complexité.

<p align="center">
  <img src="assets/empirical_vs_true_risk.png"
       alt="Évolution des risques et de la borne selon la complexité du modèle"
       style="width: 50%; max-width: 400px;">
</p>

## 3. Les mécanismes de contrôle de la généralisation

Pour éviter le surapprentissage et garantir la généralisation, différentes approches sont utilisées pour contraindre l'hypothèse apprise :

### 3.1. Minimisation du risque empirique régularisé (*Regularized Risk Minimization*)

C'est la solution la plus courante et la plus facile à mettre en œuvre en pratique. Elle consiste à ajouter un terme de **régularisation** au risque empirique dans la fonction objectif :

$$
\min_{w \in \mathbb{R}^d} \frac{1}{m} \sum_{i=1}^m \ell (w, x_i) + \lambda f(w). 
$$

- Le terme $\lambda f(w)$ contraint la complexité du modèle $w$.
- L'hyper-paramètre $\lambda$ contrôle le compromis : si $\lambda$ est petit, on privilégie l'ajustement aux données (risque de surapprentissage) ; si $\lambda$ est grand, on privilégie la simplicité du modèle (risque de sous-apprentissage).
-  La valeur de $\lambda$ doit être **réglée** (**_tuned_**) par des méthodes comme la validation croisée (*k-fold cross-validation*).

### 3.2. Théorie de la stabilité uniforme

Approche utilisée pour établir des bornes de généralisation, car elle ne repose pas sur une mesure de complexité de l'espace d'hypothèse.

#### Stabilité uniforme ($\beta$)

Un algorithme est uniformément stable avec une constante $\beta \gt 0$ si la modification d'un seul exemple dans l'échantillon d'entraînement $S$ n'entraîne qu'une faible modification de la fonction de perte $\ell$ pour les modèles appris ($\theta_S$ et $\theta_{S^i}$).

$$
\forall \text{ } S \text{, } \forall \text{ } i \text{, } 1 \le i \le m \text{, } \sup_{x} \vert \ell(\theta_S, x) - \ell(\theta_{s^i}, x) \vert \le \beta. 
$$

#### Borne via stabilité

L'Inégalité de McDiarmid est utilisée pour traduire cette propriété de stabilité en une borne de généralisation effective. Cette borne a un taux de convergence en $O(1/\sqrt{m})$.

### 3.3. Mesures de complexité du modèle

Autres "outils" pour construire des bornes de généralisation, principalement pour l'espace d'hypothèses $H$ :
- La **complexité de Rademacher**.
- La **dimension VC** (Vapnik-Chervonenkis).

Ces approches cherchent à établir des bornes de type Probablement Approximativement Correct (*Probably Approximately Correct* - PAC).

# 4. Décomposition de l'erreur

Pour mieux comprendre les problèmes de généralisation, l'erreur de généralisation peut être décomposée en trois termes (dans le cadre de la régression, basé sur l'erreur quadratique moyenne) :
1. **Biais au carré** : Représente la différence entre la prédiction moyenne du modèle sur l'ensemble des échantillons possibles et la vraie fonction $f(x)$. Un modèle simple a un biais élevé.
2. **Variance** : Mesure la sensibilité de l'hypothèse $h$ aux variations de l'échantillon d'entraînement $S$. Un modèle complexe a une variance élevée.
3. **Erreur de Bayes** : Erreur irréductible qui ne dépend que de la distribution des données.

Cette décomposition montre que pour améliorer la généralisation, il faut trouver un équilibre entre le biais et la variance. Les méthodes ensemblistes, comme le **Bagging** (utilisé dans les Random Forests), sont spécifiquement conçues pour réduire la variance des modèles complexes (arbres de décision profonds) tout en maintenant un faible biais, améliorant ainsi la généralisation.
