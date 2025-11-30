<!--
Title: "Apprentissage massif et données déséquilibrées - Concepts fondamentaux de ML - Optimisation et régularisation"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Optimisation et régularisation en Big Data

L'étude de l'optimisation et de la régularisation est un pilier fondamental de la théorie de l'apprentissage statistique (*Statistical Learning Theory*). Elle permet de traduire la recherche du "meilleur modèle" en un problème mathématique, tout en intégrant des mécanismes cruciaux pour assurer la généralisation face aux contraintes du Big Data.

Le processus d'apprentissage d'un modèle de *Machine Learning* est principalement formulé comme un problème de **minimisation du risque empirique régularisé** (RRM).

## 1. Formulation du problème d'optimisation

L'objectif d'un algorithme d'apprentissage est de déterminer une hypothèse $h$ (ou le vecteur de paramètres $w$) qui minimise une fonction de coût. Le problème d'optimisation typique est donné par :

$$
\min_{w \in \mathbb{R}^d} \frac{1}{m} \sum_{i=1}^m \ell(w, x_i) + \lambda f(w).
$$

### 1.1.  La minimisation du risque empirique

Le premier terme de cette formulation, $\frac{1}{m} \sum_{i=1}^m \ell(w, x_i)$, est le **risque empirique** $R_{\ell}^S$. Il représente l'erreur moyenne de l'hypothèse $h$ sur l'échantillon d'entraînement $S$.
- **Fonction de Perte ($\ell$)** : Le risque empirique est calculé à l'aide d'une fonction de perte ($\ell$) qui pénalise les erreurs. La perte naturelle 0−1 (qui compte le nombre d'erreurs) est souvent remplacée par des p**ertes substituts** (*surrogate losses*) convexes et différentiables, car la minimisation de la 0−1 loss est **NP-difficile**.
- E**xemples de pertes** : La *Hinge Loss* est utilisée pour l'algorithme des Séparateurs à Vaste Marge (SVM), la *Logistic Loss* pour la régression logistique, et l'*Exponential Loss* est employée dans le contexte du Boosting.

### 1.2. L'importance de la régularisation

La seule minimisation du risque empirique conduit souvent au **surapprentissage** (*over-fitting*). Le terme $\lambda f(w)$, appelé **terme de régularisation**, est ajouté pour contraindre la complexité du modèle et favoriser la **généralisation**.

## 2. Le terme de régularisation et son impact

Le terme $\lambda f(w)$ est composé de l'hyper-paramètre $\lambda$ et de la fonction de régularisation $f(w)$.

### 2.1. Le rôle de l'hyperparamètre $\lambda$

L'hyperparamètre $\lambda$ est un **paramètre de contrôle** qui définit le compromis (*trade-off*) entre l'ajustement aux données (minimisation de l'erreur) et la complexité du modèle (minimisation de $f(w)$).
-  Si $\lambda$ est petit, le modèle est plus ajusté aux données, augmentant le risque de surapprentissage.
-  Si $\lambda$ est grand, le modèle est plus simple, mais peut conduire à un **sous-apprentissage** (*under-fitting*).

La valeur de $\lambda$ doit être **réglée** (*tuned*) expérimentalement, souvent en utilisant la **validation croisée $k$-fold** (par exemple, 5-CV pour les SVM) sur l'ensemble d'entraînement.

### 2.2. Types de régularisation ($f(w)$)

Le choix de la fonction $f$ impacte la structure du modèle appris :
1. **Norme $L_2$ ($f(w) = \Vert w \Vert_2$ ou $\Vert w \Vert_2^2$)** : Utilisée pour éviter que les paramètres ne prennent des valeurs trop importantes (Ridge Regression). Dans le cas des SVM (Soft Margin SVM), le terme de régularisation est $\frac{1}{2} \Vert w \Vert_2^2$, et sa minimisation est équivalente à la **maximisation de la marge** entre les classes.
2. **Norme $L_1$ ($f(w) = \Vert w \Vert_1$)** : Utilisée pour induire la **parcimonie** (*sparsity*). Elle force un minimum de paramètres à être non nuls, ce qui est particulièrement utile en haute dimension pour la sélection de variables (Lasso Regression).

### 2.3. Régularisation et généralisation théorique

Les bornes de généralisation basées sur la stabilité uniforme montrent explicitement le lien entre l'hyperparamètre de régularisation $\lambda$ et la capacité du modèle à généraliser . La constante de stabilité uniforme ($\beta$) d'un SVM est inversement proportionnelle à $\lambda$ :

$$
\beta = \frac{2 \sigma^2}{\lambda m}.
$$

Ainsi, si $\lambda$ augmente (régularisation plus forte), la constante $\beta$ diminue, rendant l'algorithme plus stable et garantissant une **borne de généralisation plus serrée**.

## 3. Optimisation et contraintes du Big Data

L'optimisation des problèmes de Machine Learning implique souvent la recherche de solutions $w^{*}$ où le gradient de la fonction objectif est nul.

- **Algorithmes d'optimisation** : Pour résoudre le problème de minimisation, surtout lorsque les fonctions sont convexes et lisses, on utilise des algorithmes de **descente de gradient** (G*radient Descent*), ou des méthodes basées sur des approximations de la Hessienne comme l'algorithme **BFGS** (Broyden–Fletcher–Goldfarb–Shanno). La librairie `scipy.optimize` est un exemple de package utilisé pour ces tâches d'optimisation.
- **Convexité** : Il est crucial de s'assurer que le problème d'optimisation, comme celui du Soft Margin SVM (avec Hinge Loss), est **convexe** pour garantir qu'il n'y ait pas de minima locaux (uniquement un minimum global pour les problèmes strictement convexes).

### Défis des méthodes à noyaux en données massives

Dans le contexte de la fouille de données massives, les algorithmes qui nécessitent des calculs complexes durant l'optimisation sont très limités. Les **méthodes à noyaux** (comme le SVM non linéaire) sont particulièrement affectées :
- Elles nécessitent le calcul d'une matrice de similarités $K \in \mathbb{R}^{m \times m}$ entre les points du jeu de données, ce qui implique $m$2$ imilarités, où $m$ est le nombre d'exemples.
- La résolution du problème dual implique souvent l'inversion de cette matrice $K$, ce qui est **irréalisable** en Big Data.
- La complexité de résolution du problème primal du Soft Margin SVM est en $O(d^3)$ (où $d$ est la dimension des données), ce qui est trop long pour les grandes dimensions.

Pour surmonter ces contraintes, il devient nécessaire d'utiliser des **approximations**.

## 4. Solutions d'optimisation adaptées (Approximation et Boosting)

Face aux contraintes d'optimisation sur les grands jeux de données, il existe des alternatives qui accélèrent le processus :
1. **Approximation des méthodes à noyaux** : L'idée est de réduire la complexité de $O(m^2)$ ou $O(d^3)$ à des ordres de grandeur plus acceptables. Cela inclut :
      - L'utilisation de **landmarks** (points de repère, $k \ll m$) sélectionnés aléatoirement ou via des algorithmes comme $k$-means, afin de réduire le nombre de similarités à calculer.
      - L'utilisation des **Random Fourier Features (RFF**), qui permettent d'approximer le noyau (produit interne dans l'espace latent) par une projection aléatoire dans un espace de dimension potentiellement très petite ($p$).
2. **Optimisation par Gradient Boosting** : Des algorithmes comme le **Gradient Boosting** (et XGBoost) sont privilégiés car ils sont rapides, efficaces, et facilement parallélisables, malgré leur complexité. Le Gradient Boosting est interprété comme un algorithme de **descente de gradient** dans un espace fonctionnel, où la direction de descente est donnée par les pseudo-résidus (le gradient négatif de la fonction de perte $\ell$). L'objectif est de trouver le pas d'apprentissage ($\alpha_t$) optimal à chaque itération.

L'optimisation et la régularisation constituent donc des concepts théoriques fondamentaux dont la maîtrise est essentielle pour concevoir des solutions scalables et performantes face aux défis du Big Data.



