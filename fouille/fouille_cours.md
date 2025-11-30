<!--
Title: "Apprentissage automatique des données massives"
Author: rsquaredata
Last updated: 2025-11-30
-->

- [1. Fondamentaux en apprentissage machine](#1-fondamentaux-en-apprentissage-machine)
  - [1.1. Processus d'apprentissage](#11-processus-dapprentissage)
  - [1.2. Cadre d'optimisation](#12-cadre-doptimisation)
  - [1.3. Les caractéristiques du Big Data](#13-les-caractéristiques-du-big-data)
  - [1.4. Les branches du Machine Learning](#14-les-branches-du-machine-learning)
- [2. Algorithmes de classification clés](#2-algorithmes-de-classification-clés)
  - [2.1. Séparateurs à Vaste Marge (SVM)](#21-séparateurs-à-vaste-marge-svm)
    - [Méthodes à noyaux (Kernel Methods)](#méthodes-à-noyaux-kernel-methods)
    - [Limites des méthodes à noyaux](#limites-des-méthodes-à-noyaux)
  - [2.2 Arbres de décision et forêts aléatoires](#22-arbres-de-décision-et-forêts-aléatoires)
    - [Arbres de décision (CART)](#arbres-de-décision-cart)
    - [Forêts aléatoires (Random Forests)](#forêts-aléatoires-random-forests)
  - [2.3. Boosting](#23-boosting)
- [3. Apprentissage dans un contexte de données massives](#3-apprentissage-dans-un-contexte-de-données-massives)
  - [3.1. Approximation des méthodes à noyaux](#31-approximation-des-méthodes-à-noyaux)
  - [3.2. Outils Big Data pour le Deep Learning](#32-outils-big-data-pour-le-deep-learning)
- [4. Problématiques des données déséquilibrées (Imbalanced Learning)](#4-problématiques-des-données-déséquilibrées-imbalanced-learning)
  - [4.1. Mesures de performance adaptées](#41-mesures-de-performance-adaptées)
  - [4.2. Stratégies d'échantillonnage (Sampling)](#42-stratégies-déchantillonnage-sampling)
    - [Oversampling (sur-échantillonnage)](#oversampling-sur-échantillonnage)
    - [Undersampling (sous-échantillonnage)](#undersampling-sous-échantillonnage)
  - [4.3. Apprentissage sensible aux coûts (Cost-Sensitive Learning)](#43-apprentissage-sensible-aux-coûts-cost-sensitive-learning)
- [5. Théorie de la généralisation](#5-théorie-de-la-généralisation)
  - [5.1. Risques et objectif](#51-risques-et-objectif)
  - [5.2. Stabilité uniforme](#52-stabilité-uniforme)
  - [5.3. Bornes de généralisation (théorème)](#53-bornes-de-généralisation-théorème)
- [6. Outils mathématiques et concepts connexes.](#6-outils-mathématiques-et-concepts-connexes)
    - [Convexité](#convexité)
    - [Fonction lipschitzienne](#fonction-lipschitzienne)
    - [Normes](#normes)
    - [Algorithmes de résolution](#algorithmes-de-résolution)


# 1. Fondamentaux en apprentissage machine

## 1.1. Processus d'apprentissage

L'objectif est de déterminer les connaissances en *Machine Learning* (ML), notamment sur le plan pratique. Le processus d'apprentissage inclut les étapes suivantes :

- **Préparation des données** :
  - Détection des d'éventuelles **valeurs manquantes** et imoputation.
  - identification des **caractéristiques** des jeux de données (taille, dimension, type de problème, ratio des classes).
  - Mise en œuvre de **méthodes de pré-apprentissage** (pré-processing).
- **Processus d'apprentissage** :
  - Mise en œuvre de **plusieurs algorithmes**.
  - **Choix d'une mesure de performance** appropriée.
- **Évaluation** :
  - **Comparaison des performances** des algortihemes (performances et temps d'apprentissage).

  ## 1.2. Cadre d'optimisation 

L'apprentissage d'un modèle implique souvent de résoudre un problème d'optimisatin sous la forme :

$$
\min_{w \in \mathbb{R}^d} \frac{1}{m} \sum_{i=1}^m \ell(w, x_i) + \lambda f(w),
$$

avec :
- $\frac{1}{m} \sum_{i=1}^m \ell(w, x_i)$ : représente le **risque empirique** (l'erreur sur le jeu d'entraînement, mesure par la fonction de perte $\ell$).
- $f(w)$ : représente le **terme de régularisation**, qui pénalise la complexité du modèle $w$.
- $\lambda$ : est l'**hyper-paramètre de régularisation**, contrôlant l'importance de la complexité du modèle par rapport à l'erreur.

**Impact du terme de régularisation $f(w)$** :
- $f : w \mapsto \Vert w \Vert_1$ (**Régression Lasso**) : favorise un modèle **aussi parcimonieux que possible** (sparse).
- $f : w \mapsto \Vert w \Vert_2$ (**Régression Ridge**) : vise à **réduire le poids des variables**.

## 1.3. Les caractéristiques du Big Data

Les principales caractéristiques du Big Data sont souvent résumées apr les "5V" :
1. **Volume** : quantité massive (Go → To → Po)
2. **Vélocité** : flux continus, temps réel
3. **Variété** : structure, semi-structure, non structuré
4. **Véracité** : qualité, bruit, incertitude
5. **Valeur** : utilité business

L'engouement pour le Big Data s'explique par le développement constant de la capacité de stockage des données par les entreprises.

## 1.4. Les branches du Machine Learning

Les trois grandes branches de l'apprentissage machine sont :
1. **Apprentissage supervisé** (ex.: classification, régression)
2. **Apprentissage non supervisé** (ex.: clustering, détection d'anomalies)
3. **Apprentissage par renforcement**

# 2. Algorithmes de classification clés

## 2.1. Séparateurs à Vaste Marge (SVM)

Le SVM est un algorithme uissant pour la classification binaire.

| Concept | Définition / Formulation |
|---------|--------------------------|
| **Objectif principal** | Apprendre un hyperplan $h(x) = sign[\langle w, x \rangle + b]$ qui **maximise la marge** $\gamma$. |
| **Marge $\gamma$** | Distance entre les deux hyperplans supports, définie par $\gamma = \frac{2}{\vert w \vert_2}$. Maximiser $\gamma$ revient à minimiser $\frac{1}{2 \lvert w \rvert_2^2}$. |
| **Hard Margin SVM** | Suppose les données linéairement séparables (cas idyllique).</br>$\min_{(w,b)} \in \mathbb{R}^{d+1} \frac{1}{2} \text{s.t.} y_i (\langle w, x_i \rangle + b) \ge 1$. |
| **Soft Margin SVM** | Introduit les variables slacks $\xi \ge 0$ pour tolérer les erreurs (points à l'intérieur de la marge ou mal classés).</br>$\min_{\xi \in \mathbb{R}^m, (w,b) \in \mathbb{R}^{d+1}} \frac{1}{2} \lvert w \rvert_2^2 + \frac{C}{m} \sum_{i=1}^m \xi_i \text{s.t.} y_i(\langle w, x_i \rangle + b) \ge 1 - \xi_i.$ |
| **Hinge Loss** | Formulation équivalente du Soft Margin SVM :</br>$\min{(w,b) \mathbb{R}^{d+1} \frac{1}{2} \vert w \vert_2^2 + \frac{C}{m} \sum_{i=1}^m [1 - y_i(\langle w, x_i \rangle + b)]_+}$. |
| **Vecteurs supports** | Les exemples qui participent à la construction de la frontière de décision sont ceux pour lesquels $\alpha_i \neq 0$ dans la formulation duale. |

### Méthodes à noyaux (Kernel Methods)

Les méthodes à noyaux permettent de projeter implicitement les données dans un espace latent $Z$, potentiellement de dimension infinie, où la tâche peut être résolue linéairement.

- **Astuce du noyau (KernelTrick)** : basée sur le théorème de Mercer, elle permet de définir une fonction $K(x, x') = \langle \phi(x), \phi(x') \rangle_Z$ sans exliciter la transformation $\phi$.
- **Exemples de noyaux** :
  - Linéaire : $K(x,x') = \langle x, x' \rangle$.
  - Polynomial : $K(x, x') = (\langle x, x' \rangle + c)^p$.
  - Gaussien (radial Basis Function - RBF) : $K(x, x') = \exp\left(- \frac{\Vert x-x' \Vert_2^2}{2 \sigma^2} \right)$. Le paramètre $\sigma$ (ou $\sigma = 1/\sigma^2$) contrôle le lissage de la frontière de décision.
  - **Règle de prédiction (version kernelisée)** : $h(x') = sign\left( \sum_{i=1}^m \alpha_i y_i K(x', x_i)\right)$.

### Limites des méthodes à noyaux

Les méthodesà. noyaux sont très limitées dans un contexte de **données massives**.
- Le calcul nécessite $m^2$ similarités (où $m$ est le nombre d'exemples).
- Le modèle appris comporte $m$ paramètres.
- L'apprentissage requiert souvent l'inversion d'une matrice $K \in \mathbb{R}^{m \times m}$, ce qui n'est pas réalisable en Big Data.

## 2.2 Arbres de décision et forêts aléatoires

### Arbres de décision (CART)
L'objectif est de séparer les données en groupes pour un modèle plus puissant et stable. Les critères de séparation (pour la classification) incluent l'indie de **Gini** et l'**entropie**.

### Forêts aléatoires (Random Forests)
Combinaison de plusieurs arbres pour un modèle plus puissant et stable. Elles utilisent un **double échantillonnage** : sur les données et sur les variables. La combinaison des modèmes se fait par des règles de vote.

## 2.3. Boosting

Le bossing vise à combiner plusieurs **modèles qui ont un faible pouvoir prédictif** (*weak learners*) afin de créer un modèle fort, robuste et stable.

| Algorithme | Caractéristiques clés |
|------------|-----------------------|
| **Adaboost** | Apprend les hypothèses $h_t$ de manière itérative, en **repondérant les exemples** à chaque étape pour se focaliser sur ceux qui étaient mal classés. Basés sur l'*exponential loss*. |
| **Gradient Boosting** | Généralisation du boosting. Vu comme un algorithme de **descente de gradient dans un espace fonctionnel**. Apprend les apprenants faibles $h_t$ sur les **pseudo-résidus** $r_i$ (qui sont le gradient négatif ${\tilde{y}{i}}$ de la fonction de loss $\ell$ par rapport à la prédiction courante ${H_{t-1}(xi)}$ ). |
| **XGBoost** | Algorithme de *Gradient Tree Boosting* très efficace. Utilise une **approximation d'ordre 2** de la fonction de loss. Permet l'optimisation de n'importe quelle loss et est très rapide. |

# 3. Apprentissage dans un contexte de données massives

## 3.1. Approximation des méthodes à noyaux

Pour pallier la complexité $O(m^2)$ des méthodes à noyaux classqiues sur des données volumineuses, des méthodes d'approximation sont nécessaires.

- **Landmarks (Points de repères)** : L'idée est de sélectionner $k \lt m$ points de repère (aléatoirement ou via un algorithme comme *k-means*) et d'apprendre le modèle uniquement à partir des similarités avec ces points. Cela réduit le nombre de similarités à calculer.
  - La prédiction se fait en calculant la similarité du nouveau point $x$ avec les *landmarks* $x_k : f(x) = \sum_{k=1}^l \alpha_k y_k(x, x_k)$.
- **Random Fourier Features (RFF)** : Méthode d'approximation pour les noyaux. invariants par translation (comme le noyau gaussien).
  - L'objectif est d'approximer le produit interne du noyau par une projection aléatoire $u: f(x, x') \approx \langle u(x), u(x') \rangle$.
  - Basé sur le théorème de Bochner, le noyau gaussien peut être approximé par une **somme de fonctions cosinus**.
  - La projection $u(x)$ permet d'apprendre un séparateur linéaire sur les données projetées, ce qui est beaucoup plus rapide.

## 3.2. Outils Big Data pour le Deep Learning

Des plateformes et librairis sont utilisées pour contourner les barrières techniques liées au volume de donées e distribuer les calculs (Hadoop, Spark).

- **H2O** : Plateforme de ML générale, permettant l'exécution d'algorithmes (Deep Learning Gradient Boosting, K-means) sur plusieurs cœurs/processeur ou clusters/serveurs. Disponible via une interface web ou des packages sous R/Python.
- **Keras** : Librairie *open source* Python spécifiquement dédiées à l'implémentation facile des algorithmes de Deep Learning.
- **Réseaux de Neurones Convolutionnels (CNN)** : Type de réseaux nécessaire pour la classification ou la segmentation sémantique d'images. Ils utilisent des couches de convolution et de *max-pooling* pour l'extraction de *features*.
- *Composantes d'un Réseau de Neurones : Couches (cachées), neurones, fonctions d'activation, poids (paramètres). La mise à jour des poids se fait via un processus *Forward-Bawckward* appelé **rétropropagation du gradient** (*back-propagation*).

# 4. Problématiques des données déséquilibrées (Imbalanced Learning)

Le contexte déséquilibré se produi lorsque le nombre d'exemples d'une clase (minoritaire) est très faible par rapport aux autres classes (majoritaire). Dans ce cas, les algorithmes stadnards ont tendance à favoriser la classe majoritaire. lLes problèmes typiques sont la détection de **fraudes** ou d'**anomalies**.

## 4.1. Mesures de performance adaptées

Minimiser le taux d'erreur n'est pas une bonne solution. On utilise xes mesures basées sur la **matrice de confusion** :

| Mesure | Formule (pour la classe minoritaire/positive) | Utilité |
|--------|-----------------------------------------------|---------|
| **Précision** | $\frac{TP}{TP+FP}$ | Évalue la justesse du modèle dans ses prédictions positives. |
| **Rappel (Recall)** | $\frac{TP}{TP+FN}$ | Évalue la capacité du modèle à retrouver des exemples de classe minoritaire. |
| **F-mesure (F_{\beta})** | $$\frac{(1+\beta^2) \times \text{Précision} \times \text{Rappel}}{\beta^2 \times \text{Précison} + \text{Rappel}}$$ (souvent $\beta = 1$) | Moyenne harmonique entre Précision et Rappel. |
| ** AUC ROC** | Aire sous la courbe (TPR vs FPR). | Mesure plus adaptée aux problématiques de *ranking* et offre plus d'informations sur la qualité du modèle. |

## 4.2. Stratégies d'échantillonnage (Sampling)

### Oversampling (sur-échantillonnage)

Augmente le nombre d'exemples de la classe minoritaire pour équilibrer le jeu de donénes.

- **Aléatoire** : Duplication de points existans de la classe minoritaire (n'ajoute pas d'information).
- **SMOTE** : Génère de nouveaux exemples synthétiques en interpolant entre un exemple minoritaire $p_i$ et l'un de ses voisins $rNN_i$.
- **Borderline SMOTE** : Concentre la génération sur les exemples *borderline* (ceux dont la majorité des voisins appartiennent à la classe majoritaire).
- **ADASYN** : Version qui concentre la génération sur les exemples **plus difficiles à classer** (score de risque $r_i$ plus élevé).

### Undersampling (sous-échantillonnage)

Supprime aléatoirement ou intelligemment des exemples de la classe majoritaire.

- **Aléatoire** : Suppression aléatoire (risque de perdre de l'information importante).
- **Tomek Link** : Identifie les paires d'exemples d'étiquettes différentes qui sont plus proches voisins l'un de l'aure (situés à la frontière de décision) et les retire (souvent uniquement l'exemple majoritaire).
- **Edited Nearest Neighbor (ENN)** : Supprime un exemple de la classe majoritaire si sa prédiction (basée sur ses $k$ plus proches voisins) ne coïncide pas avec son vrai label.
- **One Sided Selection (OSS)** : Combine Condensed Nearest Neighbor (CNN) et Tomek Link pour réduire la taille de l'ensemble de données et le temps de calcul.

## 4.3. Apprentissage sensible aux coûts (Cost-Sensitive Learning)

Modifie la façon dont les erreurs sont pondérées par l'algorithme.

- On définit une **matrice de coûts** (similaire à la matrice de confusion).
- L'erreur de mal classer un exemple minoritaire ($c_{FN}$) doit avoir un coût plus élevé que mal classer un exemple majoritaire ($c_{FP}$).
- Implémentation via le paramètre `class_weight` des implémentations ML standard.
- Dans un contexte économique (fraude), on peut attribuer un poids plus important aux fraudes de montant élevé pour **minimiser les pertes**.

# 5. Théorie de la généralisation

## 5.1. Risques et objectif

- **Risque empirique ($R_{\ell}^S(h)$)** : Erreur moyenne sur l'échantillon d'entraînement $S: R_{\ell}^S(h) = \frac{1}{m} \sum_{i=1}^m \ell(h(x_i), y_i)$.
- **Risque réel ($R_{\ell}$)** : Espérance de l'erreur sur la distribution inconnue $D: R_{\ell} = \mathbb{E}_{(x,y)~D} [\ell\left(h(x),y\right)]$.
- **Objectif** : Borner l'écart entre le risque réel et le risque empirique : $\lvert R_{\ell} - R_{\ell}^S \le \varepsilon(\delta, m)$.

## 5.2. Stabilité uniforme

Cette approche théorique ne repose pas sur une mesure de compelxité de l'epace d'hypothèse.

**Définition** : Un algorithme $A$ est **uniformément stable** avec une constante $\beta \gt 0$ si la modificatin d'un seul exemple dans l'ensemble d'entraînement $S$ n'entraîne qu'une faible modification de la valeur de la *loss* pour toute donnée $x$ :

$$
\forall S, \forall i, 1 \le i \le m, \sup_x \lvert \ell(\theta_S, x) - \ell(\theta_{S^i}, x) \rvert \le \beta.
$$

Pour le SVM, la constante de stabilité uniforme $\beta$ est donnée par : $\beta = \frac{2 \sigma^2}{\lambda m}$, où $\sigma$ dépend de la losse et $\lambda$ est le paramètre de régularisation.

## 5.3. Bornes de généralisation (théorème)

En utilisant la convexité de la fonction de perte $\ell$ et l'inégalité de McDiarmid, on établit le résultat suivant (avec une probabilité au moins $1-\delta$) :

$$
R_{\ell}(\theta_S) \le R_{\ell}^S(\theta_S) + 2 \beta + (4m\beta + K) \sqrt{\frac{\ln{(1/\delta)}}{2m}}.
$$

**Interprétation (pour le SVM)** :
- La borne diminue avec l'augmentation du nombre d'exemples $m$.
- La borne diminue avec l'augmentation de l'hyper-paramètre $\lambda$ (qui est inversement lié à $C$). Si $\lambda$ augmente, le terme de régularisation prend plus d'importance, réduisant la complexité du modèle et améliorant la généralisation.

# 6. Outils mathématiques et concepts connexes.

### Convexité
Une fonction $f$ est convexe si le segment joignant deux points de son graphe est au-dessus du graphe. La fonction objectif d'un SVM est convexe.

### Fonction lipschitzienne
Une fonction $f$ est dite lipschitzienne s'il existe une constante $C \gt 0$ telle que la variation de la fonction est bornée par la distance entre les entrés multipliée par $C : \Vert f(x) - f(x')\Vert \le C \Vert x-x' \Vert$. Une loss $\ell$ peut être $\sigma$-admissible si elle est convexe et lipschitzienne par rapport à son premier argument.

### Normes
- $L_1$ (Manhattan) : $\sum \vert x_j \vert$
- $L_2$ (euclidienne) : $\sqrt{\sum x-J^2}$
- $L_{\infty}$ (Maximum) : \max \vert x_j \vert
- $L_p$

### Algorithmes de résolution
L'implémentation d'algorithmes peut nécessiter des solveurs d'optimisation comme `scipy.optimize.fmin_l_bfgs_b` en Python.
