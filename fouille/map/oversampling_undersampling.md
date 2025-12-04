<!--
Title: "Applications et défis - Données déséquilibrées - Oversampling/Undersampling"
Author: rsquaredata
Last updated: 2025-12-04
-->

# Stratégies d'échantillonnage contre le déséquilibre des classes

Les techniques d'**oversampling** et d'**undersampling** sont des **stratégies d'échantillonnage** cruciales utilisées dans le contexte plus large de la gestion des données déséquilibrées (*Imbalanced Learning*), notamment dans les applications de classification binaire comme la détection de fraude.

Le déséquilibre des classes pose un défi majeur car les algorithmes standards (comme les SVM ou k-NN) ont tendance à favoriser la classe majoritaire, ce qui conduit à de mauvaises performances sur la classe minoritaire d'intérêt (fraudes, anomalies). Les stratégies d'échantillonnage visent à modifier la distribution des données pour rendre l'apprentissage plus efficace.

On distuinue deux grandes catégories de méthodes d'échantillonnage :

## 1. Oversampling (Sur-échantillonnage)

L'oversampling consiste à augmenter le nombre d'exemples de la **classe minoritaire** afin de réduire le déséquilibre. Il existe plusieurs approches :

### 1.1. Oversampling aléatoire (*Random OverSampler*)

C'est l'approche la plus simple, consistant à **dupliquer** des points existants de la classe minoritaire.

- **Mécanisme** : En dupliquant des points, on demande à l'algorithme de se concentrer davantage sur les zones de l'espace où ces points ont été répliqués.

- **Inconvénient** : Le processus est totalement aléatoire et, par conséquent, il **n'ajoute pas d'information nouvelle** aux données et n'offre aucun contrôle sur le processus.

### 1.2. SMOTE (Synthetic Minority Over-sampling Technique)

SMOTE est une méthode plus sophistiquée qui génère de **nouveaux exemples synthétiques** au lieu de simplement dupliquer les anciens.

- **Mécanisme** : L'algorithme sélectionne aléatoirement un exemple de la classe minoritaire ($p_i$), cherche ses $k$-plus proches voisins positifs, sélectionne l'un de ces voisins ($r_{NN_i}), puis crée un nouvel exemple ($x_{new}$) sur le segment de droite reliant $p_i$ et $r_{NNi_}$ en utilisant un nombre aléatoire $\alpha \in \[0,1\]$.

- **Avantages** : SMOTE permet d'ajouter de l'information synthétique (et non du bruit) et peut se concentrer sur certaines zones, bien que l'oversampling puisse parfois générer du bruit dans les données.

### 1.3. Variantes de SMOTE (BorderlineSMOTE, ADASYN)

Ces variantes cherchent à concentrer la génération de points synthétiques sur les exemples qui sont difficiles à classer :

- **BorderlineSMOTE** : Cette méthode impose une règle de sélection sur les exemples de la classe minoritaire utilisés pour la génération. Elle identifie les exemples "**borderline**" comme ceux dont la majorité (mais pas tous) des $k'$ voisins appartiennent à la classe majoritaire. L'algorithme se concentre ensuite sur ces exemples difficiles à classer pour appliquer la procédure SMOTE, tout en laissant de côté les exemples faciles à classer ou ceux qui sont du bruit.

- **ADASYN** : Cette approche est une version plus lisse que BorderlineSMOTE et cherche à **concentrer la génération sur les exemples plus difficiles à classer**. Elle définit un score de risque $r_i$ pour chaque exemple minoritaire, proportionnel au nombre de voisins de la classe majoritaire, et génère plus d'exemples autour des données ayant un score de risque élevé.

## 2. Undersampling (Sous-échantillonnage)

L'undersampling consiste à **réduire ou supprimer des exemples de la classe majoritaire** afin de réduire le déséquilibre, sans toucher à la classe minoritaire.

### 2.1. Undersampling aléatoire (Random Undersampling)

Il s'agit de supprimer aléatoirement des exemples de la classe majoritaire.

- **Avantages** : Peut rendre les frontières de décision plus nettes entre les classes.

- **Inconvénient** : Ce processus aléatoire risque de **perdre de l'information**, potentiellement importante.

### 2.2. Méthodes basées sur les voisins (Nettoyage de données)

Ces méthodes introduisent une règle de suppression pour éviter la perte d'information critique et se concentrent sur le nettoyage des frontières :

- **Condensed Nearest Neighbor (CNN)** : C'est une méthode de pré-traitement qui sélectionne un sous-ensemble $S'$ de l'ensemble d'apprentissage $S$, tel que le 1-NN (1-plus proche voisin) utilisant $S'$ peut classifier les exemples presque aussi bien qu'avec $S$. Elle vise à accélérer l'algorithme $k$-NN.

- **Tomek Link** : C'est une méthode de nettoyage de données qui identifie les paires d'exemples de classes différentes qui sont les plus proches voisins l'un de l'autre (formant un "lien Tomek"). Ces liens se trouvent souvent à la frontière et peuvent indiquer une mauvaise classification. On peut choisir de retirer ces exemples de la classe majoritaire (ou les deux). L'approche **One Sided Selection** combine CNN puis Tomek Link pour réduire la taille du jeu de données de manière plus conséquente.

- **Edited Nearest Neighbor (ENN)** : Très similaire à Tomek Link, ENN supprime un exemple de la classe majoritaire si la prédiction (généralement 3-NN) ne coïncide pas avec sa vraie étiquette.

# 3. Usage pratique et alternatives

L'oversampling et l'undersampling sont des techniques clés du **pre-process** sur les données, utilisables seules ou couplées à un algorithme de classification.

- **Implémentation** : Ces méthodes peuvent être implémentées facilement via la librairie Python `ìmblearn.over_sampling`, qui propose des fonctions comme `RandomOverSampler`, `SMOTE`, `BorderlineSMOTE` et `ADASYN`, ainsi que leurs équivalents pour l'undersampling. La quantité d'exemples générés ou supprimés est généralement déterminée par le **ratio final de positifs souhaité** dans le jeu de données.

- **Alternatives** : Une alternative à l'échantillonnage pour gérer le déséquilibre est l'**apprentissage sensible aux coûts** (*Cost-Sensitive Learning*), qui consiste à modifier la pondération des erreurs pour accorder plus d'importance à la classe minoritaire. Par exemple, l'utilisation du paramètre `class_weight` avec l'option "balanced" dans des algorithmes comme le SVM permet de donner le même poids aux classes malgré le déséquilibre initial.


