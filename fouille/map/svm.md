<!--
Title: "Algorithmes clés - Support vector Machine (SVM)"
Author: rsquaredata
Last updated: 2025-12-04
-->

## SVM : De la marge maximale aux noyaux

Les **Séparateurs à Vaste Marge (Support Vector Machine - SVM)** sont l'un des algorithmes les plus répandus et les plus connus en apprentissage machine, particulièrement pour les tâches de c**lassification binaire**.

## 1. Fondements et principe du SVM linéaire

Le principe fondamental du SVM, initialement proposé par Boser, Guyon et Vapnik en 1992, repose sur l'idée de trouver l'hyperplan séparateur qui possède la plus grande **marge** entre les classes.

- **Règle de dlassification** : Un SVM linéaire apprend une hypothèse $h$ qui se présente sous la forme d'un hyperplan affine. La classification binaire d'un exemple x se fait selon la règle :

$$
h(x)=sign[ \langle w,x \rangle +b] = 
\begin{cases}
-1 \text{ si } \langle w,x \rangle + b \lt 0 \\
+1 \text{ si } \langle w,x \rangle + b \gt 0
\end{cases}.
$$

- **Maximisation de la marge** : L'objectif est de choisir le séparateur qui se trouve à la plus grande distance entre les deux classes. Cette distance, appelée marge $\gamma$, est définie par la distance entre les hyperplans d'équation $\langle w,x \rangle +b = \pm 1$. La maximisation de la marge $\gamma$ est équivalente à la **minimisation de la quantité** $\frac{1}{2} \Vert w \Vert_2^2$. Choisir la plus grande marge garantit une meilleure capacité à **généraliser** sur de nouvelles données.

### Formulation *Hard Margin SVM*

Dans le cas idéal où les données sont **linéairement séparables** (ce qui est rare en pratique), le problème d'optimisation (**problème primal**) est défini comme suit :

$$
\min_{(w,b) \in \mathbb{R}^{d+1}} \frac{1}{2} \lVert w \rVert_2^2 \text{ s.t. } y_i \left( \langle w, x_i \rangle \right) \ge 1 \text{, } \forall \text{ } i = 1, \ldots, m.
$$

## 2. Le Soft Margin SVM et la gestion des erreurs

Puisque les données réelles sont rarement linéairement séparables, il est nécessaire de **relâcher (relaxer) le problème** pour autoriser des erreurs de classification.

- **Variables Slacks ($\xi_i$)** : Ces erreurs sont capturées par des variables *slacks* ($\xi_i \ge 0$), qui sont introduites dans le problème d'optimisation. Ces variables sont positives si les contraintes de marge ne sont pas respectées (l'exemple est mal classé ou à l'intérieur de la marge) et nulles sinon.

- **Problème _Soft Margin SVM_ (formulation primale) : L'objectif devient de trouver un compromis entre la maximisation de la marge et la minimisation du coût de violation des contraintes :

$$
\min{\xi \in \mathbb{R}^m \text{, } (w,b) \in \mathbb{R}^{d+1}} \frac{1}{2} \Vert w \Vert_2^2 + \frac{C}{m} \sum_{i=1}^m \xi_i \text{ s.t. } y_i \left( \langle w, x_i \rangle + b \right) \ge 1 - \xi_i, \quad  \xi_i \ge 0.
$$

- **Hyperparamètre $C$** : Le paramètre $C$ est l'**hyperparamètre de régularisation** qui contrôle l'équilibre entre la complexité du modèle ($\Vert w \Vert _2^2$) et le coût des erreurs ($\sum \xi_i$). Il doit être **tuné** lors du processus d'apprentissage.

- **Formulation équivalente (_Hinge Loss_)** : En intégrant directement les variables *slacks* dans la fonction objectif, le problème *Soft Margin SVM* se réécrit en minimisant la **Hinge Loss** :

$$
\min{(w,b) \in \mathbb{R}^{d+1}} \frac{1}{2} \Vert w \Vert _2^2 + \frac{C}{m} \sum_{i=1}^m \left[ 1 - y_i ( \langle w, x_i \rangle + b\right]^+ .
$$

 
## 3. Les méthodes à noyaux (*Kernel SVM*) et le *Dual Problem*

Les SVM linéaires sont limités aux problèmes séparables par un hyperplan. Pour gérer la **non-linéarité**, on utilise l'**astuce du noyau** (*kernel trick*).

### 3.1. Le problème dual

Pour introduire les noyaux, on utilise la formulation duale du problème *Soft Margin SVM*. Le problème dual se formule en fonction du vecteur des variables lagrangiennes $\alpha$.

- La résolution du problème dual est souvent préférée pour améliorer la vitesse de convergence de l'algorithme d'optimisation, car elle trouve une solution dans l'espace des individus plutôt que dans l'espace des paramètres.

- De plus, la solution du problème dual est la même que celle du problème primal (*dualité forte*).

### 3.2. L'astuce du noyau

L'astuce du noyau repose sur le **théorème de Mercer** : au lieu d'utiliser le produit scalaire standard $\langle x, x_i \rangle$ dans le problème dual, on le remplace par une fonction noyau $K(x_i, x_j)$.

- **Projection implicite** : Ce noyau définit un produit scalaire dans un **espace latent $Z$ de dimension supérieure** (potentiellement infinie), via une transformation implicite $\phi$. Dans cet espace, le problème devient **linéairement séparable**.

- **Prédiction** : Le classifieur *kernelisé* utilise l'ensemble des points d'entraînement pour la prédiction :

$$
h(x') = sign \left( \sum_{i=1}m \alpha_i y_i K(x', x_i) \right).
$$

- **Exemples de noyaux** :
  - **noyau linéaire** $K(x,x') = \langle x, x' \rangle$.
  - **noyau polynomial** $K(x, x') = \left( \langle x, x' \rangle + c \right)^p$.
  - **noyau gaussien (RBF)** $K(x,x') = \exp \left( - \frac{\Vert x-x' \Vert _2^2}{ \sigma^2} \right)$.

- **Hyperparamètre $\gamma$** : Le noyau gaussien introduit l'hyperparamètre $\gamma$ (souvent défini par $\gamma = \frac{1}{\sigma^2}$ en Python). Ce paramètre contrôle l'importance de la similarité entre les exemples, influençant le lissage de la frontière de décision. Pour les SVM à noyau gaussien, le tuning de $C$ et $\gamma$ est indispensable.

## 4. Les vecteurs supports (*Support Vectors*)

Le nom *Support Vector Machine* provient des exemples qui participent réellement à la construction de la frontière de décision.

- **Rôle** : Dans la formulation duale, seuls les exemples pour lesquels les coefficients duaux $\alpha_i$ sont non nuls contribuent à la définition de l'hyperplan séparateur.

- **Définition** : Les vecteurs supports sont les points qui se trouvent s**ur la marge, ou du mauvais côté de l'hyperplan**. Les points correctement classés et en dehors de la marge ont $\alpha_i = 0$ et ne contribuent pas à la définition de l'hyperplan.

## 5. Limites et scalabilité dans un contexte de données massives

Le SVM, en particulier dans sa version *kernelisée*, présente des limitations importantes pour la **fouille de données massives**.

- **Complexité de calcul** : Les méthodes à noyaux nécessitent le calcul de toutes les similarités deux à deux $K(x_i, x_j)$ entre les $mm exemples, soit $m^2$ similarités. De plus, le modèle appris comporte $m$ paramètres $\alpha_i$.
​
- **Inversion de matrice** : La résolution de la plupart des modèles à noyaux nécessite l'inversion d'une matrice de Gram ($K \in \mathbb{R}^{m \times m}$), ce qui n'est **pas réalisable dans un contexte de _Big Data_**.

- **Approximation nécessaire** : En raison de ces contraintes, il est **nécessaire de trouver des approches d'approximation** pour rendre les méthodes à noyaux utilisables sur de grands ensembles de données. Ces méthodes incluent l'utilisation de **Landmarks** (points de repère) ou **de Random Fourier Features (RFF)**.

## 6. Le One-Class SVM (non supervisé)

Le SVM possède également une version non supervisée, le **One-Class SVM**, utilisée pour la **détection d'anomalies**.

- **Objectif** : Il s'agit de séparer les individus en deux classes : les exemples "normaux" et les exemples "aberrants".

- **Principe** : Le problème se formule comme la recherche de la plus petite sphère (ou hyperplan) contenant la majorité des points "normaux", tout en autorisant des erreurs via des variables *slacks* $\xi_i$ pour les anomalies. Le paramètre $\mu$ dans cette formulation contrôle une borne supérieure sur le pourcentage d'anomalies autorisées.
