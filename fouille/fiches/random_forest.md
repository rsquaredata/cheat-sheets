<!--
Title: "Big Data Mining - Fiches - Random Forest"
Author: rsquaredata
Last updated: 2025-12-09
-->

# Random Forest

## 1. Principe général

Une **Random Forest** est un ensemble d'**arbres de décision** entraînés en **parallèle**, chacun sur une version différente des données.  
La prédiction finale est une **agrégation** des prédictions des arbres.  

Idée clé :  
> On réduit la variance d'un arbre en moyennan beaucoup d'arbres décorrélés.

## 2. Le double échantillonnage

Le cœeur des Random Forests repose sur **2 sources d'aléa** :

1. **Bootstrap des observations (bagging)** : pour chaque arbre,
    - on tire $n$ observations **avec remise**,
    - donc ~63% uniques, ~37% OOB (out-of-bag).

2. ** Sous-échantillonnage des variables (mtry)** :
    - à chaque split, on ne regarde qu'un **sous-ensemble aléatoire** de variables parmi $p$.
    - valeurs courantes : claasification → mtry = $\sqrt{p}$ / régression → mtry = $\frac{p}{3}$.
    - objectif : **décoreller les arbres**, donc réduire la variance du modèle final.

> Les Random Forest utulisent un double échantillonnage : *bootstrap des observations* et *sous-échantillonnage des variables à chaque nœud*, ce qui décorrelle les arbres et réduit la variance par simple moyennage.

## 3. Construction d'une Random Forest

1. Pour $t = 1 \ldots T$ arbres :
    - Tirage bootstrap des données
    - Construction d'un arbre **non élagué** (arbre profond, low bias)
    - À chaque nœud, choisir $m$ variables au hasard
    - Chercher le split optimal parmi ces $m$ variables  

2. Modèle final :
    - Classification → vote majoritaire
    - régression → moyenne des prédictions

## 4. Règles de vote

**Classification** :  
- **majorité simple** (classique)
- **majorité pondérée** : chaque arbre peut être pondéré (rare en pratique)
- *probabilités moyennées** : moyenne des distributions des feuilles  

**Régression** :  
- moyenne simple
- médiane (robuste aux outliers)

## 5. Pourquoi Random Forest fonctionne si bien ?

1. *Biais faible** : les abres sont **non élagués** → apprennent des interactions complexes.
2. **Variance maîtrisée** : bagging + mtry → arbres par nature très différents → variance ↓.
3. **Stable et robuste** : résiste au bruit, aux données mal nettoyées, aux variables inutiles.
4. **Peu de tuning** : hyperparamètres simples (nb d'arbres, profondeur, mtry).

## 6. Avantages / Limites

**Avantages** :  
- très robuste au surapprentissage
- fonctionne bien sans tuning lourd
- gère les variables qualitatives sans encodage spécial
- donne des mesures d'importance des variables
- supporte les grands $p$, petits $n$
- supporte le déséquilibre si class weight / sampling

**Limites** :  
- moins interprétable qu'un arbre
- coût computationnnel (T arbres profonds)
- pas optimal pour les très grandes bases (XGBoost souvent meilleur)
- mauvais en extrapolation pour la régression (comme tout arbre)

## 7. OOB (Out-of-Bag)

Les ~37% d'observations non utilisées dans un arbre servent à :  
- estimer l'erreur OOB
- estimer l'importance des variables
- jouer un rôle de validation croisées interne  

> Les données OOB forment une estimation non biaisée de l'erreur, évitant l'usage d'un set de validation.  
