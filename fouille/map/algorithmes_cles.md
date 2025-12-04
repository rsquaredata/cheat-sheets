<!--
Title: "Apprentissage massif et données déséquilibrées - Algorithmes clés"
Author: rsquaredata
Last updated: 2025-12-04
-->

# Panorama des algorithmes de fouille massive

Voici une vue d'ensemble des **algorithmes clés** en contexte de **fouille de données massives**, en mettant l'accent sur leur application, leurs performances, et les défis liés à leur mise à l'échelle (scalabilité). Ces algorithmes couvrent à la fois l'apprentissage supervisé (classification et régression) et des techniques d'apprentissage non supervisé ou d'ensemble.

## 1. Algorithmes de base en apprentissage supervisé

Les algorithmes fondamentaux sont souvent les points de départ pour la construction de solutions plus complexes.

### 1.1. Séparateurs à Vaste Marge (SVM)

Les SVM sont des algorithmes largement utilisés pour la **classification binaire**.

- **Principe** : Les SVM cherchent l'hyperplan qui maximise la marge entre les deux classes afin de maximiser la capacité de généralisation.
- **Version linéaire (Hard/Soft Margin)** : Le *Hard Margin SVM* est la solution idéale lorsque les données sont linéairement séparables, en minimisant la norme $\Vert w \Vert_2^2$ soumise à des contraintes. Le *Soft Margin SVM* introduit des **variables _slacks_** ($\xi_i$) et un **hyperparamètre de régularisation** $C$ pour tolérer les erreurs et le non-linéarité.
- **Méthodes à noyaux (_Kernel SVM_)** : Pour traiter les données non linéairement séparables, l'**astuce du noyau** (*Kernel trick*) permet de projeter implicitement les données dans un espace de dimension supérieure (potentiellement infinie) où elles deviennent séparables linéairement. Les noyaux incluent le **noyau linéaire** et le **noyau gaussien (RBF)**, qui nécessite le tuning des hyperparamètres $C$ et $\gamma$.
- **Limites et scalabilité** : Les méthodes à noyaux classiques sont **très limitées** dans un contexte de données massives, car elles nécessitent le calcul de $m^2$ similarités (matrice de Gram $K \in \mathbb{R}^{m \times m}$), ce qui est irréalisable pour le *Big Data*.

## 1.2. Régression logistique (Logistic Regression)

Similaire aux SVM linéaires car il apprend également un séparateur linéaire, la régression logistique se distingue en modélisant la **probabilité** qu'un exemple appartienne à une classe donnée.

- **Objectif** : Estimer le logarithme du rapport de chances (*log odds*) comme une fonction linéaire $h(w,b,x)=b+ \langle x,w \rangle⟩$.

- **Perte (_Loss_)** : Le modèle est estimé en minimisant la **négative log-vraisemblance** (utilisant la *Logistic loss*). Cette perte pénalise exponentiellement les erreurs, ce qui la rend sensible aux valeurs aberrantes (*outliers*) comparativement au SVM.

- **Classification multiple** : La régression logistique peut être étendue à la classification multi-classes (régression logistique multinomiale) en utilisant la fonction **Softmax** pour calculer les probabilités d'appartenance à chaque classe.

### 1.3. k-Plus Proches Voisins (k-NN)

Le k-NN est une méthode **non paramétrique** qui ne fait aucune hypothèse sur la distribution sous-jacente.

- **Principe** : L'étiquette d'un nouvel exemple est prédite par un **vote majoritaire** parmi ses $k$ voisins les plus proches dans l'ensemble d'entraînement.

- **Défis de scalabilité** : Le k-NN est peu adapté aux grands jeux de données car il nécessite de **stocker l'ensemble des données d'entraînement** et de calculer toutes les distances pour chaque prédiction, ce qui a une complexité en $O(nd+nk)$.

- **Améliorations** : L'efficacité peut être améliorée par la sélection d'un sous-ensemble pertinent d'exemples (*Condensed Nearest Neighbor*) ou par l'utilisation de poids (inverses de la distance) pour la décision (*Weighted k-Nearest Neighbors*).

### 1.4. Arbres de décision (Decision Trees) et forêts aléatoires (Random Forests)

Les arbres de décision, utilisés pour la classification et la régression, séparent les données en appliquant une série de règles pour créer des groupes homogènes.

- **Forces** : Ils sont faciles à interpréter, peuvent gérer différents types de données (numériques ou catégoriques), et sont appropriés pour de grands ensembles de données.

- **Faiblesses** : Un arbre unique a une forte instabilité et un risque élevé de **sur-apprentissage** (*over-fitting*).

- **Forêts aléatoires (Random Forests)** : Pour pallier l'instabilité, les RF combinent plusieurs arbres de décision par la méthode du **Bagging** (**Bootstrap Aggregating**), qui implique un **double échantillonnage** (sur les exemples et sur les variables) pour créer de la diversité et réduire la variance des résultats. Les RF sont particulièrement performantes car elles utilisent des classifieurs de base à **haute variance et faible biais** (arbres profonds).

## 2. Algorithmes avancés et techniques d'ensemble

Pour optimiser les performances sur les données massives et faire face au déséquilibre, des méthodes plus sophistiquées sont nécessaires.

### 2.1. Boosting (Adaboost et Gradient Boosting)

Le Boosting est une méthode d'ensemble qui vise à combiner plusieurs **apprenants faibles** (*weak learners*) pour construire un **apprenant fort**.

- **Adaboost** : Fonctionne itérativement en modifiant la distribution des poids des exemples, se focalisant sur les données mal classées par le modèle précédent, en utilisant la **perte exponentielle**.

- **Gradient Boosting (GB)** : Généralisation d'Adaboost où l'optimisation est réalisée dans l'espace fonctionnel, en utilisant le **gradient négatif (pseudo-résidus)** de la fonction de perte pour former le nouvel apprenant faible à chaque étape.

- **XGBoost** : Variante très efficace de Gradient Boosting basée sur des **arbres de faible profondeur** (*Gradient Tree Boosting*). Il utilise une approximation d'ordre 2 de la fonction de perte et intègre des termes de régularisation pour contrôler la complexité des arbres. XGBoost est souvent privilégié pour les données volumineuses en raison de son apprentissage rapide et de sa capacité à être parallélisé.

### 2.2 Deep Learning (Réseaux de Neurones Profonds)

Les réseaux de neurones (Deep Learning) sont utilisés pour l'implémentation de modèles complexes, notamment pour les données d'images (MNIST, CIFAR10).

- **Architecture** : Les modèles sont construits avec des couches d'entrée, des **couches cachées** (avec fonctions d'activation comme la **Tanh**), et une couche de sortie. La complexité dépend du nombre de couches et de neurones par couche.

- **Entraînement** : Le processus utilise l'algorithme de **rétropropagation** (*Back-Propagation*), qui applique la règle de la chaîne pour mettre à jour les poids par descente de gradient.

- **Outils pour les données massives** : Des librairies comme **H2O** et **Keras** sont utilisées pour distribuer les calculs et rendre l'implémentation flexible.

## 3. Algorithmes et techniques d'approximation pour la scalabilité

Face aux contraintes du *Big Data*, plusieurs méthodes sont proposées pour rendre les algorithmes non scalables (comme les SVM à noyaux) utilisables :

- **Landmarks** : Technique consistant à sélectionner un **sous-ensemble $k \lt m$ de points de repère** (choisis aléatoirement ou par un algorithme comme k-means) pour l'apprentissage du modèle à noyau, réduisant ainsi le nombre de similarités à calculer.

- **Random Fourier Features (RFF)** : Méthode d'approximation des noyaux invariants par translation (comme le noyau gaussien) qui consiste à** projeter les données via une transformation aléatoire** $u : \mathbb{R}^d \to \mathbb{R}^p$ (où $p$ est petit) et à apprendre un **séparateur linéaire** rapide sur ces données projetées.

- **Apprentissage hybride** : Il est fortement recommandé de combiner des algorithmes supervisés avec des méthodes non supervisées (comme le **k-means** ou les **auto-encodeurs**), ou des techniques d'échantillonnage (*sampling*) pour améliorer les performances sur les données déséquilibrées et s'adapter aux contraintes massives.

## 4. Algorithmes non supervisés

Les algorithmes non supervisés sont également utilisés pour la détection d'anomalies et la segmentation :

- **Clustering (k-means)** : Peut être utilisé pour détecter des fraudes ou segmenter des populations. Il peut être combiné à des algorithmes supervisés.

- **One-Class SVM** : Une version non supervisée du SVM, utilisée pour la **détection d'anomalies**, où l'objectif est de séparer les exemples "normaux" des exemples "aberrants" en trouvant la plus petite sphère ou l'hyperplan qui englobe la majorité des données
