<!--
Title: "Applications et défis - Données déséquilibrées - Paramètre class_weight ('balanced')"
Author: rsquaredata
Last updated: 2025-12-04
-->

# Pondération `class_weight` : Apprentissage sensible aux coûts

L'utilisation du **paramètre `class_weight` avec l'option "balanced"** est une méthode essentielle de *gestion des données déséquilibrées* (*Imbalanced Learning*), s'inscrivant dans le cadre plus large de l'**apprentissage sensible aux coûts** (*Cost-Sensitive Learning*).

## 1. Contexte du déséquilibre de classes

Dans les problèmes de classification binaire réels, comme la détection de fraude, la classe minoritaire est souvent largement sous-représentée (le taux de fraude peut être très faible, par exemple 0.28%). Ce déséquilibre amène les algorithmes classiques (qui cherchent à minimiser le taux d'erreur, ou *Accuracy*) à privilégier la classe majoritaire, conduisant à des performances insatisfaisantes sur la classe d'intérêt (la classe minoritaire).

## 2. Le paramètre classe_weight et l'apprentissage sensible aux coûts

L'**Apprentissage sensible aux coûts* est une stratégie qui modifie la manière dont les erreurs sont évaluées par l'algorithme. Le principe est d'attribuer des poids différents aux erreurs, en particulier celles concernant la classe minoritaire, pour forcer le modèle à s'y concentrer.

Le paramètre `class_weight`, notamment avec l'option **"balanced"**, est la façon la plus courante d'implémenter cette approche dans les implémentations standard (comme dans les librairies Python).

### 2.1. Rôle et mécanisme de "balanced"

L'option **"balanced"** du paramètre `class_weight` permet de modifier le poids des classes lorsque le jeu de données est déséquilibré. Son objectif est de garantir que les classes en présence aient le **même poids** dans la fonction de perte (*loss*) ou d'objectif du modèle, et ce malgré leur déséquilibre effectif dans le jeu de données d'entraînement.

En donnant un poids plus important aux exemples de la classe minoritaire (les Faux Négatifs coûtent plus cher qu'un Faux Positif), le modèle est incité à minimiser les erreurs sur cette classe.

### 2.2. Application pratique

cf. TD3

On utilise `class_weight="balanced"` pour contrer les effets du déséquilibre sur des jeux de données spécifiques (Satimage, Segmentation et Yeast6) en utilisant un SVM linéaire.

Il est demandé de :
1.  Observer les performances sur la classe positive et la classe négative (via la matrice de confusion) sans pondération.
2.  Utiliser le paramètre class_weight avec l'option **"balanced"**.
3.  Comparer les **nouvelles matrices de confusion** obtenues avec celles du cas non pondéré pour évaluer l'impact de la modification des poids des classes.

## 3. Contexte plus large et alternatives

L'utilisation de `class_weight` n'est qu'une des méthodes de gestion du déséquilibre, souvent préférée à l'**échantillonnage** (oversampling ou undersampling). L'échantillonnage modifie physiquement la taille de la base de données en dupliquant (oversampling) ou en supprimant (undersampling) des exemples, tandis que `class_weight` **modifie la fonction de perte** pour accorder plus d'importance aux exemples minoritaires sans changer la taille du jeu de données.

En pratique, pour la détection de fraudes, il est même possible d'aller plus loin que l'option "balanced" en attribuant des poids différents à chaque exemple, en fonction par exemple du **montant de la fraude** (pour minimiser la perte monétaire totale), ce qui maximise la marge au lieu d'une métrique statistique simple.

---

En résumé, le paramètre `class_weight="balanced"` est une technique d'apprentissage sensible aux coûts permettant d'équilibrer l'influence des classes lors de l'entraînement des modèles de classification, ce qui est crucial pour obtenir des performances fiables lorsque les données sont déséquilibrées.

