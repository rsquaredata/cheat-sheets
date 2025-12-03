<!--
Title: "Apprentissage massif et données déséquilibrées"
Author: rsquaredata
Last updated: 2025-12-03
-->

Thèmes principaux et concepts abordés :
1. Fondamentaux du Marchine Learning et Processus d'apprentissage
2. Théorie de la généralisation et Stabilité
3. Algorithmes centraux et leurs défis en Big Data
4. Gestion des problèmes spécifiques : Données déséquilibrées
5. Protocole expérimental et ingénierie des modèles

# 1. Fondamentaux du Machine Learning et Processus d'apprentissage

Le **processus d'apprentissage** d'un modèle comprend plusieurs étapes essentielles :
1. **Préparation des données** : Détection d'éventuelles valeurs manquantes et imputation, et identification des caractéristiques des jeux de données (taille, dimension, type de problème, ratio des classes).
2. **Mise en œuvre**: Application de plusieurs algorithmes, mise en œuvre de méthodes de pré-apprentissage (*pre-processing*), et choix d'une mesure de performance appropriée.
3. **Évaluation** : Comparaison des performances des algorithmes, en tenant compte à la fois des performances et du **temps d’apprentissage**.

Le **Big Data** se caractérise par des données volumineuses et complexes, et l'engouement pour ce sujet s'explique par le développement constant de la capacité de stockage des données par les entreprises.

Les trois grandes branches du Machine Learning sont les suivantes :
1. Apprentissage supervisé.
2. Apprentissage non supervisé.
3. Aprentissafe par renforcement.

# 2. Théorie de la généralisation et Stabilité

**Théorie des bornes en généralisation** : L'objectif est de borner l'écart $\vert R_{\ell} - R_{\ell}^S \vert$ entre le **risque réel** ($R_{\ell}$), sur la distribution inconnue $D$, et le **risque empirique** ($R_{\ell}^S$), sur le jeu d'entraînement, avec une certaine probabilité $1 - \delta$.

**Stabilité uniforme** : La stabilité uniforme ne repose pas sur une mesure de complexité de l'espace d'hypothèse.
- Un algorithme est **uniformément stable** avec une constante $\beta \gt 0$ si la modification d'un seul exemple dans l'ensemble d'entraînement n'entraîne qu'une faible modification de la *loss*.
- L'**_inégalité de McDiarmid_** (théorème) est l'outil probabiliste essentiel utilisé pour établir le **_théorème de généralisation par borne uniforme_** (ou borne de généralisation Bousquet-Elisseeff) qui fournit la borne de généralisation en fonction de $\beta$ et de la taille de l'échantillon $m$.
- Dans le contexte du SVM, la constante de stabilité uniforme $\beta$ est donnée par $\beta = \frac{2 \sigma^2}{\lambda m}$. L'interprétation de la borne obtenue est que l'erreur décroît avec le nombre d'exemples $m$ que la borne diminue avec l'augmentation de l'hyper-paramètre $\lambda$ (régularisation).

# 3. Algorithmes centraux et leurs défis en Big Data

Plusieurs algorithmes sont étudiés en profondeur, en insistant sur leurs limites en contexte de données massives :

## 3.1. Séparateurs à Vaste Marge (SVM)

- Le SVM cherche à maximiser la marge $\gamma = \frac{2}{\Vert w \Vert_2}$. Maximiser la marge revient à minimiser $\frac{1}{2} \Vert w \Vert_2^2 $.
- Le problème d'optimisation classique est celui du *Soft Margin SVM* qui utilise les variables *slacks* $\xi_i$ et l'hyper-paramètre $C$ (ou la *Hinge Loss* $\frac{C}{m} \sum_{i=1}^m [y_i(\langle w, x_i \rangle + b)]_{+}$).
- Les exemples qui participent à la construction de la frontière de décision sont les **vecteurs supports**.
- **Méthodes à Noyaux** : L'astuce du noyau (*kernel trick*), basée sur le *théorème de Mercer*, permet de projeter implicitement les données dans un espace latent $Z$, potentiellement de dimension infinie, où la séparation est linéaire. Des exemples de noyaux étudiés sont le linéaire, le polynomial, et le gaussien.
- **Limite en Big Data** : Les méthodes à noyaux classiques sont très limitées car elles nécessitent de calculer $m^2$ similarités (la matrice $K \in \mathbb{R}^{m \times m}$) et le modèle appris comporte $m$ paramètres, rendant l'inversion de la matrice $K$ irréalisable en Big Data.

## 3.2. Approximation des méthodes à noyaux (solution Big Data)

Face aux limites du SVM, les sources explorent des méthodes d'approximation :
- **Landmarks (Points de repère)** : L'idée est de sélectionner $k \gt m$ points de repère (possiblement via $k$-means) dans l'ensemble de données pour réduire le nombre de similarités à calculer et le nombre d'exemples.
- **Random Fourier Features (RFF)** : Méthode d'approximation pour les noyaux invariants par translation (comme le noyau gaussien). L'objectif est d'approximer le produit interne du noyau $k(x, x')$ par une projection aléatoire $u$ des données dans un espace de dimension potentiellement très petite $p$. Cette approximation se base sur le *théorème de Bochner* et utilise une **somme de fonctions cosinus**.
- **Combinaison RFF et Boosting** : Exploration de la combinaison des RFF avec du Gradient Boosting pour créer des approximations performantes et rapides.

## 3.3. Boosting et forêts aléatoires

Ces méthodes ensemblistes sont importantes pour leur robustesse et leur performance :
- Le **Boosting** vise à combiner plusieurs apprenants faibles (*weak learners*) de manière itérative pour construire un modèle fort.
- L'algorithme **Adaboost** apprend les hypothèses itérativement en **repondérant les exemples** pour se concentrer sur les erreurs commises par le modèle précédent. Adaboost est basé sur l'*exponential loss*.
- Le **Gradient Boosting** est une généralisation plus flexible, capable d'utiliser **n'importe quelle fonction de loss**. Il apprend les apprenants faibles (*weak learners*) en les ajustant sur les **pseudo-résidus** (le gradient négatif de la loss).
- **XGBoost** (eXtreme Gradient Boosting) est une implémentation très efficace, utilisant une a**pproximation d’ordre 2** de la fonction objectif pour mesurer la pureté d'une feuille.
- Les **forêts aléatoires** (Random Forests) utilisent un **double échantillonnage** (sur les données et sur les variables) pour créer des modèles diversifiés, augmentant la stabilité et la généralisation.

## 3.4. Deep Learning

Pour les problèmes impliquant des données images (MNIST, CIFAR10), on utilise des outils pour contourner les barrières techniques liées au volume de données :
- **H2O** : Plateforme de ML générale, permettant d'exécuter des algorithmes (Deep Learning, Gradient Boosting, K-means) sur plusieurs cœurs ou *clusters*.
- **Keras** : Librairie Python dédiée à l'implémentation facile des réseaux de neurones, offrant plus de flexibilité que H2O.
- **Réseaux de Neurones Convolutionnels (CNN)** : Type de réseau nécessaire pour la classification ou la segmentation sémantique d'images, utilisant des couches de convolution et de *max-pooling* pour l'extraction de *features*.

# 4. Gestion des problèmes spécifiques : Données déséquilibrées

La problématique des données déséquilibrées est un axe majeur, illustrée par des applications comme la détection de fraudes bancaires ou fiscales (où le taux de fraude peut être très faible, par exemple 1% ou 0.28%).

## 4.1. Mesures de performance

L'Accuracy est insuffisante. Les mesures basées sur la matrice de confusion sont privilégiées :
- **Précision** et **Rappel**.
- **F-mesure ($F_{\beta}$)** : Moyenne harmonique entre précision et rappel, souvent utilisée lorsque l'on souhaite se concentrer sur la classe minoritaire.
- **AUC ROC** : Aire sous la courbe *True Positive Rate* (TPR) contre *False Positive Rate* (FPR), offrant plus d'informations et adaptée aux problèmes de *ranking*.

## 4.2. Stratégies de traitement du déséquilibre

### 4.2.1 Échantillonnage (*Sampling*) : Modifier la distribution des classes

- **Oversampling** (Sur-échantillonnage) :
  - Idée : Générer des exemples de la classe minoritaire.
  - Méthodes clés : **SMOTE** (génération par interpolation), **Borderline SMOTE** (se concentre sur les exemples difficiles à classer à la frontière) et **ADASYN** (concentration sur les exemples les plus difficiles à classer).
- **Undersampling** (Sous-échantillonnage) :
- Idée : Supprimer intelligemment des exemples de la classe majoritaire pour nettoyer la frontière et réduire le temps de calcul.
- Méthodes clés : **Tomek Link**, **Edited Nearest Neighbor (ENN)** et **One Sided Selection (OSS)** (combinaison CNN + Tomek Link).

### 4.2.2. Apprentissage sensible aux soûts (*Cost-Sensitive Learning*) 

Idée : Modifier la fonction de perte en pondérant les erreurs. Cela est crucial pour les objectifs économiques (ex.: minimiser la perte due à la fraude fiscale). On attribue un coût plus élevé à un Faux Négatif (fraude non détectée) qu'à un Faux Positif.

# 5. Protocole expérimental et ingénierie des modèles

L'objectif est d'être formé à la résolution de problèmes réels, ce qui se traduit par la demande de **procédures complètes et justifiées**.

Un protocole expérimental doit inclure :
- **Pré-traitement** : Détection de manquants, gestion des variables catégorielles, normalisation.
- C**hoix des algorithmes et stratégies** : Utilisation de combinaisons de modèles (*bagging*, *boosting*), de méthodes d'échantillonnage, ou de méthodes hybrides (non supervisé + supervisé, ex.: K-means ou Auto-encodeurs pour la détection d'anomalies, puis SVM).
- **Validation** : Définir les ensembles d'entraînement/test (en prenant garde aux données temporelles) et le processus d'optimisation des hyper-paramètres (validation croisée $k$-fold).
- **Mesures adaptées** : Utiliser la F-mesure ou une matrice de coût personnalisée si l'objectif est économique (comme minimiser la perte due à la fraude).

La cohérence et la justification des choix sont cruciales.
