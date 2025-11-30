<!--
Title: "Concepts fondamentaux de ML - Optimisation et régularisation - Algorithmes - Méthode de Newton / Quasi-Newton (BFGS)"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Méthode de Newton / Quasi-Newton (BFGS) - Optimisation accélérée

Les méthodes de Newton et Quasi-Newton (BFGS) représentent des algorithmes d'optimisation avancés utilisés pour minimiser les fonctions de coût (ou de perte) dans le contexte plus large des algorithmes d'apprentissage automatique, en particulier pour les problèmes convexes et régularisés. Ces méthodes cherchent à accélérer la convergence par rapport à la descente de gradient classique en utilisant des informations de courbure (dérivées secondes) de la fonction objectif.

## 1. La méthode de Newton

La méthode de Newton est un algorithme de descente de gradient qui utilise la dérivée seconde de la fonction objectif pour déterminer la direction de descente la plus appropriée.

### Principe et formule

Le schéma de mise à jour pour la méthode de Newton est le suivant :

$$
u_{k+1} \leftarrow u_k - \left( \nabla^2 f(u_k) \right)^{-1} \cdot \nabla f(u_k),
$$

où $\nabla f(u_k)$ est le gradient et $\nabla^2 f(u_k)$ est la **matrice Hessienne** (la matrice des dérivées secondes) évaluée au point courant $u_k$.

### Avantages et inconvénients

- **Avantage** : La méthode de Newton nécessite **moins d'itérations pour converger** vers la solution optimale par rapport aux autres méthodes de descente de gradient.
- **Inconvénients** : Elle est plus difficile à implémenter. Le principal inconvénient réside dans la nécessité de calculer la matrice Hessienne et, surtout, de **calculer son inverse** à chaque itération. Cette inversion matricielle a une complexité algorithmique élevée, typiquement en $O(n^3)$, où $n$ est la dimension des données, ce qui rend la méthode très coûteuse en temps de calcul dans un contexte de grande dimension. De plus, la matrice Hessienne n'est pas toujours inversible en un point donné.

## 2. Les méthodes Quasi-Newton et le BFGS

Les méthodes Quasi-Newton ont été développées pour contourner les inconvénients liés au calcul et à l'inversion de la matrice Hessienne dans la méthode de Newton.

### Principe d'approximation

L'idée des méthodes Quasi-Newton est d'**approximer l'inverse de la matrice Hessienne** ($H_k^{-1}$) par une matrice $M_k$. Cette matrice d'approximation $M_k$ est ensuite mise à jour itérativement en lui ajoutant une matrice de correction $C_k$ à chaque étape ($M_{k+1} = M_k + C_k$).

Le schéma de mise à jour itérative devient alors :

$$
u_{k+1} = u_k - M_k \nabla f(u_k).
$$

La correction $C_k$ doit satisfaire la Condition Quasi-Newton ($M_{k+1} \gamma_k = \delta_k$), où $\gamma_k$ et $\delta_k$ sont des termes dérivés du gradient et de la différence des positions successives.

### L'algorithme BFGS (Broyden–Fletcher–Goldfarb–Shanno)

Le BFGS est l'une des solutions de type Quasi-Newton les plus utilisées en pratique.
- **Correction de rang 2** : BFGS utilise une correction de rang 2 pour définir la matrice $C_k$.
- **Mise à jour $M_{k+1}$** : La règle de mise à jour pour $M_{k+1}$ dans l'algorithme BFGS est définie de la manière suivante :

$$
M_{k+1} = M_k + \frac{\gamma_k \gamma_k^\top}{\gamma_k^\top \delta_k} - \frac{M_k \delta_k \delta_k^\top M_k^\top}{\delta_k^\top M_k \delta_k}.
$$

- **Implémentation pratique** : On utilise la fonction `optimize.fmin_l_bfgs_b` de la librairie `scipy.optimize` pour résoudre les problèmes d'optimisation. . Ceci montre que le BFGS est un solveur standard et efficace pour les problèmes réels en ML, car il est capable de gérer des contraintes et il est adapté pour les optimisations en grande dimension sans le coût de calcul de l'inverse de la Hessienne.

## 3. Application aux algorithmes de Machine Learning

Les méthodes de Newton et Quasi-Newton sont particulièrement pertinentes pour l'optimisation des modèles qui impliquent des fonctions objectifs convexes et (idéalement) lisses, comme la régression logistique.

- **Régression logistique** : La descente de gradient de **Newton-Raphson** est souvent utilisée pour minimiser la fonction de perte (le log-vraisemblance négatif) dans la régression logistique, car elle offre un taux de convergence rapide.
- **Propriétés de la Hessienne** : Dans le cas de la régression logistique, la matrice Hessienne $\nabla^2 \ell{w, X}$ est exprimée comme une combinaison linéaire positive de matrices de Gram et est donc garantie d'être une **matrice semi-définie positive (PSD)**. Cela est important car cela assure la convexité locale et facilite la recherche du minimum.
- **Optimisation dans les approches avancées** : Les méthodes Quasi-Newton (BFGS) sont également des solveurs génériques qui peuvent être utilisés pour optimiser les étapes de minimisation dans des algorithmes plus complexes, y compris ceux basés sur l'approximation de noyaux.



