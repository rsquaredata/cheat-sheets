<!--
Title: "Apprentissage massif et données déséquilibrées - Concepts fondamentaux de ML"
Author: rsquaredata
Last updated: 2025-11-30
-->

# 1. Définition et branches de l'apprentissage machine

Le Machine Learning est un sous-domaine de l'Intelligence Artificielle qui vise à élaborer et à étudier des systèmes capables d'apprendre et de faire des prédictions à partir de données.

Les sources distinguent trois grandes branches principales de l'apprentissage machine :
1. **Apprentissage supervisé (_Supervised Learning_)** : Comprend les tâches de classification et de régression. L'élément clé est l'accès à des données étiquetées. Des exemples incluent la détection de fraude et l'analyse de séries temporelles.
2. **Apprentissage non supervisé (_Unsupervised Learning_)** : Regroupe les tâches de *clustering* ou de réduction de dimensionnalité. Le *clustering* (segmentation) est notamment utilisé en combinaison avec d'autres algorithmes.
3. **Apprentissage par renforcement (_Reinforcement Learning_)** : Axé sur les décisions en temps réel et l'IA dans les jeux.

# 2. Le Processus d'apprentissage et la préparation des données

Il est crucial de maîtriser le processus d’apprentissage d’un modèle et d'avoir les "réflexes" à adopter face aux caractéristiques des données. Ce processus commence bien avant le choix de l'algorithme.

## 2.1. Identification et pré-traitement

La première étape fondamentale est l'étude des jeux de données, incluant :
- La détection d'éventuelles **valeurs manquantes** et leur imputation.
- L'identification des **caractéristiques** des jeux de données (taille, dimension, type de problème, **ratio des classes**).
- La mise en œuvre de méthodes de **pré-apprentissage** (*pre-processing*), comme la normalisation ou la mise à l'échelle des données, ce qui est particulièrement important pour les algorithmes basés sur la notion de distance ou de produit interne (comme les SVM ou les $k$-NN).

## 2.2. Optimisation et Hyper-paramètres

L'apprentissage d'un modèle est souvent formulé comme un problème d'optimisation. La forme générale minimisée est :

$$
\min_{w \in \mathbb{R}^d} \frac{1}{m} \sum_{i=1}^m \ell(w, x_i) + \lambda f(w)
$$

- Le terme $\frac{1}{m} \sum_{i=1}^m \ell(w, x_i)$ est le **risque empirique** (l'erreur sur le jeu d'entraînement, mesurée par la fonction de perte $\ell$).
- Le terme $\lambda f(w)$ est le terme de **régularisation**. L'hyper-paramètre $\lambda$ contrôle l'importance accordée à la complexité du modèle $w$ par rapport aux erreurs commises.
- Le choix de la fonction $f$ impacte le modèle : $f: < \mapsto \Vert w \Vert_1$ (norme $L_1$) conduit à un modèle **parcimonieux** (sparse), tandis que $f : w \mapsto \Vert w \Vert_2$ (norme $L_2$) vise à r**éduire le poids des variables**.

Le processus d’apprentissage doit inclure l'étape de réglage (*tuning*) des hyper-paramètres (comme $C$ et $\gamma$ pour le SVM, ou $\lambda$) en utilisant la validation croisée (*k-fold cross-validation*, souvent 5-CV).

## 2.3. Mesures de performance dans le contexte de données massives

Dans le contexte de problèmes de **données déséquilibrées** (comme la détection de fraude où seulement 1% ou 0.28% des transactions sont frauduleuses), les mesures de performance classiques doivent être remises en question.

- **Taux d'erreur (_Accuracy_)** : Il tend à **favoriser la classe majoritaire** et n'est donc pas une bonne solution dans un contexte déséquilibré.
- **Mesures adaptées** : Il est importan d'évaluer le modèle en se concentrant sur la classe minoritaire à l'aide de mesures dérivées de la matrice de confusion.
  - **Précision** (justesse des prédictions positives) et **Rappel** (*Recall*, capacité à retrouver les exemples minoritaires).
  - **F-mesure ($F_{\beta}$)** : Moyenne harmonique de la Précision et du Rappel, souvent utilisée pour se concentrer sur le comportement sur la classe minoritaire.
  - **AUC ROC** : L'aire sous la courbe (TPR vs FPR) qui offre **plus d’informations sur la qualité du modèle** et est mieux adaptée aux problématiques de *ranking*.

De plus, l'évaluation dans un contexte de données massives ne doit pas se limiter aux performances, mais doit impérativement inclure le temps d’apprentissage (rapidité). Les modèles doivent s'apprendre en un temps raisonnable et souvent répondre rapidement (par exemple, environ 20 ms dans le contexte de détection de fraudes par chèque).
