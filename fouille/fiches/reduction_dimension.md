<!--
Title: "Big Data Mining - Fiches - Réduction de dimension / ACP"
Author: rsquaredata
Last updated: 2025-12-1°
-->

# Réduction de dimension / ACP (PCA)

## 1. Pourquoi réduire la dimension ?

1. **Lutter contre le sur-apprentissage** : Quand p est grand (ex : 10 000 variables), les modèles deviennent instables → Rrduire la dimension améliore la généralisation.
2. **Réduire le coût computationnel** : Certains modèles explosent quand p est trop grand (SVM kernel, clustering, distance-based) → PCA = outil indispensable.
3. **Décorréler les variables** :  ACP = produit des axes orthogonaux → facilite l’apprentissage.

> Dans un contexte p ≫ n, la réduction de dimension est indispensable pour la stabilité, la performance et le coût.

## 2. ACP / PCA : intuition générale

L’ACP consiste à **projeter les données sur des directions (composantes principales) qui maximisent la variance**, sous contrainte d’orthogonalité.  

On obtient :  
- une base orthogonale
- triée par importance
- où chaque axe explique une part de la variance totale

## 3. Étapes mathématiques

1. **Centrer** les données (indispensable).
2. Optionnel : **standardiser** (si échelles différentes → très recommandé).
3. Calculer la **matrice de covariance** $\sigma$.
4. Calculer ses **vecteurs propres / valeurs propres**.
5. Trier les vecteurs propres par variance expliquée décroissante.
6. Projeter les données sur les k premiers axes : $Z = XW_k$, où $W_k$ = matrice des k vecteurs propres et $Z$ = données dans l’espace réduit.

Trop hétérogènes ? → toujours **standardiser**.

## 4. Comment choisir k (nombre de composantes) ?

1. **Variance cumulée** : Choisir $k$ tel que : $\text{Var expliquée cumulée}(k) \approx$ 80% - 95%.
2. **Coude** (scree plot) : Observer la cassure dans les valeurs propres.
3. **Objectif métier** : Parfois on veut réduire p=10 000 → k=50 ou 100 pour un modèle rapide.
4. **Cross-validation** (rare mais correct)

## 5. ACP dans les mises en situation

**p >> n (génomique, texte, images)** → ACP quasi obligatoire.  
> Avec 10 000 variables et seulement 1 000 individus, on applique une réduction de dimension (PCA) pour stabiliser l’apprentissage et éviter un modèle p≫n instable.

**Données corrélées** → ACP élimine la redondance.  

**vitesse / scalabilité** → ACP réduit la charge avant un modèle exigeant (SVM, clustering, kNN).  

**Visualisation 2D / 3D** → ACP pour explorer la structure.  

## 6. Alternatives à l’ACP

- Auto-encoders (deep learning) : Réduction non linéaire.
- t-SNE / UMAP : Visualisation uniquement, pas pour entraîner un modèle.
- Sélection de variables : attention ifférence importante →  ACP = transformation /  sélection = sous-ensemble de variables
- Méthodes L1 (Lasso) : induisent de la parcimonie → réduction indirecte.

## 7. ACP + modèle : pipeline recommandé

1. Standardisation
2. ACP (choix de k par variance cumulée)
3. Entraînement d'un modèle linéaire (logistic regression / SVM linéaire) ou d'un modèle complexe si p est réduit
4. Validation croisée
5. Analyse des performances

## 8. Points clés à mentionner

- ACP projette sur des axes orthogonaux maximisant la variance
- nécessite centrage et généralement standardisation
- appropriée quand p est très grand, quand les variables sont corrélées, ou quand on veut stabiliser un modèle linéaire
- choix du nombre d’axes via variance cumulée
- ACP = réduction linéaire → ne modélise pas les non-linéarités
- pipeline : standardiser → ACP → modèle → métrique

















