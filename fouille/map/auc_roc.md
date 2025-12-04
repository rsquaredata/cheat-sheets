<!--
Title: "Procédures d'apprentissage - Mesures de performance - Classification (Contexte binaire) - AUC ROC"
Author: rsquaredata
Last updated: 2025-12-04
-->

## AUC ROC : Métrique d'évaluation pour le classement


L'**AUC ROC** (*Area Under the Receiver Operating Curve*) est une métrique d'évaluation populaire qui joue un rôle essentiel dans la *procédure d'apprentissage** des modèles de **classification binaire**, particulièrement dans les scénarios de données déséquilibrées où le simple taux d'exactitude (*Accuracy*) est insuffisant.

## 1. Définition et contexte du classement (R*anking*)

L'AUC ROC est principalement une mesure de performance adaptée aux problématiques de **classement** (*ranking*).

- **Modèles à score** : Cette métrique est utilisée lorsque le modèle appris ($f$) renvoie un **score**, représentant une confiance dans une prédiction donnée, plutôt qu'une simple étiquette binaire.

- **Calcul probabiliste** : L'AUC ROC peut être définie comme la probabilité qu'un exemple positif ($x^+$) ait un score ($f(x^+)$) supérieur ou égal à celui d'un exemple négatif ($x^−$) :

$$
\text{AUC ROC} = \frac{1}{PN} \sum_{i=1}^P \sum_{j=1}^N \mathbb{1}_{\{f(x_{i^+}) \ge (x_{j^-})\}},
$$

où $P$ est le nombre d'exemples positifs et $N$ le nombre d'exemples négatifs.

## 2. Construction de la Courbe ROC

L'AUC est calculée comme l'**aire sous la courbe ROC**.

- **Tracé de la courbe** : La courbe ROC est tracée en utilisant les différents scores obtenus par le modèle sur l'échantillon de données. Elle met en relation deux métriques issues de la matrice de confusion :

1. Le **Rappel** (_Recall_ ou Sensibilité), également appelé Taux de Vrais Positifs (**TPR**) : $TPR = \frac{TP}{TP+FN}$.
2. Le **Taux de Faux Positifs (FPR)** : $FPR = \frac{FP}{FP+TN}$.

- **Interprétation visuelle** : La courbe ROC permet de **visualiser la performance du modèle**. Le modèle idéal est celui qui atteint un Taux de Vrais Positifs (TPR) égal à **1** tandis que le Taux de Faux Positifs (FPR) reste proche de **0**.

## 3. Interprétation des résultats

L'évaluation de la performance du modèle est directement liée à la valeur de l'AUC :

- **Qualité du modèle** : **Plus la valeur de l'AUC est proche de 1, meilleur est le modèle**.

- **Avantages pratiques** : L'AUC ROC offre plus d'informations sur la qualité du modèle. Elle est utilisée pour choisir le **seuil approprié** pour la tâche donnée. De plus, elle permet de tracer et de choisir le modèle le mieux adapté à un objectif donné en comparant plusieurs modèles sur un seul graphique.

## 4. Rôle dans la procédure d'apprentissage

L'AUC ROC est reconnue comme une mesure cruciale dans la méthodologie expérimentale, en particulier dans l'apprentissage déséquilibré :

- **Choix de la mesure** : Lors de la conception du protocole expérimental, il est nécessaire de choisir une mesure de performance appropriée et de spécifier si l'on souhaite maximiser l'**AUC ROC** ou la F-mesure.
- **Évaluation technique** : Pour l'implémentation d'algorithmes complexes comme les réseaux de neurones profonds via des plateformes comme h2o, il est spécifiquement demandé de mesurer les performances sur l'ensemble de validation et de donner la matrice de confusion ainsi que l'**AUC**.
- C**omparaison** : L'AUC ROC est une métrique permettant de comparer les résultats des différents algorithmes testés lors de la procédure d'apprentissage. Elle est utilisée pour évaluer la qualité d'un modèle en classification en contexte déséquilibré.

---

En résumé, l'AUC ROC fait partie des métriques incontournables, à l'instar de la F-mesure, pour juger de la capacité d'un modèle de classification binaire, notamment dans les cas complexes où la classe minoritaire est difficile à identifier.
