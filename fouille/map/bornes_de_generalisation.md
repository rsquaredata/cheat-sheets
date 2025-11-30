<!--
Title: "Concepts fondamentaux de ML - Risques et généralisation - Bornes de généralisation"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Bornes de généralisation

L'établissement des **bornes de généralisation** est au cœur de la théorie de l'apprentissage statistique, car elles fournissent des garanties théoriques sur la capacité d'un modèle appris sur des données finies ($S$) à prédire correctement sur la distribution réelle et inconnue ($D$).

Ces bornes relient les deux concepts fondamentaux de risque : le **risque empirique** $R_{\ell}^S$ et le **risque réel** $R_{\ell}$.

## 1. Objectif et formalisme des bornes de généralisation

L'objectif de la généralisation est de garantir que l'écart entre le risque réel et le risque empirique soit borné.

### Formulation générale

Une borne de généralisation est une déclaration probabiliste, souvent appelée borne PAC (*Probably Approximately Correct*). Elle s'écrit généralement sous la forme :

$$
\vert R_{\ell} - R_{\ell}^S \le \varepsilon(\delta, m). 
$$

### Signification

Cette formule stipule qu'avec une certaine probabilité $1 - \delta$, l'écart entre le risque réel $R_{\ell}$ et le risque empirique $T_{\ell}^S$ est au plus $\varepsilon(\delta, m)$.

### Dépendances

La fonction $\varepsilon(\delta, m)$ doit être décroissante par rapport au nombre d'exemples $m$. Plus $m$ est grand, plus la borne est serrée, ce qui est intuitif puisque l'échantillon $S$ devrait mieux représenter $D$.

Le but est de construire une fonction $\varepsilon(\cdot)$ qui converge rapidement, souvent avec un taux en $O\left( \frac{1}{\sqrt{m}} \right)$ ou $O \left( \frac{\ln{(m)}}{\sqrt{m}}\right)$.

## 2. Méthodes pour construire les bornes de généralisation

Il existe deux approches principales pour établir ces bornes, en utilisant des outils probabilistes appelés inégalités de concentration (comme l'inégalité de Hoeffding ou celle de **McDiarmid**).

### 2.1. Méthodes basées sur la complexité de l'espace d'hypothèses

Cette approche borne l'écart de généralisation en fonction de la complexité ou de la "taille" de l'ensemble des hypothèses $H$.

#### Uniform Deviation

Cette approche repose sur la convergence uniforme des quantités empiriques vers leurs moyennes, valable pour toute hypothèse $h \in H$. Si l'ensemble d'hypothèses $H$ est de taille finie, on utilise l'inégalité de Hoeffding et l'Union bound pour obtenir une borne qui dépend de $\ln \vert H \vert$ et $m$.

#### Complexité de Rademacher ($R_m(H)$)

Cet outil mesure la capacité d'un ensemble d'hypothèses à s'ajuster au bruit dans l'échantillon de données. Le théorème de la *borne de généralisation de RadeMacher* (*Rademacher Generalization Bound*) établit une borne de généralisation en fonction de la complexité de Rademacher empirique $\hat{R}_S(H)$ :

$$
R(h) \le R_S(h) + 2 \hat{R}_S(h) + 3 \sqrt{\frac{\log(1/\delta)}{2m}}. 
$$

Cette preuve utilise l'Inégalité de McDiarmid.

### Dimension VC

La dimension VC (Vapnik-Chervonenkis) est une autre mesure de la taille de l'espace d'hypothèses, souvent plus facile à calculer pour les classifieurs linéaires.

### 2.2. Méthodes basées sur la stabilité uniforme

Cette approche, plus récente, ne repose pas directement sur une mesure de la complexité de l'espace d'hypothèse. Elle est particulièrement pertinente dans les problèmes d'optimisation convexes régularisé.

#### Concept de stabilité

L'algorithme est jugé stable si son résultat change peu lorsqu'une petite modification est apportée à l'ensemble d'entraînement.

#### Stabilité uniforme ($\beta$)

Un algorithme possède une stabilité uniforme de $\beta \gt 0$ si la substitution d'un seul exemple dans l'échantillon $S$ par un autre exemple $x_i$ n'entraîne qu'une variation maximale $\beta$ de la fonction de perte pour les paramètres appris.

#### Bornes via stabilité (théorème)

En utilisant la convexité de la fonction de perte $\ell$ et l'inégalité de McDiarmid, on obtient une borne de généralisation dont le taux de convergence est en $O(1/\sqrt{m}$).

Le **théorème des bornes via stabilité uniforme** s'énonce ainsi :  Pour tout algorithme avec une constante de stabilité uniforme $\beta$ utilisant une perte $÷ell$ bornée par $K$, nous avons, avec probabilité au moins $1 - \delta$ :

$$
R_{\ell}(\theta_S) \le R_{\ell}^S(\theta_S) + 2 \beta + (4 m \beta + K) \sqrt{\frac{\ln(1/\delta)}{2m}}. 
$$

Cette borne est considérée comme plus informative en pratique et est cohérente avec l'usage de la régularisation en ML.

## 3. Lien des bornes avec la régularisation ($\lambda$)

Les bornes de généralisation sont intimement liées au problème de minimisation du risque empirique régularisé (RRM).
- **Rôle de $\lambda$** : Le paramètre de régularisation $\lambda$ contrôle le compromis entre la minimisation du risque empirique et la complexité du modèle.
- **Impact sur la borne** : La constante de stabilité uniforme $\beta$ (qui apparaît dans la borne) est elle-même une f**onction décroissante de l'hyperparamètre $\lambda$**.
- **Interprétation** : L'expression générale de la borne est souvent de la forme :

$$
\vert R_{\ell}(h) - R_{\ell}^S(h) \le O\left(m^{-1/2}, \lambda^{-1}\right).
$$

Cela signifie que si le paramètre de régularisation $\lambda$ augmente, la complexité du modèle diminue (régularisation plus forte), ce qui se traduit par une **borne plus serrée** et donc une meilleure garantie de généralisation. Inversement, si $\lambda \to 0$, la borne peut devenir très lâche, reflétant un risque accru de surapprentissage.

## 4. Le Cycle risque-généralisation-borne

L'ensemble de ces concepts forme un cycle fondamental de la théorie de l'apprentissage :

1. **Risque Empirique ($R_{\ell}^S$)** : L'algorithme minimise cette quantité observée sur les données d'entraînement.
2. **Régularisation ($\lambda$)** : Une contrainte est ajoutée pour limiter la complexité du modèle et éviter le surapprentissage.
3. **Généralisation** : La théorie garantit que l'algorithme qui minimise le risque régularisé aura de bonnes performances sur le **risque réel** ($R_{\ell}$).
4. **Borne** : Les bornes de généralisation (via stabilité ou somplexité) quantifient cette garantie, donnant un intervalle de confiance sur l'écart entre $R_{\ell}$ et $R_{\ell}^S$.
