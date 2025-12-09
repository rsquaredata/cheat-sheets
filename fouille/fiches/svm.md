<!--
Title: "Big Data Mining - Fiches - SVM"
Author: rsquaredata
Last updated: 2025-12-09
-->

# Séparateurs à Vaste Marge (SVM)

## 1. Version simple

### 1.1. Idée intuitive

Un SVM cherche **l'hyperplan qui sépare le mieux** deux classes *avec la plus grande marge* possible.  
> Maximiser la marge = maximiser la distance entre l'hyperplan et les points les plus proches (les vecteurs supports).

### 1.2. Formulation du problème (SVM linéaire, soft margin)

On ccherche $w$, $b$ qui résolvent :  

$$
\min_{w, b, \xi} \frac{1}{2} \Vert w \Vert^2 + C \sum_{i=1}^m \xi_i
$$

sous contraintes :  

$$
y_i (w^\top x_i + ) \ge 1 - \xi_i, \quad \xi_i \gt 0.
$$

Interprétation :
- $\Vert w \Vert^2$ : terme de régularisation (contrôle la marge).
- $C$ : pénalisation des erreurs (plus $C$ est grand → modèle plsu rigide.
- $\xi_i$ : slack; erreurs sur les données non séparables.  

### 1.3. Règles de classification

Pour un point $x$ :  $\hat{y} = \text{sign}(w^\top w + b)$.  

Dans le cas à noyau : $\hat{y} = \text{sign} \left( \sum_{i=1}^m \alpha_i y_i K (x_i, x) + b \right)$.

### 1.4. Hinge loss

Le SVM minimise la **hinge looss** : $L(y, f(x)) = \max(0, 1-yf(x))$.  

Propriétés :  
- impose de **correctement classer** ET avec une **marge d'au moins 1**
- valeur 0 si marge respectée
- pénalisation linéaire si marge non respectée
- favorise des marges larges

> La hinge loss encourage une bonne séparation avec marge, tout en permettant quelques erreurs via les slacks.

### 1.5. Vecteurs support

Ce sont les points **sur la marge** ou **mal classés** :  
- ce sont les **seuls** points qui déterminent $w$
- si on les enlève → l'hyperplan change
- les autres points, loin des marges, n'influencent pas la frontière

### 1.6. Noyaux (version courte)

On remplace le produit scalaire par une **fonction noyau** : $K(x,x') = \langle \phi(x), \phi(x') \rangle$.  
→ permet de séparer non linéairement **sans calculer explicitement** $\phi(x)$ (*kernel trick*).  

Exemples de noyaux :  
- linéaire
- RBF (gaussien)
- polynomial

Limites pratiques des noyaux :
- matrice de Gram $K$ de taille $m \times m$ → coûte $O(m^2)$ en mémoire
- résolution du dual $O(m^3)$ → impossible pour $m$ grand (genre > 100k)
- pas adapté au Big Data → on passe à linear SVM (SGD) ou méthodes approximatives

## 2. Version hardcore

### 2.1. Lagrangien du SVM

On introduit des multiplicateur de Lagrange $\alpha_i$ et $\beta_i$ :  

$$
L(w, b, \xi, \alpha, \beta) = \frac{1}{2} \Vert w \Vert^2 + C \sum_{i=1}^m \xi_i - \sum_{i=1}^m \alpha_i \left[ y_i (w^\top x_i + b) - 1 + \xi_i \right] - \sum_{i=1}^m \beta_i \xi_i. 
$$

### 2.2. Conditions d’annulation (∂L/∂w, ∂L/∂b, ∂L/∂ξ)

En annulant les dérivées, on obtient :  

$$
w = \sum_{i=1} \alpha_i y_i x_i
$$

$$
\sum_{i=1}^m \alpha_i y_i = 0
$$

$$
\alpha_i \le C
$$

$$
\beta_i = C - \alpha_i
$$

### 2.3. Dual du SVM

Après substitution :

$$
\max{\alpha} \sum_{i=1}^m \alpha_i - \frac{1}{2} \sum_{i,j} \alpha_i \alpha_j y_i y_j K(x_i, x_j)
$$

sous contraintes :  

$$
0 \le \alpha_i \le C, \quad \sum_{i=1}^m \alpha_i y_i = 0
$$

→ c'est ce dual qui est résolu dans les SVM kernelisés.  

### 2.4. Hypothèses sur un noyau (conditions de Mercer)

Un noyau $K$ doit être :  
- **symétrique** : $K(x,x') = K(x',x)
- **défini positif** : matrices $K_{ij} = K(x_i, x_j$ PSD

### 2.5. Approximation des méthodes à noyaux

Deux techniques :  

1. **Méthodes des landmarks (Nystrom)** :
    - on sélectionne $L$ points et on approxime le noyau complet par une factorisation
    - réduit le coût de calcul  

2. **Random Fourier Features (RFF)** :  
    - on approxime un noyau RBF par une projection aléatoire
    - transforme un problème kernel en problème linéaire  

## 3. SVM : Points-clés

- cherche **l'hyperplan à marge maximale**
- les **vecteurs supports** déterminent la frontière
- le **dual** permet d'utiliser les **noyaux**
- noyau = **produit scalaire dans un espace projeté**
- limites : $O(m^2)$ mémoire + $O(m^3)$ calcul
- solutions approximatives : Nystrom, RFF








