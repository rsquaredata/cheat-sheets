<!--
Title: "Big Data Mining - Fiches - Boosting"
Author: rsquaredata
Last updated: 2025-12-09
-->

# Boosting

## 1. Définition générale du Boosting

Le **Boosting** est une famille de méthodes d'apprentissage supervisé qui construit un **modèle fort** en combinant plusieurs **modèles faibles** (appelés *weak learners*), entraînés **séquentiellement**, chacun corrigeant les erreurs du précédent.  

Idée clé :
> On apprend un modèle difficile en empilant des modèles très simples en série.

## 2. Principe général du Boosting

1. On initialise un modèle faible (par exemple, un stump = arbre de décision de profondeur 1).
2. On identifie les erreurs commises.
3. On entraîne un nouveau modèle **en insistant** sur les modèles mal prédits.
4. On combine les modèles (moyenne pondérée).
5. On répète le processus T fois.

## 3. AdaBoost (Boosting classique)

### 3.1. Idée centrale

AdaBoost modifie les **poids des observations** :  
- Les observations mal classées → poids augmenté
- Les observations bien classées → poids diminué

Ainsi, chaque weak learner se concentre de plus en plus sur les cas difficiles.

### 3.2. Formule clé

Poids mis à jour : $w_i^{t+1} e^{\alpha_t I(y_i \neq h_t (x_i)}$,  
où  
- $h_t$ = faible classifieur $t$
- $\alpha_i$ = importance du modèle $t$
- $I$ = indicatruce d'erreur

### 3.3. Modèle final

$$
H(x) = \text{sign} \left( \sum_{t=1}^T \alpha_t h_t (x) \right)
$$

### 3.4. Points clés attendus

- met l'accent sur les erreurs via un **recentrage des poids**,
- utilise principalemen des **arbres très peu profonds**,
- tend à **réduire le biais** meut peut sur-apprendre si $T$ est énorme.

## 4. Gradient Boosting

### 4.1. Intuition

Contrairement à AdaBoost, il **ne modifie pas les poids des données**.  
Il construit un modèle en effectuant une **descente de gradient fonctionnelle** :  
> À chaque itération, on entraîne un nouveau modèle faible pour approximer **le gradient négatif** de la loss.

### 4.2. Étapes

1. Choisir une loss (ex.: square losse, log-loss).
2. Calculer le résidu (pseudo-résidu) : $r_i^{(t)} = - \frac{\partial L(y_i, F(x_i))}{\partial F(x_i}$.
3. Entraîner un arbre faible sur les résidus.
4. Mettre àa jour : $F_t (x) = F_{t-1} (x) + \eta h_t (x)x$, où $\eta$ est le learning rate.

### 4.3. Points clés

- plus flexible que AdaBoost (on choisit la loss),
- **plus stable** est moins sensible au bruit,
- base de XGBoost, LightGBM, CatBoost.

## 5. Différences AdaBoost vs Gradient Boosting

| Aspect | Adaboost | Gradient Boosting |
|--------|----------|-------------------|
| Principe | Réajustement des **poids des observations** | descente de gradient sur la **los globale** |
| Ce qu'on corrige | Les erreurs de classification | Le gradient de la fonction de perte |
| Modèles faibles | Stumps très simples | Arbres faibles + beaucoup de paramètres |
| Sensibilité au bruit  | Très sensible | Beaucoup plus robuste |
| Type de loss | Hinge-like | Loss choisie (square, log, MAE, etc.) |
| Interprétation | Réduction du biais | Approximations successives du gradient |

> Adaboost s'appuie sur un réajustement du poids des observations, alors que Gradient Boosting fait une descente de gradient fonctionnelle sur la loss.

## 6. Résultat théorique d'AdaBoost

Borne : $R_< \le \exp{(-2 \gamma^2 T)}$, où  
- $R_S$ = risque empirique,
- $T$ = nomber d'itérations,
- $\gamma$ = marge minimale.

**Interprétation** :  
- Quand $$T \to \infty$, le risque empirique décroît **exponentiellement** et tend vers 0.
- En pratique :
  - *o*on ne peut pas envoyer $T$ à l'infini** → overfitting, coûts démesurés, bruit amplifié.
  - on régule avec la profondeur, learning rate, early stopping.

> Théoriquement, le risque empirique décroît exponentiellement avec T. En pratique, augmenter T indéfiniment mène au sur-apprentissage et à un coût prohibitif.

## 7. POurquoi le Boosting marche aussi bien ?

1. Réduction du **biais**.
2. Mise à jour séquentielle → apprend progressivement les cas difficiles.
3. Flexibilité (surtout gradient boosting).
4. Très bonne performance hors boîte (XGBoost, LightGBM).

## 8. Avantages et limites

**Avantages** :  
- très performant sans tuning extrême
- réduit le biais
- gère bien les interactions non linéaires (via arbres faibles)
- excellent avec features catégorielles (CatBoost)

**Limites** :  
- coût computationnel important
- surapprentissage possible si trop d'itérations
- sensibilité au bruit (surtout AdaBoost)  
