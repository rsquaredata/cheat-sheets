<!--
Title: "Procédures d'apprentissage - Mesures de performance - Régression (MSE, RMSE, MAE)"
Author: rsquaredata
Last updated: 2025-11-30
-->

## Évaluation de  la régression : MSE, MAE et gestion des erreurs

La **régression** est l'une des tâches fondamentales de l'apprentissage supervisé, aux côtés de la classification. Dans le contexte plus large de la **procédure d'apprentissage** et des **mesures de performance**, la régression est associée à des métriques spécifiques qui quantifient l'erreur entre la valeur prédite et la valeur réelle.

Les mesures de performance couramment utilisées pour les tâches de régression incluent l'**erreur quadratique moyenne (MSE)**, l'**erreur quadratique moyenne (RMSE)** et l'**erreur absolue moyenne (MAE)**.

## 1. Contexte et définition des tâches de régression

Dans une tâche de régression, l'espace de sortie $Y$ est un sous-ensemble de $\mathbb{R}. L'objectif est de prédire une valeur réelle, comme l'estimation du temps de survie d'un greffon après transplantation, ou la prédiction du score obtenu à un test selon les scores obtenus à d'autres tests.

Pour trouver la meilleure hypothèse $h$ (ou le meilleur vecteur de paramètres $\theta$) qui explique la variable réponse $y$ à partir des caractéristiques $X$, il faut **minimiser une fonction de perte** (*loss function*). Dans le contexte de la régression, cette perte est souvent basée sur la différence entre la valeur estimée $\hat{y}=h(x)$ et la valeur réelle $y$.

## 2. Mesures de performance spécifiques à la régression

Il existe trois mesures principales utilisées pour évaluer la performance d'un modèle de régression

### 2.1.  L'zrreur quadratique moyenne (MSE)

L'**erreur quadratique moyenne (Mean Square Error - MSE)** est la mesure de performance la plus utilisée en régression.

- **Définition** : Elle est calculée comme la valeur moyenne, sur tous les exemples, de la **différence au carré** entre la valeur prédite ($\hat{y}_i$) et la valeur réelle  ($y_i$) :

$$
MSE = \frac{1}{m} \sum_{i=1}^m (y_i - \hat{y}_i)^2.
$$

- **Propriétés** : Hormis la constante $1/m$, la MSE peut être vue comme la norme $\ell_2$ du vecteur des différences $(y - \hat{y})$.
- **Avantages** : La MSE est une mesure qui présente l'avantage d'être convexe et **dérivable** (*differentiable*), ce qui permet son optimisation directe par des algorithmes d'optimisation (comme la descente de gradient).
- **Utilisation comme _loss_** : Minimiser le problème de régression linéaire revient souvent à **minimiser la norme $\ell_2$ des erreurs $(\min_{\theta \in \mathbb{R}} \Vert y - X \theta \Vert_2^2)$, ce qui est équivalent à minimiser la MSE.
- **Inconvénient** : La MSE est **très sensible aux _outliers_** (valeurs aberrantes). Si une donnée s'écarte fortement de la ligne de régression, sa valeur au carré devient très rapidement importante. Cela peut induire un biais important dans le modèle qui cherchera à se rapprocher de cet *outlier*.

### 2.2 L'erreur quadratique moyenne (RMSE)

L'**erreur quadratique moyenne (_Root Mean Square Error_ - RMSE)** est une variante souvent considérée lorsque les déviations au carré deviennent très importantes (pour des raisons numériques).

- **Définition** : C'est la racine carrée de la MSE :

$$
RMSE_h = \sqrt{\frac{1}{m} \sum_{i=1}^m (y_i - \hat{y}_i)^2 }.
$$

- **Usage** : Elle est souvent considérée comme la **mesure de performance la plus utilisée** dans les tâches de régression.

### 2.3. L'erreur absolue moyenne (MAE)

L'**erreur absolue moyenne (_Mean Absolute Error_ - MAE)** est souvent utilisée comme alternative pour contourner le problème de sensibilité aux *outliers* inhérent à la MSE/RMSE.

- **Définition** : Elle est définie comme la valeur moyenne de la **différence absolue** entre la valeur prédite et la valeur réelle :

$$
MAE_h = \frac{1}{m} \sum_{i=1}^m \vert y_i - \hat{y}_i \vert.
$$

- **Propriétés** : Elle est vue comme la **norme $\ell_1$ du vecteur des différences $(y - \hat{y}$.

- **Avantage** : Contrairement à la MSE, la MAE est moins sensible aux *outliers*.

## 3. Autres critères d'évaluation en régression

En plus de ces mesures basées sur l'erreur (MSE, RMSE, MAE), d'autres mesures de performance peuvent être rencontrées pour évaluer la qualité d'un modèle de régression, telles que le critère AIC, le critère BIC, ou la valeur du $R^2$ (*R squared value*).

## 4. Régression dans la procédure d'apprentissage

La régression fait partie des tâches que les algorithmes d'apprentissage machine peuvent réaliser.

Dans le cadre d'une mise en situation pratique, comme l'estimation de la durée de vie d'un greffon (problème de régression), il est attendu de proposer une procédure complète incluant le choix des algorithmes et les **mesures de performance**. Dans ce cas, les mesures comme la MSE, la RMSE ou la MAE seraient essentielles pour le *tuning* et l'évaluation finale.

---

En résumé, si la classification est évaluée par l'Accuracy, le Rappel, la Précision et surtout la F-mesure et l'AUC ROC dans les cas déséquilibrés, la régression utilise principalement la **MSE**, la **RMSE** et la **MAE** comme critères pour juger de la qualité du modèle et pour définir la fonction de perte à optimiser.

---

**Analogie** : Si l'évaluation des performances en classification (F-mesure, AUC) est comme juger la qualité d'un filet de pêche (sa capacité à capturer la proie rare sans laisser passer les bonnes prises), l'évaluation en régression (MSE, MAE) est comme évaluer la précision d'une cible de tir à l'arc : la **MAE** mesure la distance moyenne de chaque flèche au centre de la cible (précision absolue), tandis que la **MSE** (et RMSE) amplifie l'impact des flèches qui sont très éloignées (les *outliers*), favorisant ainsi un modèle qui évite les erreurs catastrophiques.





