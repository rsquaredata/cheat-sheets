<!--
Title: "Concepts fondamentaux de ML - Risques et généralisation - Bornes de généralisation - VC-dimension / Complexité de Rademacher"
Author: rsquaredata
Last updated: 2025-11-30
-->

# VC-dimension / Complexité de Rademacher

La **dimension VC** (Vapnik-Chervonenkis dimension) et la **complexité de Rademacher** sont des outils théoriques fondamentaux utilisés dans la construction des bornes de généralisation. Elles servent à mesurer la complexité ou la "taille" de l'ensemble des hypothèses ($H$) qu'un algorithme peut générer, afin de quantifier l'écart entre le risque réel $R_{\ell}$ et le risque empirique $R_{\ell}^S$.

## 1. Contexte des bornes de généralisation

L'objectif général de la théorie de l'apprentissage statistique est d'établir une borne probabiliste (une borne PAC, P*robably Approximately Correct*) qui garantit que l'écart entre le risque réel et le risque empirique est faible. Cette borne prend la forme générale $\vert R_{\ell} - R_{\ell}^S \vert \le \varepsilon(\delta,m)$, où $\varepsilon$ est une fonction décroissante du nombre d'exemples $m$.

Lorsque l'ensemble des hypothèses $H$ est fini, cette borne peut être obtenue relativement facilement en utilisant l'inégalité de Hoeffding et l'Union Bound. Cependant, pour de nombreux classifieurs pratiques (comme les séparateurs linéaires ou les SVM, dont les paramètres $w$ appartiennent à $\mathbb{R}^{d+1}$), l'espace d'hypothèses $H$ est infini. Dans ce cas, il est nécessaire d'utiliser des mesures de complexité structurelle pour borner l'écart de généralisation.

## 2. La dimension VC (Vapnik-Chervonenkis)

La Dimension VC est une mesure de la taille de l'espace d'hypothèses $H$. Cependant, la dimension VC est souvent difficile à estimer pour la plupart des problèmes, à l'exception des classifieurs linéaires.

## 3. La complexité de Rademacher

La complexité de Rademacher est une alternative à la Dimension VC pour mesurer la complexité d'un ensemble d'hypothèses $H$.

### Définition et fonctionnement

Informellement, la complexité de Rademacher mesure la capacité de l'ensemble des hypothèses $H$ à **s'ajuster au bruit** dans l'échantillon de données.

On dstingue deux formes de complexité de Rademacher :
1. **Complexité de Rademacher empirique ($R_S(H)$)** : Définie pour un échantillon fixé $S = \{ x_i\}_{i=1}^m$ :

$$
R_S(H) = E_{\sigma} \left[ \sup_{h \in H} \frac{1}{m} \sum_{i=1}^m \sigma_i h(x_i) \right],
$$

où $\sigma = (\\sigma_1, \ldots, \sigma_m)$ est un vecteur de **variables aléatoires de Rademacher** prenant les valeurs $\{-1, +1\}$ avec une probabilité 1/2.

2. **Complexité de Rademacher ($R_m(H)$)** : C'est l'espérance de la complexité empirique sur la distribution $D$ des données. La valeur de cette mesure augmente avec la taille de $H$ et diminue avec la taille de l'échantillon $m$.

### Bornes de généralisation basées sur Rademacher

La complexité de Rademacher est utilisée pour borner l'écart de généralisation (l'excès de risque). Elle permet d'établir le **théorème de la borne de généralisation de Rademacher**, qui stipule que, pour une fonction de perte ℓ bornée par , le risque réel $R(h)$ est borné :

$$
R(h) \le R_S(h) + 2 R_m(H) + \sqrt{\frac{\log(1/\delta)}{2m}}
$$
$$
R(h) \le R_S(h) + 2 R_S(H) + 3 \sqrt{\frac{\log(1/\delta)}{2m}}
$$

L'obtention de cette borne utilise l'**inégalité de McDiarmid**. La relation entre l'excès de risque (la quantité que l'on cherche à borner) et la complexité de Rademacher est illustrée par l'inégalité :

$$
E_{S \sim D^m} \left[ R(h) - R_{*} \right] \le \inf{g \in H} R(g) - R_{$} + 4R_m(H),
$$

qui rend explicite le lien entre la complexité et la taille de l'échantillon.

## 4. Comparaison avec la stabilité uniforme

Bien que la dimension VC et la complexité de Rademacher soient des outils valides pour établir des bornes de généralisation, elles sont souvent difficiles à calculer en pratique.

C'est pourquoi une autre approche, celle de la stabilité uniforme, est privilégiée dans le cadre de l'étude des problèmes d'optimisation convexes régularisés.</br>
La stabilité uniforme permet d'établir des bornes sans recourir directement à une mesure de complexité de l'espace d'hypothèse $H$. L'approche par stabilité est considérée comme plus informative en pratique, car elle prend explicitement en compte le terme de régularisation $\lambda$, qui est essentiel dans les problèmes de minimisation du risque régularisé.
