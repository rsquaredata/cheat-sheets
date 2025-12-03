<!--
Title: "Procédures d'apprentissage - Mesures de performance - Classification (Contexte binaire) - Matrice de confusion"
Author: rsquaredata
Last updated: 2025-12-04
-->
## Matrice de confusion : Métriques de classification binaire

La **matrice de confusion** est l'outil fondamental et indispensable pour évaluer et analyser les performances d'un modèle dans le contexte de la **classification binaire**. Elle est particulièrement essentielle lorsque l'on travaille avec des **jeux de données déséquilibrés** (ou *imbalanced*).

## 1. Structure et composantes de la matrice

La matrice de confusion synthétise les résultats d'un classifieur $h$ en comparant les étiquettes prédites ($h(x)$) aux étiquettes réelles ($y$) sur un ensemble de données. Dans un contexte binaire, où les classes sont souvent désignées par $\{0,1\}$ ou $\{−1,+1\}$, elle se décompose en quatre catégories :

| - | **Prédiction Positive** ($h(x)=1$) | **Prédiction Négative** ($h(x)=0 \text{ ou } −1$) |
|-|-|-|
| **Réel Positif** ($y=1$) | **Vrai Positif (TP)** | **Faux Négatif (FN)** |
| **Réel Négatif** ($y=−1 \text{ ou }  0$) | **Faux Positif (FP)** | **Vrai Négatif (TN)** |

1. **Vrai Positif (TP)** : Exemples qui appartiennent réellement à la classe positive (la classe d'intérêt ou minoritaire) et qui sont correctement prédits comme positifs par le modèle.
2. **Faux Négatif (FN)** : Exemples qui sont réellement positifs mais que le modèle a prédits comme négatifs (mal classés).
3. **Vrai Négatif (TN)** : Exemples qui appartiennent réellement à la classe négative (la classe majoritaire) et qui sont correctement prédits comme négatifs.
4. **Faux Positif (FP)** : Exemples qui sont réellement négatifs mais que le modèle a prédits comme positifs (mal classés).

Dans un contexte d'application spécifique, comme la détection de fraude, un TP est une fraude correctement identifiée comme fraude, tandis qu'un FN est une fraude non identifiée comme telle par le modèle.

## 2. Pertinence dans l'apprentissage déséquilibré

La matrice de confusion est vitale dans les problèmes où la classe minoritaire est la cible d'intérêt (anomalies, fraudes bancaires ou fiscales). Dans ces situations, le déséquilibre des classes est souvent extrême (par exemple, seulement **0.28%** des transactions sont frauduleuses).

- **Insuffisance de l'Exactitude (_Accuracy_)** : Simplement minimiser le taux d'erreur (ou maximiser l'Accuracy, $\frac{TP+TN}{m}$) n'est pas adéquat, car cette mesure favorise la classe majoritaire. Un classifieur trivial pourrait atteindre une exactitude de 99% tout en ignorant complètement la classe minoritaire.
- **Nécessité d'une analyse granulaire** : L'étude de la matrice de confusion permet de déterminer le taux de classification **sur la classe positive et sur la classe négative séparément**, révélant la véritable capacité du modèle à détecter la classe minoritaire.

## 3. Mesures de performance dérivées de la matrice

La matrice de confusion est la base à partir de laquelle sont calculées les métriques pertinentes pour les problèmes déséquilibrés.

- **Rappel (_Recall_ ou Sensibilité)** : Mesure la proportion de vrais positifs qui ont été correctement identifiés. C'est la capacité du modèle à **retrouver les exemples de la classe positive ou minoritaire**.

$$
Rappel = \frac{TP}{TP+FN}.
$$

- **Précision (_Precision_)** : Évalue la justesse du modèle dans ses prédictions positives.

$$
Précision = \frac{TP}{TP+FP}.
$$

- **F-mesure ($F_{\beta}$** : C'est la moyenne harmonique entre la Précision et le Rappel. Elle est la mesure principale que l'on cherche à maximiser dans les tâches de détection de fraude. La F-mesure dépend des TP, FN et FP.

- **Taux de Faux Positifs (FPR)** : Le FPR, qui est $\frac{FP}{FP+TN}$, est utilisé conjointement avec le Rappel (ou TPR) pour tracer la courbe ROC et calculer l'AUC ROC.

## 4. Application pratique dans l'apprentissage

Dans les TP, l'analyse des matrices de confusion est une étape requise pour valider le modèle appris.

- **Analyse du déséquilibre - TP** : On examine les **matrices de confusion** pour évaluer le taux de classification sur les classes positive et négative sur des jeux de données spécifiques (Satimage, Segmentation et Yeast).

- **Apprentissage sensible aux coûts (_Cost-Sensitive Learning_)** : Pour contrer les biais, il est possible d'utiliser le paramètre `class_weight` avec l'option "balanced", ce qui modifie le poids des classes. Les **nouvelles matrices de confusions** obtenues après l'application de cette méthode doivent être comparées aux matrices initiales pour évaluer l'impact de l'équilibrage.

- **Évaluation des modèles Deep Learning** : Lors de l'utilisation de plateformes comme H2O, il est demandé de fournir la **matrice de confusion** (ainsi que l'AUC) pour mesurer les performances du modèle sur l'ensemble de validation.
