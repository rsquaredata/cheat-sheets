<!--
Title: "Concepts fondamentaux de ML - Risques et généralisation - Bornes de généralisation - Inégalité de McDiarmid"
Author: rsquaredata
Last updated: 2025-11-30
-->

## Inégalité de McDiarmmid - Outil de concentration pour la stabilité uniforme

L'**inégalité de McDiarmid** est un outil probabiliste qui sert de fondement mathématique à la construction des bornes de généralisation, notamment celles basées sur le cadre de la stabilité uniforme..

Dans le contexte des risques et de la généralisation, ces bornes visent à garantir que le modèle appris sur l'échantillon d'entraînement ($S$) sera performant sur la distribution réelle et inconnue ($D$).

## 1. Définition et rôle de l'inégalité de McDiarmid

L'inégalité de McDiarmid est une i**négalité de concentration**. Elle permet de borner la déviation d'une fonction mesurable $F$ (qui dépend de variables aléatoires indépendantes) autour de son espérance.

### Définition formelle (théorème)

Soient deux ensembles $S$ et $S^i$, et $F:(X \times Y)^m \to \mathbb{R}$ une fonction mesurable pour laquelle il existe des constantes $c_i$ telles que $\vert F(S)-F(S^i)\vert \le c_i$. Alors l'inégalité de McDiarmid stipule que :

$$
\mathbb{P} [(F(S) - \mathbb{E}_S [ F(S)] \ge \varepsilon] \le \exp \left( \frac{2 \varepsilon^2}{\sum_{i=1}^m c_i^2}\right).
$$

### Application à la généralisation

Dans le domaine de l'apprentissage statistique, l'inégalité de McDiarmid est utilisée pour majorer l'écart entre le risque rél $R_{\ell}$ et le risque empirique $R_{\ell}^S$. Elle est l'outil central utilisé pour obtenir des bornes de généralisation, au même titre que l'inégalité de Hoeffding.

En particulier, l'inégalité de McDiarmid est utilisée pour établir :
1. Les **bornes via stabilité uniforme**.
2. La **borne de généralisation de Rademacher**, pour laquelle la preuve consiste à appliquer l'inégalité de McDiarmid à la fonction $\phi(S) = \sup_{h \in H}(R(h)-R_S(h))$.

## 2. L'inégalité de McDiarmid dans le cadre de la stabilité Uniforme

L'application de l'inégalité de McDiarmid requiert de borner la variation de la fonction d'intérêt (la différence entre les risques) lorsque l'on substitue un seul point dans l'échantillon $S$.

### Hypothèse de stabilité

La notion de **stabilité uniforme** d'un algorithme $A$ (définie par une constante $\beta$) garantit que la modification d'un seul exemple dans l'échantillon $S$ (pour former $S^i$) n'entraîne qu'une faible modification de la *loss*. C'est cette propriété de stabilité qui permet de déterminer les constantes $c_i$ nécessaires pour appliquer l'inégalité de McDiarmid.

### Démonstration du théorème des bornes via stabilité uniforme

La démonstration du théorème des bornes via stabilité uniforme utilise le fait que la variable aléatoire $R_{\ell}(\theta_S) - R_{\ell}^S(\theta_S)$ satisfait les conditions du théorème de l'inégalité de McDiarmid.

En utilisant l'hypothèse de stabilité, on montre que, pour une variable aléatoire $F(S) = R(\theta_S) - R_S(\theta_S)$, l'écart entre $F(S)$ et $F(S^i)$ est borné par des constantes $c_i$ qui sont liées à la constante de stabilité $\beta$ et à la borne $K$ de la fonction de perte $\ell$.

L'étape finale de la preuve consiste à appliquer l'inégalité de McDiarmid en utilisant le fait que $\mathbb{E}_S [R(\theta_S) - R_S(\theta_S)] \le 2 \beta$ (où $\beta $ est la constante de stabilité uniforme) pour déduire l'expression finale de la borne.

## 3. Conclusion sur les bornes

L'inégalité de McDiarmid est donc un **outil probabiliste non négociable** pour prouver rigoureusement les bornes de généralisation. Ces bornes confirment que le risque réel $R_{\ell(\theta_S)}$ du modèle appris est borné par le risque empirique $R_{\ell^S(\theta_S)}$ plus un terme d'erreur $\varepsilon(\delta, m)$ qui dépend inversement de la taille de l'échantillon $m$ et, dans le cas de la stabilité, du terme de régularisation $\lambda$ (via la constante $\beta$).

L'existence et l'application de cette inégalité permettent de passer d'une observation empirique (la performance sur $S$) à une garantie théorique forte sur la performance future (sur $D$).

Pour illustrer l'importance de cet outil, on pourrait considérer l'Inégalité de McDiarmid comme un **thermomètre théorique** pour un modèle de ML. Le thermomètre (l'inégalité) ne mesure pas directement la chaleur (le risque réel), mais il garantit que si la composition interne du fluide (la stabilité $\beta$) est connue, et si la température affichée (le risque empirique) est basse, alors la vraie température est presque certainement proche de cette valeur affichée.

