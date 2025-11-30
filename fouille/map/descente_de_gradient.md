<!--
Title: "Concepts fondamentaux de ML - Optimisation et régularisation - Algorithmes - Descente de gradient"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Descente de gradient

La **descente de gradient** est l'algorithme d'optimisation fondamental qui soutient la grande majorité des **algorithmes** d'apprentissage machine, en particulier dans le contexte de la minimisation du risque empirique régularisé (RRM).

## 1. Objectif et principe de la descente de gradient

L'objectif de l'optimisation en ML est de déterminer la solution optimale $u^*$ qui minimise une fonction de coût (ou fonction objectif) f$(v)$. La descente de gradient est une méthode itérative visant à construire une séquence de valeurs $(u_k)$ qui converge vers ce minimum $u^*$.

### Le principe de la descente la plus raide

La méthode repose sur l'approximation au premier ordre de la fonction $ff. Pour minimiser la fonction $f$ à chaque étape, il faut choisir la direction de descente $d_k$ qui maximise le produit interne $\langle \nabla f(u_k), d_k \rangle$. En vertu de l'inégalité de Cauchy-Schwarz, cette direction est le **gradient négatif** de la fonction objectif ($d_k = \nabla f(u_k)$). C'est la direction de la descente la plus raide. C'est la direction de la *descente la plus raide*.

Le schéma de mise à jour pour trouver la nouvelle position $u_{k+1}$est donc défini par :

$$
u_{k+1} = u_k - \rho_k \nabla f(u_k).
$$

L'algorithme se répète jusqu'à ce que la norme du gradient de $f$ soit suffisamment proche de zéro ($\Vert \nabla f(u_k) \Vert \le \eta$), car les solutions optimales $u^\star$ vérifient $\nabla f(u^\star) = 0$ (équation d'Euler).

## 2. Le rôle crucial du pas d'apprentissage ($\rho_k$)

La performance et la convergence de l'algorithme dépendent essentiellement du choix du pas d'apprentissage $\rho_k$ (le *learning rate*).

### 2.1. Descente de gradient à pas constant

Le choix le plus simple est d'utiliser un pas constant $\rho_k = \rho \gt 0$. Cependant, un pas constant n'est pas idéal :
- Si $\rho$ est trop petit, la **vitesse de convergence** est lente.
- Si $\rho$ est trop grand, l'algorithme peut **diverger**.

### 2.2. Descente de gradient à pas optimal

L'approche la plus efficace théoriquement consiste à choisir $\rho_k$ qui minimise la fonction le long de la direction donnée. Le pas optimal est donné par la solution de l'optimisation unidimensionnelle :

$$
\rho_k = \arg \min_{\rho \gt 0} f(u_k - \rho \nabla f(u_k)).
$$

Lorsque ce pas optimal est utilisé, une propriété fondamentale est que **les deux directions de descente successives sont orthogonales** :

$$
\langle \nabla f(u_{k+1}), \nabla f(u_k) \rangle = 0.
$$

our les problèmes quadratiques (où $f(u) = \frac{1}{2} \langle Au, u \rangle -\langle b,u \rangle$), cette orthogonalité permet de dériver une solution analytique pour le pas optimal $\rho_k$. De plus, pour les fonctions **fortement convexes** ($\alpha$-elliptiques), la descente de dradient à pas optimal est garantie de converger.

## 3. Application dans les algorithmes et alternatives

La descente de gradient est l'épine dorsale de plusieurs algorithmes de ML et d'optimisation avancée :

### 3.1. Régression logistique

La descente de gradient est essentielle pour la régression Logistique. Étant donné qu'il n'existe **pas de solution analytique** (forme fermée) pour minimiser la *negative log-likelihood* (la *loss*), les paramètres sont estimés en utilisant la descente de gradient avec le calcul du gradient de la fonction de perte. La descente de gradient de **Newton-Raphson** est souvent utilisée dans ce contexte pour son taux de convergence plus rapide.

### 3.2. Algorithmes de Boosting (Gradient Boosting)

Le concept de descente de gradient s'étend aux méthodes ensemblistes complexes comme le **Gradient Boosting**.
- À chaque itération $t$, l'algorithme apprend un modèle faible $h_t$ qui minimise le risque en se basant sur les **pseudo-résidus** ($\hat{y}_i$). Ces pseudo-résidus sont définis comme le **gradient négatif** de la fonction de perte $\ell$ par rapport à la prédiction courante $H—{t-1}(x_i)$.
- e pas d'apprentissage $$\alpha_t pour le nouveau modèle $h_t$ est également choisi de manière à **minimiser la perte le long de la direction du gradient fonctionnel**, ce qui est équivalent à trouver un pas optimal.

### 3.3. Méthode de Newton

La méthode de Newton est une variante de la descente de gradient qui utilise la dérivée seconde de la fonction (la matrice Hessienne, $\nabla^2 f(u_k)$) pour affiner la direction de descente :

$$
u_{k+1} \leftarrow u_k - \left( \nabla^2 f(u_k) \right)^{-1} \cdot \nabla f(U_k).
$$

Bien qu'elle nécessite moins d'itérations pour converger, elle est **plus difficile à implémenter** en raison de la nécessité de calculer et d'inverser la matrice Hessienne (complexité en $O(n^3)$), qui n'est pas toujours inversible.

## 4. Outils d'optimisation pratiques

Dans les implémentations pratiques, surtout pour gérer la complexité de l'optimisation, des méthodes sophistiquées sont employées :
- **Méthodes Quasi-Newton** : Pour pallier l'inversion coûteuse de la Hessienne dans la méthode de Newton, des algorithmes comme **BFGS** (*Broyden–Fletcher–Goldfarb–Shanno*) sont utilisés. Ces algorithmes approximent l'inverse de la Hessienne itérativement.
- **Librairies** : L'implémentation de ces solveurs, y compris les méthodes basées sur la descente de gradient comme BFGS, peut être réalisée à l'aide de la librairie `scipy.optimize` en Python, notamment la fonction `optimize.fmin_l_bfgs_b`. De même, des plateformes comme **H2O** sont conçues pour exécuter des algorithmes d'apprentissage (comme le Gradient Boosting) dans des environnements distribués, optimisant ainsi les calculs.

La descente de gradient est le moteur qui permet aux algorithmes de ML d'atteindre la solution optimale en parcourant itérativement l'espace des paramètres dans la direction de la minimisation de l'erreur.




