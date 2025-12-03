<!--
Title: "Procédures d'apprentissage - Mesures de performance - Classification (Contexte binaire) - Rappel / Précision / F-mesure"
Author: rsquaredata
Last updated: 2025-12-04
-->
# Rappel, Précision et F-mesure : Évaluation du déséquilibre

L'évaluation de la performance dans un contexte de **classification binaire** est fondamentale pour la **procédure d'apprentissage**, particulièrement dans les scénarios de **fouille de données massives** où le **déséquilibre des classes** (*Imbalanced Learning*) est la norme. Dans ce cadre, les mesures traditionnelles comme l'exactitude (*Accuracy*) sont insuffisantes, d'où la nécessité de recourir au **Rappel**, à la **Précision** et à la **F-mesure**.

## 1. Le contexte de l'apprentissage déséquilibré

Dans les applications courantes (telles que la détection de fraude bancaire ou fiscale), la classe d'intérêt (la classe positive) est souvent une **classe minoritaire**. Par exemple, le taux de fraude peut être aussi faible que 0.6% ou 0.28% des transactions.

Dans ce contexte :

- Le but est de **retrouver les exemples de la classe minoritaire** (fraudes, anomalies)
- Chercher à minimiser le taux d’erreur (*Error rate*) **n'est pas une bonne solution**, car cette métrique tend à favoriser la classe majoritaire. Un classifieur trivial qui prédit toujours la classe majoritaire peut atteindre une exactitude de 99% mais ne serait pas satisfaisant car il n'aurait pas capturé la classe positive.

Les mesures pertinentes sont donc basées sur les résultats de la **matrice de confusion**.

## 2. Rappel (Recall ou Sensibilité)

Le **Rappel** (ou Sensibilité) évalue la capacité du modèle à **retrouver des exemples de la classe positive ou minoritaire**.

- **Définition** : Il est calculé comme la proportion des Vrais Positifs (TP) parmi tous les exemples qui étaient réellement positifs (TP + Faux Négatifs (FN)) :

$$
Rappel = \frac{TP}{TP+FN}.
$$

- **Importance** : Cette mesure est cruciale car elle indique le pourcentage d'exemples de la classe d'intérêt que le modèle a réussi à capturer. Dans des domaines comme le diagnostic médical, un Rappel élevé est vital pour ne pas manquer un patient atteint d'une maladie grave.

## 3. Précision (Precision)

La Précision évalue la justesse ou la **confiance du modèle dans ses prédictions positives**.

- **Définition** : Elle est calculée comme la proportion de Vrais Positifs (TP) parmi toutes les prédictions positives effectuées par le modèle (TP + Faux Positifs (FP)) :

$$
Précision = \frac{TP}{TP+FP}.
$$

- **Importance** : Un modèle avec une haute précision signifie que lorsqu'il classe un exemple comme positif, il a de bonnes chances d'avoir raison. Ceci est important dans des contextes bancaires où refuser un droit ou un service à un individu doit être basé sur une décision juste.

## 4. La F-mesure (F-measure)

La **F-mesure ($F_{\beta}$)** est la mesure de performance la plus souvent utilisée dans les scénarios déséquilibrés, car elle constitue une **moyenne harmonique** de la Précision et du Rappel. Elle est hautement utilisée dans la détection de fraude et d'anomalie.

- **Formulation** : La F-mesure dépend d'un paramètre $\beta$ qui permet de pondérer l'importance du Rappel par rapport à la Précision :

$$
F_{\beta} = \frac{(1 + \beta^2)TP}{(1 + \beta^2)TP + \beta^2 FN + FP}.
$$

- **Cas $F_1$** : Lorsque $\beta = 1$, le Rappel et la Précision reçoivent le même poids. La formule se simplifie alors :

$$
F = \frac{2TP}{2TP+FN+FP}.
$$

- **Rôle de $\beta$** : L'utilisateur peut contrôler l'importance donnée au rappel (si $\beta \gt 1$) ou à la Précision (si $\beta \lt 1$).

- **Objectif pratique** : Dans les TP, l'objectif principal est souvent d'établir le modèle permettant d'obtenir les *meilleurs résultats en classification en terme de F-mesure**.

## 5. Défis de la F-mesure dans l'entraînement

Malgré son utilité pour l'évaluation, la F-mesure présente des inconvénients majeurs dans la phase d'entraînement et de *tuning* :

- Elle est **non-linéaire et non-convexe**.
- Ces propriétés rendent son **optimisation difficile** car les algorithmes standards de descente de gradient ne peuvent pas l'optimiser directement, risquant de tomber dans des optima locaux.
- De plus, la non-linéarité rend difficile l'établissement de **garanties de généralisation théoriques** pour cette mesure.

## 6. Rôle des mesures dans la procédure d'apprentissage

Ces mesures sont utilisées de manière centrale dans la procédure pour évaluer et comparer rigoureusement les algorithmes.

- **Tuning** : La F-mesure est souvent le critère utilisé pour choisir les meilleurs hyperparamètres du modèle (par exemple, via la validation croisée).
- **Comparaison** : L'évaluation de modèles concurrents doit considérer ces métriques. Par exemple, une comparaison montre qu'un modèle peut avoir une précision parfaite (1.0) mais un rappel très faible (0.3), résultant en une F-mesure de 0.46, tandis qu'un autre modèle avec une précision plus faible (0.6) mais un rappel élevé (0.9) atteint une meilleure F-mesure (0.72).
- **Techniques d'amélioration** : Des méthodes comme le *Cost-Sensitive Learning* (apprentissage sensible aux coûts) peuvent être utilisées pour modifier la pondération des classes minoritaires, par exemple en utilisant le paramètre `class_weight` avec l'option "balanced", afin d'améliorer spécifiquement la performance sur ces mesures.

