<!--
Title: "Procédures d'apprentissage - Entraînement et tuning - Séparation Training/Validation/Test"
Author: rsquaredata
Last updated: 2025-12-03
-->

# Séparation des ensembles : Généralisation et Validation croisée

L'étape de **séparation Training/Validation/Test** est l'un des piliers méthodologiques de la **procédure 
d'apprentissage** et du **tuning** d'un modèle en *Machine Learning*. Cette division des données est essentielle pour 
garantir l'évaluation sans biais de la capacité de **généralisation** du modèle.

## 1. Objectifs de la séparation des ensembles

Le but principal de cette séparation est de s'assurer que l'efficacité du modèle est mesurée sur des données qu'il n'a 
jamais rencontrées pendant la phase d'apprentissage.

### 1.1.  L'ensemble d'entraînement (*Training Set*)

L'ensemble d'entraînement est utilisé pour **apprendre les paramètres du modèle** (par exemple, le paramètre $w$ d'un 
séparateur linéaire ou les poids d'un réseau de neurones).

- L'algorithme a accès aux descripteurs ($x_i) et aux étiquettes ($y_i$) pour être entraîné.
- Il est crucial de conserver une grande quantité de données pour cet ensemble (généralement **environ 2/3 des 
exemples*) afin de permettre au modèle de capturer toutes les spécificités de la distribution sous-jacente des données.

### 1.2. L'ensemble de test (*Test Set*)

L'ensemble de test est utilisé **uniquement** pour **évaluer le modèle** une fois qu'il a été appris.

- L'évaluation de la performance finale du modèle, effectuée *a posteriori*, repose sur cet ensemble.
- Il est important que l'ensemble de test contienne suffisamment d'exemples (environ 1/3 de l'échantillon total) pour 
que **les résultats observés soient statistiquement significatifs**.
- Dans le cadre des projets, l'ensemble de test est souvent **fixé** et donné par l'énoncé, et il faut s'assurer que 
le modèle final y obtient les meilleures performances, par exemple en termes de **F-mesure**.

## 2. Le rôle spécifique de l'ensemble de validation

La procédure d'apprentissage doit également gérer l'optimisation des **hyperparamètres** (comme $C$ ou $\gamma$ pour 
les SVM, ou $\lambda$ pour la régularisation). Pour cela, un ensemble de validation est nécessaire, qui est dérivé de 
l'ensemble d'entraînement.

### 2.1. Validation simple

Une méthode de *tuning* consiste à imiter la procédure d'évaluation en divisant l'ensemble d'entraînement en deux 
parties :
1. **Ensemble d'apprentissage (_Learning Set_)** : Utilisé pour entraîner le modèle avec une valeur spécifique de 
l'hyperparamètre $\lambda_i$.
2. **Ensemble de validation (_Validation Set_)** : Utilisé pour tester la performance ($p_i$) du modèle entraîné avec 
ce $\lambda_i$. C'est une sorte de "test set temporaire" pour le tuning.
3. **Choix optimal** : On retient l'hyperparamètre $\lambda_{i_{\max}}$ associé à la performance la plus élevée 
($p_{i_{\max}}$) sur cet ensemble.
4. Une fois $\lambda_{i_{\max}}$ trouvé, le modèle final est entraîné à nouveau en utilisant la totalité de l'ensemble 
d'entraînement (apprentissage + validation) avec cette valeur optimale.

Cette méthode, cependant, peut avoir une faille si l'ensemble de validation est un **sous-ensemble trop petit** et ne 
représente qu'une petite partie de la distribution des données.

### 2.2. Validation croisée ($k$-fold Cross-Validation)

Pour pallier les limites de la validation simple, la procédure de **validation croisée ($k$-fold CV)** est la méthode 
privilégiée pour le tuning des hyperparamètres..

- **Principe** : L'ensemble d'entraînement est séparé en $k$ groupes ou plis (*folds*) (par exemple, **5-CV** est 
utilisé dans les travaux sur les SVM).

- **Itérations** : Pour chaque valeur d'hyperparamètre $\lambda_i$ testée, on apprend le modèle $k$ fois, en utilisant 
à chaque tour $k−1$ plis pour l'apprentissage et le pli restant comme ensemble de validation.

- **Score moyen** : Le score $s_i$ de $\lambda_i$ est la moyenne des $k$ performances mesurées sur les ensembles de 
validation.

- **Avantage** : Cette méthode permet de parcourir la **totalité de l'ensemble d'entraînement** pour la validation, 
accédant ainsi à une plus grande partie de la distribution sous-jacente, ce qui conduit à de meilleurs résultats 
pratiques.

## 3. Contraintes spécifiques (données massives et temporelles)

Le contexte de la fouille de données massives introduit des contraintes particulières concernant la séparation des 
ensembles.

- **Séparation temporelle** : Pour les données séquentielles, comme les transactions bancaires dans un projet de 
détection de fraude, la séparation doit être faite selon la date. Par exemple, les transactions entre le 2017-02-01 et 
le 2017-08-31 constituent l'ensemble d'**apprentissage**, tandis que celles du 2017-09-01 au 2017-11-30 forment 
l'ensemble de **test**.

- **Interdiction de la CV aléatoire** : Dans ce contexte, il est crucial de **ne pas _cross-valider_ les modèles 
n'importe comment** en mélangeant aléatoirement les données, car cela violerait la structure temporelle.

---

En résumé, la séparation *Training/Validation/Test* est un protocole qui assure que :
1. Les paramètres du modèle sont appris sur une grande portion des données (Training).
2. Les hyperparamètres sont optimisés de manière rigoureuse (Validation, idéalement par CV).
3. La performance finale est mesurée sur un jeu de données indépendant et non rencontré (Test) pour évaluer la 
généralisation.

