<!--
Title: "Concepts fondamentaux de ML - Risques et généralisation - Risque empirique"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Risque empirique

Le **risque empirique** $R_{\ell}^S(h)$ représente le critère que les algorithmes de ML cherchent activement à minimiser lors de la phase d'entraînement.Dans le contexte plus large des risques et de la rénéralisation, le risque empirique est un pilier de la théorie de l'apprentissage statistique.

## 1. Définition et calcul du risque empirique

Le risque empirique est l'e**rreur moyenne** d'une hypothèse $h$ (la fonction ou le classifieur appris) sur l'échantillon d'entraînement $S$. Il est défini formellement par :

$$
R_{\ell}^{S}(h) = \mathbb{E}_{(x,y) \in S} \left( \ell(h(x),y \right) = \sum _{i=1}^m \ell \left( \ell(h(x),y \right),
$$

avec :
- $S = \{(x_i, y_i)\}_{i=1}^m$ : L'ensemble des $m$ exemples d'entraînement tirés de manière i.i.d. (identiquement et indépendamment distribués) à partir d'une distribution inconnue $D$.
- La fonction $\ell$ est la **fonction de perte** (*loss function*) qui pénalise les erreurs commises par l'hypothèse $h$.

Le risque empirique représente donc la **valeur moyenne de la perte** sur l'ensemble des données observées.

## 2. Le principe de minimisation du risque empirique (ERM)

En pratique, comme la distribution conjointe des données $D$ est inconnue, le risque réel $R_{\ell}^S(h)$, qui est l'erreur attendue sur la distribution $D$, ne peut pas être calculé.

Par conséquent, les algorithmes d'apprentissage machine se concentrent sur la **minimisation du risque empirique** :

$$
h_S^* = \arg \min_{h \in H} R_{\ell^S(h)}.
$$

En minimisant cette quantité, on espère que l'échantillon $S$ est suffisamment représentatif de la distribution inconnue $D$. Ce concept est lié à **la loi des grands nombres** : intuitivement, plus l'ensemble d'entraînement $m$ est grand, plus le risque empirique sera proche du risque réel.

##  Le risque empirique régularisé (RRM)

Le problème de la généralisation survient lorsque la minimisation du risque empirique est insuffisante et mène au **surapprentissage** (*over-fitting*). Le surapprentissage signifie que l'hypothèse $h$ s'ajuste parfaitement aux données d'entraînement (faible $R_{\ell}^S$) mais possède un risque réel $R_{\ell}$ élevé sur des données nouvelles.

Pour garantir une bonne capacité de généralisation et trouver un **bon compromis** entre l'ajustement aux données et la complexité du modèle, le risque empirique est souvent régularisé dans la fonction objectif (RRM) :

$$
\min_{w in \mathbb{R}^d} \frac{1}{m} \sum_{i=1}^m \ell(w, x_i) + \lambda N(w).
$$

Dans cette formulation, $R_{\ell}^S$ correspond au terme $\frac{1}{m} \sum_{i=1}^m \ell(w, x_i)$, et il est complété par le terme de régularisation $\lambda N(w)$ qui contraint la complexité du modèle $w$.

## 4. Le risque empirique dans les bornes de généralisation

Le risque empirique est une composante essentielle dans l'établissement des bornes de généralisation, notamment celles basées sur la **stabilité uniforme**. Ces bornes visent à majorer l'écart entre le risque réel et le risque empirique :

$$
\vert R_{\ell} - R_{\ell}^S \vert \le \varepsilon(\delta, m).
$$

Le théorème de Bousquet-Elisseeff (borne via stabilité uniforme) donne, avec probabilité au moins $1- \delta$, une borne supérieure pour le risque réel en fonction du risque empirique :

$$
R_{\ell}(\theta_S) \le R_{\ell}S(\theta_S) + 2 \beta + (4m\beta + K) \sqrt{\frac{\ln(1/\delta)}{2m}}.
$$

Dans cette formulation, $R_{\ell}S(\theta_S)$ est l'estimation de l'erreur sur les données d'entraînement pour le modèle $\theta_S$. Si $R_{\ell}S(\theta_S)$ est très faible (ce qui est souvent le cas pour les modèles complexes), il faut s'assurer que le terme de droite de l'inégalité (la borne) reste petit pour garantir la généralisation.

## 5. Choix de la fonction de perte empirique ($\ell$)

Le choix de la fonction de perte $\ell$ est fondamental car il définit ce que l'on minimise en pratique ($R_{\ell}S$).

- **Classification** : La perte naturelle est la 0-1 loss ($\ell(h(x),y) = \mathbb{1}_{h(x)} \neq y$), qui compte le nombre d'erreurs. Cependant, elle est non convexe et non différentiable, ce qui la rend **NP-difficile** à minimiser directement.
- **Pertes substituts (Surrogate Losses)** : Pour résoudre le problème en pratique, des fonctions de perte convexes et souvent différentiables sont utilisées comme substituts, ce qui définit la nature du risque empirique minimisé.
  - **Hinge Loss** : Utilisée pour les SVM (Séparateurs à Vaste Marge).
  - **Exponential Loss** : Utilisée dans le contexte du Boosting (Adaboost).
  - **Logistic Loss** : Utilisée pour la régression logistique.

Dans les contextes de données déséquilibrées, où minimiser le taux d'erreur (et par conséquent le risque empirique basé sur la 0−1 loss ou ses substituts non pondérés) peut amener le modèle à ignorer la classe minoritaire, il est crucial de considérer des stratégies de pondération des erreurs ou l'utilisation de l'**apprentissage sensible aux coûts** (*Cost-Sensitive Learning*).

Le risque empirique est donc la mesure que l'algorithme "voit" et cherche à minimiser, mais la **théorie de la généralisation** s'assure que cette minimisation corresponde bien à une performance optimale sur les données réelles (risque réel).
