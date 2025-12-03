<!--
Title: "Apprentissage massif et données déséquilibrées - Procédures d'apprentissage"
Author: rsquaredata
Last updated: 2025-12-03
-->

# Procédures d'apprentissage à l'échelle massive

La **procédure d'apprentissage** est l'ensemble des étapes méthodologiques et pratiques essentielles à la construction, 
à l'évaluation et à l'optimisation des modèles de Machine Learning (ML), en particulier dans le contexte des 
**données massives**.

La procédure d'apprentissage est une procédure rigoureuse qui intègre la préparation des données, le réglage des 
hyperparamètres, le choix d'algorithmes adaptés aux contraintes d'échelle et de déséquilibre, et une évaluation 
complète des performances.

## 1. Étude et préparation des données

La procédure d'apprentissage commence toujours par une phase d'étude et de préparation des données.

- **Analyse des caractéristiques** : Il est nécessaire d'identifier les caractéristiques des jeux de données, 
notamment leur **taille** ($m$ ou nombre d'exemples), leur **dimension** ($d$ ou nombre de descripteurs), le type de 
problème (classification ou régression), et surtout le **ratio des classes**.
- **Nettoyage et imputation** : Les étapes initiales incluent la détection d'éventuelles **valeurs manquantes** et leur 
**imputation**.
- **Séparation des ensembles** : Pour pouvoir évaluer le modèle sur des données non rencontrées, le jeu de données est 
séparé en plusieurs sous-ensembles.
	- **Ensemble d'entraînement (_Training Set_)** : Utilisé pour apprendre les paramètres du modèle.
	- **Ensemble de test (_Test Set_)** : Utilisé *a posteriori* pour vérifier si la prédiction du modèle est correcte 
et évaluer sa performance finale sur des données nouvelles.
	- Généralement, on utilise **environ les 2/3 des exemples pour l'entraînement** et le tiers restant pour le test.
	- Dans certains contextes (comme le projet de détection de fraude), la séparation doit tenir compte de la 
temporalité des données ; par exemple, les transactions ayant eu lieu entre le 2017-02-01 et le 2017-08-31 sont 
utilisées pour l'apprentissage, et celles du 2017-09-01 au 2017-11-30 sont utilisées pour le test.

## 2. Réglage des hyperparamètres et entraînement

Une fois les données préparées, le processus d'apprentissage vise à apprendre l'hypothèse $h$ de manière optimale.

### 2.1. Le tuning des hyperparamètres

La plupart des modèles (comme les SVM avec $C$ ou les réseaux de neurones) dépendent d'**hyperparamètres** 
($\lambda$ ou $C$ pour les SVM, $\gamma$ pour le noyau gaussien) dont la valeur optimale doit être déterminée.

- **Validation croisée ($k$-fold CV)** : C'est la méthodologie de réglage privilégiée pour *tuner* les hyper-paramètres.
- **Processus $k$-CV$ : L'ensemble d'entraînement est divisé en $k$ plis (*k-folds*). Pour chaque valeur 
d'hyperparamètre $\lambda_i$, on entraîne le modèle sur $k−1$ plis (ensemble d'apprentissage) et on teste sa 
performance sur le pli restant (ensemble de validation). La performance moyenne $s_i$ est calculée sur les $k$ tours, 
et la valeur de $\lambda$ qui donne le score le plus élevé ($i_{\max}) est retenue pour l'entraînement final sur 
l'ensemble d'entraînement complet.

### 2.2. Optimisation algorithmique

L'apprentissage implique l'utilisation de solveurs pour les problèmes de minimisation du risque régularisé. Le travail 
pratique implique d'utiliser des algorithmes et des outils spécifiques :

- **Solveurs d'optimisation** : Des solveurs avancés comme `optimize.fmin_l_bfgs_b` de la librairie `scipy` (méthode 
Quasi-Newton BFGS) sont utilisés pour résoudre les problèmes d'optimisation.

- **Réseaux de neurones / Deep Learning** : L'implémentation de réseaux de neurones (Deep Learning) se fait à l'aide 
de librairies comme `h2o` (qui permet l'exécution dans des environnements distribués) ou `Keras`.

## 3. Gestion des contraintes du Big Data

La fouille de données massives impose des contraintes de temps d'apprentissage et de scalabilité. La procédure 
d'apprentissage doit donc privilégier des méthodes rapides :

- **Limites des méthodes à noyaux** : Les expériences montrent que les méthodes à noyaux (comme le SVM non linéaire) 
sont **très limitées lorsque le nombre de données est considérable**. Le calcul des $m^2$ similarités et l'inversion 
de la matrice $K \in \mathbb{R}^(m \times m} ne sont pas réalisables en Big Data.

- **Techniques d'approximation** : Pour rendre les noyaux scalables, la procédure intègre des méthodes d'approximation :

	- **Landmarks (Points de repère)** : Consiste à sélectionner un nombre fixe $k \lt m$ de points de repère (choisis 
aléatoirement ou via $k$-means) pour réduire le nombre de similarités à calculer.

	- **Random Fourier Features (RFF)** : Une approche qui approxime le produit interne du noyau par une projection 
aléatoire $u:\mathbb{R}^d \to \mathbb{R}^p$ dans un espace de faible dimension $p$, permettant d'apprendre un 
séparateur linéaire (plus rapide) sur les données projetées.

- **Algorithmes scalables** : Des algorithmes comme le **Gradient Boosting** (par exemple, XGBoost) sont favorisés car 
ils sont rapides, performants et facilement parallélisables, malgré leur complexité.

## 4. Mesures de performance et stratégies pour le déséquilibre

Le choix des mesures et la gestion du déséquilibre sont des étapes critiques de la procédure d'apprentissage, 
notamment dans les contextes de détection de fraude.

### 4.1. Choix des mesures

Chercher à minimiser le taux d'erreur (*Accuracy*) n'est **pas une bonne solution** dans un contexte déséquilibré, car 
cela favorise la classe majoritaire. On utilise des mesures plus pertinentes :

- **Précisipn** et **Rappel** (ou *Recall*).
- **F-mesure ($F_{\beta}$** : C'est la moyenne harmonique de la précision et du rappel. Pour le projet de détection de 
fraude, l'objectif est d'obtenir le meilleur résultat en terme de **F-mesure**.
- **AUC ROC** : L'aire sous la courbe ROC, adaptée aux problématiques de classement (*ranking*).

### 4.2. Gestion du déséquilibre

Pour pallier le déséquilibre des classes, la procédure d'apprentissage doit impliquer des stratégies de pré-traitement :

1. **Stratégies d'échantillonnage (_Sampling_)**

	- **Oversampling** (sur-échantillonnage) de la classe minoritaire (exemples : SMOTE, ADASYN). L'*oversampling* aléatoire 
duplique des points existants, concentrant l'algorithme sur ces zones.

	- **Undersampling** (sous-échantillonnage) de la classe majoritaire (exemples : Tomek Link, Edited Nearest Neighbor, 
Condensed Nearest Neighbor).

2. **Apprentissage sensible aux coûts (_Cost-Sensitive Learning_)** : Modification de la fonction de perte pour
attribuer un **poids plus important** ($c_{FN}) aux erreurs sur la classe minoritaire (faux négatifs). Ceci est 
particulièrement pertinent pour **minimiser les pertes monétaires** (comme dans le cas des fraudes bancaires ou 
fiscales).

## 5. Protocole expérimental et clarté du rapport

Il est nécessaire de documenter l'ensemble du processus dans un rapport structuré, souvent en réponse à une *mise en
situation*.

- **Méthodologie** : Présenter les notations, les outils utilisés et la **justification des approches**.

- **Créativité et combinaison** : Il s'agit d'être **créatif et de **combiner plusieurs modèles** ensemble (par 
exemple, SVM + échantillonnage, ou méthodes non supervisées comme $k$-means ou auto-encodeurs avec des algorithmes 
supervisés) pour améliorer les performances et la stabilité.

- **Reproductibilité** : Le protocole doit être suffisamment détaillé pour que le lecteur puisse **reproduire les 
résultats*, incluant les gammes d'hyperparamètres et la méthode d'optimisation employée (ex : *cross-validation en 
$k$-folds*).

- **Analyse des résultats** : L'évaluation doit comparer non seulement les performances, mais aussi le **temps 
d'apprentissage des algorithmes.

- **Cohérence** : Il est crucial de **motiver le choix des algorithmes** et d'expliquer pourquoi ils sont cohérents à 
comparer entre eux. Si une méthode ne fonctionne pas (par exemple, un noyau inadapté au Big Data), il faut l'indiquer 
et justifier pourquoi.










































