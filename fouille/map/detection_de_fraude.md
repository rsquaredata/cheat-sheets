<!--
Title: "Applications et défis - Données déséquilibrées - étection de fraude (Application)"
Author: rsquaredata
Last updated: 2025-12-04
-->

# Détection de fraude : gestion du déséquilibre et métriques clés

La **détection de fraude* est une application fondamentale et récurrente dans le domaine de la **fouille de données massives**, servant d'exemple typique d'une tâche de **classification binaire** en apprentissage supervisé. Ce domaine d'application pose des défis majeurs, principalement en raison de l'extrême **gestion des données déséquilibrées** (*Imbalanced Learning*).

## 1. La détection de fraude comme problème déséquilibré

La détection de fraude, qu'elle soit bancaire (fraude par chèque, transactions frauduleuses) ou fiscale, se caractérise par un déséquilibre critique des classes:

- **Classe minoritaire** : La fraude ou l'anomalie constitue la **classe positive** ou **classe d'intérêt**.
- **Ratio d'exemples** : Le taux d'exemples frauduleux est extrêmement faible, souvent désigné comme un problème de **Big Data et apprentissage déséquilibré**. Par exemple, le taux de transactions frauduleuses peut être d'environ 1%, 0.6% des transactions (soit 20 000 fraudes sur 3.2 millions d'exemples), ou même descendre à 0.28% en nombre de transactions.

Dans un tel contexte, les algorithmes standards (comme les SVM linéaires) ont tendance à prédire toutes les données comme appartenant à la classe majoritaire, ce qui entraîne de mauvaises performances sur la classe minoritaire. Il est crucial de trouver les exemples de la classe minoritaire, car ils représentent des menaces importantes (fraudes, anomalies).

## 2. Mesures de performance adaptées à la détection de fraude

Puisque chercher à minimiser le taux d'erreur (*Accuracy*) n'est pas une solution pertinente dans un contexte déséquilibré, car cela favorise la classe majoritaire, l'évaluation des modèles de détection de fraude repose sur des mesures spécifiques dérivées de la matrice de confusion.

1. **Rappel (_Recall_)** : Évalue la capacité du modèle à capturer les exemples de la classe minoritaire (fraudes réelles).
2. **Précision (_Precision_)** : Évalue la justesse du modèle dans ses prédictions positives (parmi les cas identifiés comme fraudes, combien le sont réellement).
3. **F-mesure ($F_{\beta}$)** : C'est une moyenne harmonique de la Précision et du Rappel, et c'est la mesure privilégiée dans la détection d'anomalie et de fraude. L'objectif est souvent d'établir le modèle permettant d'obtenir les **meilleurs résultats en terme de F-mesure**.
4. **AUC ROC** : Utilisée pour évaluer la qualité du classement (*ranking*) du modèle, en traçant le Rappel (TPR) par rapport au Taux de Faux Positifs (FPR). Plus sa valeur est proche de 1, meilleur est le modèle.

## 3. Stratégies de gestion du déséquilibre dans les données massives

Face à l'enjeu du déséquilibre, plusieurs stratégies sont recommandées en fouille de données massives :

### 3.1. Stratégies d'échantillonnage (*Sampling*)

Ces méthodes modifient la distribution des données avant l'apprentissage.

- **Oversampling** : Augmenter la population de la classe minoritaire, par exemple avec la technique SMOTE (*Synthetic Minority Over-sampling Technique*) ou ses variantes (BorderlineSMOTE, ADASYN).

- **Undersampling** : Réduire la population de la classe majoritaire, par exemple en utilisant l'échantillonnage aléatoire, les liens Tomek (*Tomek Link*), ou l'ENN (*Edited Nearest Neighbor*).

### 3.2. Apprentissage sensible aux coûts (*Cost-Sensitive Learning*)

L'objectif de la DGFiP ou des organismes bancaires n'est pas seulement d'optimiser une métrique statistique, mais de **minimiser la perte monétaire** due à la fraude fiscale.

- **Matrice de coût** : Cette approche consiste à définir une matrice de coût où les erreurs (Faux Négatifs $c_{FN}$ et Faux Positifs $c_{FP}$) sont pondérées selon leur impact financier.

- **Priorité à la perte monétaire** : Dans un contexte économique, il est crucial d'attribuer un **poids plus important aux fraudes d'un montant élevé** par rapport aux petites fraudes, afin de minimiser les pertes globales.

- **Pondération de classe** : Cette approche peut être mise en œuvre en réglant le paramètre `class_weight` (par exemple sur "balanced") dans les algorithmes, ce qui permet d'accorder plus de poids aux exemples de la classe minoritaire.

---

En résumé, la détection de fraude est un problème critique de classification en contexte massif et déséquilibré. La méthodologie de résolution doit nécessairement inclure non seulement des algorithmes robustes (comme le boosting ou les forêts aléatoires), mais aussi un protocole expérimental rigoureux utilisant la **F-mesure** ou l'**AUC ROC**, et souvent des stratégies avancées de pondération ou d'échantillonnage pour faire face au déséquilibre et à la **minimisation des pertes financières**.


