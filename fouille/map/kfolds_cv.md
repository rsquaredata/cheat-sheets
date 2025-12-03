<!--
Title: "Procédures d'apprentissage - Entraînement et tuning - Validation croisée (K-folds CV)"
Author: rsquaredata
Last updated: 2025-12-03
-->

# Validation croisée $k$-folds pour l'optimisation des hyperparamètres

La **validation croisée ($k$-folds CV)** est une procédure fondamentale dans la phase d'**entraînement et de tuning** 
des modèles d'apprentissage automatique, pour assurer la généralisation des modèles appris. Elle sert principalement à 
**optimiser la valeur des hyperparamètres** du modèle.

## 1. Rôle de la validation croisée dans le tuning

Le tuning des hyperparamètres (tels que $C$ et $\gamma$ pour les SVM) est crucial, car ils contrôlent la complexité du 
modèle et le compromis biais/variance.

### 1.1. Intérêt par rapport à la validation simple

La procédure de validation simple (diviser l'ensemble d'entraînement en un ensemble d'apprentissage et un ensemble 
de validation) a une limitation : l'ensemble de validation peut être un **sous-ensemble trop petit** des données, ce 
qui ne représente potentiellement qu'une très petite partie de la distribution sous-jacente.  
La validation croisée ($k$-folds CV) permet de pallier ce défaut.

### 1.2. La procédure k-fold CV

La validation croisée fonctionne en divisant l'ensemble d'entraînement en $k$ groupes ou plis (*folds*). La procédure 
se déroule comme suit :

1. **Sélection des valeurs** : On choisit une plage de valeurs candidates pour l'hyperparamètre $\lambda$ (notées 
$\lambda_i$).

2. **Itérations ($k$ tours) : Pour chaque valeur $\lambda_i$ testée, on effectue $k$ tours :
		- À chaque tour $l$ (de 1 à k), le modèle est entraîné sur $k−1$ plis (appelés *learning set*).
		- Le modèle appris est ensuite validé sur le pli restant (pli $l$, appelé *validation set*) pour obtenir une 
mesure de performance $p_l$.
		- Le processus est répété $k$ fois, en changeant le pli de validation à chaque tour pour que l'ensemble des 
données d'entraînement soit parcouru.

3. ​**Calcul du score moyen** : Le score final $s_i$ pour cette valeur $\lambda_i$ est calculé comme la **moyenne des 
$k$ performances** mesurées :

$$
s_i = \frac{1}{k} \sum_{l=1}^k p_l
$$

4. **Sélection optimale** : L'hyper-paramètre $\lambda_{i_{\max}}$ qui a conduit au score moyen $s_i$ le plus élevé est 
retenu.

5. **Entraînement final** : Le modèle final est ensuite entraîné en utilisant la valeur optimale $\lambda_{i_{\max}}$ 
sur la **totalité de l'ensemble d'entraînement**.

Les TP impliquent spécifiquement l'utilisation d'une **5-CV** pour l'apprentissage des SVM avec noyau linéaire 
(tuning de $C$ parmi {0.1,0.5,1,2,4}) et avec noyau gaussien (tuning de $C$ parmi {0.1,0.5,1,2,4} et $\gammaù parmi 
{0.01,0.1,1,10}).

### 1.23. Avantages et coûts

- **Avantage** : Cette méthode de tuning permet de **parcourir l'ensemble complet des données d'entraînement** pour la 
validation, accédant ainsi à une plus grande partie de la distribution sous-jacente, ce qui conduit à de **meilleurs 
résultats pratiques**.

- **Coût** : La $k$-fold CV est coûteuse en calcul car elle nécessite d'apprendre k modèles pour chaque valeur 
d'hyperparamètre testée.

## 2. Le $k$-fold CV dans le protocole expérimental

La validation croisée est un élément clé de la **méthodologie expérimentale** qu'il faut maîtriser et documenter.

- **Documentation du protocole** : Dans la section "Protocole expérimental" d'un rapport, il convient  de présenter la 
manière dont les hyperparamètres sont optimisés, en précisant si l'on utilise la **cross-validation en $k$-folds** ou 
une  validation simple, ou si l'on choisit de fixer les hyper-paramètres. La documentation doit être suffisamment 
détaillée pour que le lecteur puisse **reproduire les résultats**.

- **Contraintes temporelles** : Attention à l'utilisation de la $k$-fold CV de manière aléatoire lorsque les données 
sont datées (comme dans le cas des transactions bancaires). Dans ces cas, la séparation entre l'ensemble d'entraînement 
(utilisé pour l'apprentissage et potentiellement la validation croisée si elle respecte l'ordre temporel) et l'ensemble 
de test doit être fixe et basée sur la date.

---

En résumé, la validation croisée est le mécanisme standard pour le **tuning rigoureux** des hyperparamètres, 
permettant de trouver le point d'équilibre entre la minimisation du risque empirique et la complexité du modèle, tout 
en optimisant sa capacité à généraliser.

