<!--
Title: "Procédures d'apprentissage - Mesures de performance - Classification (contexte binaire"
Author: rsquaredata
Last updated: 2025-12-03
-->

# Performance et déséquilibre en classification binaire

La **classification binaire** est une tâche fondamentale de l'apprentissage supervisé où l'objectif est d'assigner une donnée à l'une des deux catégories possibles, l'espace des étiquettes $Y$ étant typiquement $\{−1,+1\}$ ou $\{0,1\}$. En foouille de données massives, les scénarios pratiques (tels que la détection de fraude bancaire ou fiscale) se caractérisent souvent par un fort déséquilibre des classes, ce qui rend le choix et l'interprétation des **mesures de performance** particulièrement critique.

## 1. Le contexte binaire et la limitation des mesures simples

### 1.1. Le contexte de la classification

La classification binaire vise à apprendre une hypothèse $h$ (ou un classifieur) qui sépare l'espace en deux. Dans ce contexte, la mesure de l'erreur la plus naturelle est la **0-1 loss**, définie par $\ell (h(x), y) = \mathbb{1}_{(h(x) \neq y )}$, qui compte le nombre d'erreurs commises par le classifieur h. Cependant, cette perte est **non-convexe et non-dérivable**, ce qui rend sa minimisation (pour obtenir le risque empirique $R_S$) un problème NP-difficile. C'est pourquoi des **fonctions de perte substituts** (*surrogate losses*), comme la *Hinge loss* (SVM) ou la *Logistic loss* (Régression Logistique), sont utilisées lors de l'entraînement.

### 1.2. La problématique du déséquilibre

Dans les jeux de données concrets (exemples de transactions frauduleuses ou de fraude fiscale), la classe minoritaire (la fraude) est largement sous-représentée, pouvant n'atteindre que 0.6% ou 0.28% des transactions.

- **L'Accuracy (taux d'exactitude)** : L'Accuracy, ou le complément du taux d'erreur, est la métrique la plus courante. Cependant, chercher à minimiser le taux d'erreur n'est **pas une bonne solution** dans un contexte déséquilibré, car cette mesure favorise la classe majoritaire.

- **Performance trompeuse** : Un classifieur trivial qui prédit systématiquement la classe majoritaire peut obtenir une exactitude très élevée (par exemple, 99%) si les exemples négatifs représentent 99% du jeu de données, mais ce résultat est insatisfaisant car le modèle n'a pas réussi à identifier les exemples d'intérêt (la classe minoritaire).

## 2. Les mesures adaptées au contexte déséquilibré

Pour évaluer correctement la performance dans un contexte binaire déséquilibré, on se base sur les quatre issues possibles de la **matrice de confusion** : Vrais Positifs (TP), Faux Négatifs (FN), Faux Positifs (FP) et Vrais Négatifs (TN). L'objectif devient de **retrouver les exemples de la classe minoritaire** (anomalies, fraudes).

### 2.1. Précision et Rappel

- **Le Rappel (_Recall_ ou Sensibilité)** : Mesure la capacité du modèle à capturer les exemples de la classe positive ou minoritaire. Un rappel élevé est souvent essentiel (par exemple, dans le diagnostic médical pour ne pas manquer un patient).

$$
Rappel = \frac{TP}{TP + FN}.
$$

- **La Précision (_Precision_)** : Évalue la justesse du modèle dans ses prédictions positives.

$$
Précision = \frac{TP}{TP+FP}.
$$

### 2.2. La F-mesure

La **F-mesure** ($R_{\beta}$) st la mesure privilégiée dans les scénarios déséquilibrés, car elle combine la Précision et le Rappel.

- **Définition** : Il s'agit d'une moyenne harmonique des deux mesures précédentes.

$$
F_{\beta} = \frac{(1 + \beta^2) TP}{(1 + \beta^2)TP + \beta^2 FN + FP}.
$$

- **Tuning de $\beta$** : Le paramètre $\beta$ permet de pondérer l'importance du Rappel par rapport à la Précision (souvent $\beta = 1$).

- **Objectif pratique** : En TD, l'objectif est d'établir le modèle permettant d'obtenir les **meilleurs résultats en classification en terme de F-mesure**.

- **Défis** : La F-mesure est **non-linéaire et non-convexe**. Ces caractéristiques la rendent difficile à optimiser directement par des algorithmes de descente de gradient, et il est difficile d'établir des garanties de généralisation pour elle.

### 2.3. L'AUC ROC

L'**AUC ROC** (*Area Under the Receiver Operating Curve*) est une autre mesure populaire pour l'évaluation dans les scénarios déséquilibrés.

- **Rôle** : Elle est particulièrement adaptée aux problématiques de **classement** (*ranking*) lorsque le modèle retourne un score de confiance.

- **Définition** : Elle représente l'aire sous la courbe traçant le Rappel (ou Taux de Vrais Positifs, TPR) en fonction du Taux de Faux Positifs (FPR).

- **Qualité** : Plus la valeur de l'AUC ROC est proche de 1, meilleur est le modèle.

## 3. L'évaluation basée sur les coûts (Cost-Sensitive Learning)

Dans certains problèmes binaires, l'objectif ultime n'est pas seulement de maximiser un score statistique, mais de **minimiser la perte monétaire**.

- **Matrice de coût** : Cette approche modifie la manière dont les erreurs sont prises en compte en associant des **poids/coûts** aux décisions.

- **Importance des erreurs** : L'accent est mis sur les coûts associés aux **Faux Négatifs** ($c_{FN}$) et aux **Faux Positifs** ($c_{FP}$). Mal classer un exemple de la classe minoritaire (FN) doit entraîner une valeur de perte plus importante.

- **Pondération spécifique** : Dans le contexte de la fraude (bancaire ou fiscale), il est recommandé d'attribuer un p**oids plus important aux fraudes d'un montant élevé** afin de minimiser les pertes financières globales. Les implémentations peuvent utiliser le paramètre `class_weight` avec l'option "balanced" pour donner le même poids aux classes.

---

En résumé, la classification binaire, notamment en présence de données massives déséquilibrées, exige de dépasser la simple *Accuracy* pour privilégier des mesures qui mettent l'accent sur la capacité du modèle à identifier correctement la classe minoritaire, telles que la **F-mesure** et l'**AUC ROC**, ou l'intégration de **matrices de coût** pour une évaluation directement liée aux impacts économiques.

