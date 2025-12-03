<!-- 
Title: "Apprentissage massif et données déséquilibrées - Procédures d'apprentissage - Mesures de performance" 
Author: rsquaredata
Last updated: 2025-12-03
-->

# Mesures de performance face au déséquilibre des données

L'évaluation des **mesures de performance** est une étape essentielle de la **procédure d'apprentissage** en fouille de 
données massives. Dans ce contexte, le choix d'une mesure appropriée est crucial, car les jeux de données sont souvent 
caractérisés par un **fort déséquilibre** des classes, rendant les métriques traditionnelles peu fiables.

L'objectif de cette phase est de comparer les résultats obtenus par les différents algorithmes (performances et temps 
d'apprentissage) pour valider le protocole expérimental.

## 1. Le contexte de la classification binaire déséquilibrée

Dans les applications courantes (telles que la détection de fraude bancaire ou fiscale), le jeu de données est 
généralement déséquilibré. Par exemple, seulement 0.6% ou 0.28% des transactions sont frauduleuses.

Dans ces scénarios, l'objectif est de retrouver les **exemples de la classe minoritaire** (les fraudes, les anomalies) 
qui représentent une menace ou une perte.

## 2. Les limites des mesures de performance standards

La plupart des algorithmes sont conçus pour minimiser leur taux d'erreur (*Error rate*) sur l'ensemble d'apprentissage, 
ou ses substituts convexes (fonctions de *loss*).

- **Taux d'erreur (_Accuracy_)** : Chercher à minimiser le taux d'erreur n'est **pas une bonne solution** dans un 
contexte déséquilibré, car cette mesure a tendance à **favoriser la classe majoritaire**.

- **Performance trompeuse** : Un classifieur trivial qui prédit systématiquement la classe majoritaire peut obtenir un 
score d'exactitude très élevé (par exemple, 99% si 99% des données sont non frauduleuses), ce qui est insatisfaisant 
car il n'a pas réussi à identifier les exemples de la classe d'intérêt.

- **Comportement des algorithmes** : Face au déséquilibre, des modèles comme le SVM linéaire ont tendance à prédire 
toutes les données comme appartenant à la classe majoritaire, ce qui biaise les performances.

## 3. Les mesures adaptées au déséquilibre (basées sur la matrice de confusion)

Pour évaluer la capacité du modèle à gérer la classe minoritaire, on utilise des métriques dérivées de la 
**matrice de confusion** :

| Prédiction | $h(x)$ =Positif | $h(x) = Négatif |
| **Réel** $y$=Positif (*Minoritaire**) | Vrai Positif (TP) | Faux Négatif (FN) |
| **Réel** $y$=Négatif (**Majoritaire**) | Faux Positif (FP) | Vrai Négatif (TN) |

### 3.1. Précision et Rappel

1. **Le Rappel (_Recall_ ou Sensibilité)** : Mesure la capacité du modèle à retrouver les exemples de la classe 
positive ou minoritaire. Il est défini par $\frac{TP}{TP+FN}$. Un rappel élevé est souvent essentiel, par exemple en 
diagnostic médical, pour ne pas manquer un patient.

2. **La Précision (_Precision_)** : Évalue la justesse du modèle dans ses prédictions positives, soit $\frac{TP}{TP+FP}.

### 3.2. La F-mesure

La **F-mesure** ($F-{\beta}$) est la mesure privilégiée pour synthétiser la performance dans les contextes 
déséquilibrés, notamment en détection de fraude et d'anomalie.

- **Définition** : C'est une moyenne harmonique de la Précision et du Rappel.

$$
F_{\beta} = ÷frac{(1 + \beta^2)TP}{(1 + \beta^2) TP + \beta^2 FN + FP}.
$$

- **Tuning de $\beta$ : Le paramètre $\beta$ permet de pondérer l'importance du Rappel par rapport à la Précision. 
Si $\beta=1$, les deux sont jugés avec la même importance.

- **Objectif pratique** : Il s'agit chercher à établir le modèle qui permet d'obtenir les **meilleurs résultats en 
classification en terme de F-mesure**.

- **Défis** : La F-mesure est non-linéaire et non convexe, ce qui la rend **difficile à optimiser* directement par des 
algorithmes de descente de gradient standard.

## 4. Mesures de performance basées sur le classement

L'évaluation peut également se concentrer sur la qualité du classement des exemples, ce qui est pertinent lorsque le 
modèle renvoie un score ou une confiance.

- **AUC ROC (_Area Under the Receiver Operating Curve_)** : C'est une métrique populaire, adaptée aux problématiques 
de classement (*ranking*).

- **Calcul** : La courbe ROC trace le Rappel (*True Positive Rate*, TPR) en fonction du Taux de Faux Positifs (*False 
Positive Rate*, FPR).

- **Interprétation** : Plus la valeur de l'AUC ROC est proche de 1, meilleur est le modèle. Les mesures comme l'AUC 
ROC ou la F-mesure sont souvent requises pour évaluer les modèles, notamment en Deep Learning.

## 5. Critères économiques et de rapidité

Dans la procédure d'apprentissage, l'évaluation va au-delà des métriques statistiques pures pour intégrer des 
contraintes pratiques du *Big Data*.

### 5.1. Apprentissage sensible aux coûts (*Cost-Sensitive Learning*)

L'objectif réel de certaines tâches, comme la détection de fraude fiscale, est de **minimiser la perte monétaire** 
due à la fraude.

- **Matrice de coût** : Au lieu de seulement compter les erreurs, on associe des **poids/coûts** aux décisions. Il 
faut se concentrer sur la valeur du coût des faux négatifs ($c_{FN}) — une fraude non détectée — et des faux positifs 
($c_{FP}).

- **Minimisation des pertes** : La procédure peut être ajustée pour attribuer un poids plus important aux fraudes de 
montant élevé, afin de **maximiser la marge** de l'enseigne en prenant en compte les gains et les pertes spécifiques 
liés à l'acceptation ou au refus des transactions (TN, FN, FP).

### 5.2. Temps d'apprentissage

L'évaluation doit non seulement analyser les performances (F-mesure, AUC) des algorithmes, mais aussi leur **temps 
d'apprentissage**. Ce critère est essentiel pour justifier si un algorithme est adapté au contexte de **onnées 
massives**. On peut par exemple comparer les résultats obtenus, ainsi que le temps d’apprentissage, entre un modèle 
initial et sa version approchée (comme les *landmarks* ou RFF).

---

En définitive, dans le cadre de la fouille de données massives, la sélection des **mesures de performance** fait 
partie intégrante du protocole expérimental et doit être justifiée par la nature déséquilibrée des données, ce qui 
rend la **F-mesure** et l'**AUC ROC** les critères de choix pour le *tuning* et l'évaluation finale.


