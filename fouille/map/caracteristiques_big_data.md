<!--
Title: "Applications et défis - Caractéristiques Big Data"
Author: rsquaredata
Last updated: 2025-12-04
-->

# Volume, variété, vitesse : Défis de la fouille de données massives

Les **caractéristiques du Big data** déterminent non seulement la nature des problèmes rencontrés, mais aussi la nécessité de développer des algorithmes et des architectures spécifiques pour l'analyse.

## 1. Les caractéristiques fondamentales du Big Data

Historiquement, l'analyse de données se faisait sur des quantités de données et de variables très faibles. Avec l'augmentation constante de la capacité de stockage des entreprises, et l'émergence des réseaux sociaux et des données génomiques, la capacité à analyser des données en quantités importantes et avec un grand nombre de variables est devenue possible.

Les principales caractéristiques du Big Data qui expliquent l'engouement pour ces données et qui engendrent des défis spécifiques sont :

### 1.1 Volume (grande taille de l'échantillon)

Les applications du Big Data sont définies par la grande quantité d'observations disponibles, souvent trop volumineuses pour être analysées par une seule machine.

- **Exemples de jeux de données massifs** :  **10 mois de transactions** par carte bancaire représentent environ **3,2 millions de transactions** ; une base de données de transplantation comporte **plus d’un million de greffes**. Le jeu de données CIFAR10 comporte **60 000 exemples (images)**.

- **Défis** : Ce volume nécessite l'utilisation d'**architectures complexes** pour distribuer les calculs (comme Hadoop ou Spark) ou l'emploi d'outils optimisés comme h2o et Keras pour contourner les barrières techniques.

### 1.2. Variété (grande dimension et types de données)

La variété fait référence au fait que les données présentent un grand nombre de variables ou de types différents.

- **Exemples de dimensions** : Les données génomiques se caractérisent par un **très grand nombre de variables**. Une mise en situation concernant la transplantation d'organes peut comporte plus de **1000 caractéristiques**.
- **Types de variables** : Les données peuvent être composées de variables numériques, catégorielles, ordinales, de séries temporelles, d'images (comme MNIST et FASHION MNIST), de vidéos, de graphes, etc.. Par exemple, une base de données de fraude bancaire comprend des descripteurs quantitatifs, des variables ordinales et une variable catégorielle.
- **Conséquence (Malédiction de la dimension)** : La classification avec des données de grande dimension peut entraîner une faible performance si les caractéristiques non informatives ont un impact trop grand dans le calcul des distances.

### 1.3. Vitesse (contraintes temporelles)

Les contraintes temporelles des applications pratiques incluent notamment la rapidité nécessaire pour l'apprentissage et la prédiction.

- **Rapidité d'inférence** : Dans la gestion d'un flux massif de données permanent, les modèles doivent répondre **rapidement (environ 20 ms)**.
- **Temps d'apprentissage** : Il est crucial de comparer les performances des algorithmes non seulement en termes de précision, mais aussi en termes de **temps d'apprentissage**, afin de déterminer s'ils sont adaptés au contexte des données massives.
- **Données datées** : Lorsque les données sont datées (comme des transactions bancaires), la procédure de validation (apprentissage, validation croisée et test) doit respecter l'ordre temporel, imposant des ensembles d'entraînement et de test fixes basés sur la date.

## 2. Défis et conséquences pour l'apprentissage

Ces caractéristiques du Big Data soulèvent des défis majeurs qui guident la méthodologie de la fouille de données massives :

### 2.1. Problèmes de scalabilité (Passage à l'échelle)

Le volume et la dimensionnalité des données rendent les algorithmes classiques inefficaces :

- **Limitation des méthodes à noyaux** : Les SVM utilisant des noyaux (comme le noyau gaussien) ne sont pas réalisables dans un contexte de Big Data, car ils nécessitent le calcul de $m^2$ similarités (où $m$ est le nombre d'exemples), ce qui est trop coûteux lorsque $m$ est considérable.

- **Approximation nécessaire** : Pour rendre ces méthodes utilisables, il est nécessaire d'employer des techniques d'approximation comme les **Landmarks** (points de repère) ou les **Random Fourier Features (RFF)**.

- **Complexité de k-NN** : L'algorithme des $k$-plus proches voisins est **peu adapté aux grands jeux de données** car sa complexité dépend de la taille de l'échantillon ($O(nd+nk)$) et il doit conserver l'intégralité des données en mémoire.

111 2.2. Problème du déséquilibre des classes (_Imbalanced Learning_)

De nombreuses applications réelles du Big Data (détection de fraudes bancaires ou fiscales) présentent un **déséquilibre de classes** sévère, où la classe minoritaire est la cible d'intérêt.

- **Ratios extrêmes** : Le taux de fraude peut être inférieur 0.6% dans les jeux de données de transactions.

- **Conséquence sur la performance** : Dans ce contexte, l'exactitude (*Accuracy*) est une mesure inappropriée car elle favorise la classe majoritaire. Cela nécessite l'utilisation de **mesures spécifiques** comme la F-mesure et l'AUC ROC.

- **Stratégies d'adaptation** : Pour relever ce défi, il est souvent nécessaire d'appliquer des stratégies de pré-traitement (oversampling, undersampling) ou d'utiliser des approches sensibles aux coûts (*Cost-Sensitive Learning*) pour pondérer l'importance de la classe minoritaire, voire le montant de la perte monétaire associée.

---

Le domaine de la fouille de données massives est donc intrinsèquement lié à la nécessité de développer des **outils d'analyse et de traitement sophistiqués** capables d'opérer sous ces fortes contraintes de volume, de variété et de vitesse.
