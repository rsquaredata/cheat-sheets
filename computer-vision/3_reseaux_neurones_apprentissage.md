<!--
Title: "Réseaux de neurones et apprentissage"
Author: rsquaredata
Last updated: 2026-09-08
-->
# 3. Réseaux de neurones et apprentissage

---

## 3.1. Du modèle linéaire au neurone artificiel

Un neurone artificiel commence par calculer une combinaison linéaire de ses entrées.

Pour une observation :

$$
\mathbf{x} =
\begin{bmatrix}
x_1 \\
x_2 \\
\vdots \\
x_p
\end{bmatrix}
$$

le neurone calcule :

$$
z = w_1x_1 + w_2x_2 + \cdots + w_px_p + b
$$

Sous forme vectorielle :

$$
z = \mathbf{w}^{\top}\mathbf{x} + b
$$

avec :

- $\mathbf{x}$ : vecteur des features d’entrée ;
- $\mathbf{w}$ : vecteur des poids ;
- $b$ : biais ;
- $z$ : score linéaire.

Le score est ensuite transformé par une fonction d’activation $f$ :

$$
a = f(z)
$$

### Schéma

```text
x₁ ── w₁ ┐
x₂ ── w₂ ├─→ somme + biais → activation → sortie
...      │
xₚ ── wₚ ┘
```

## 3.2. Rôle des poids et du biais

### 3.2.1. Poids

Un poids contrôle l’influence d’une entrée sur le score du neurone.

#### Poids positif

Si $w_j > 0$, une augmentation de $x_j$ tend à augmenter $z$, toutes choses égales par ailleurs.

#### Poids négatif

Si $w_j < 0$, une augmentation de $x_j$ tend à diminuer $z$.

#### Poids proche de zéro

Si $w_j \approx 0$, la variable possède peu d’influence directe sur ce neurone.

### 2.2 Biais

Sans biais :

$$
z = \mathbf{w}^{\top}\mathbf{x}
$$

Avec biais :

$$
z = \mathbf{w}^{\top}\mathbf{x} + b
$$

Le biais permet de déplacer la fonction de décision. Il évite d’imposer que la transformation passe par l’origine.

## 3.3. Pourquoi ajouter une fonction d’activation ?

Supposons deux couches linéaires successives :

$$
\mathbf{z}^{(1)} = W^{(1)}\mathbf{x} + \mathbf{b}^{(1)}
$$

puis :

$$
\mathbf{z}^{(2)} = W^{(2)}\mathbf{z}^{(1)} + \mathbf{b}^{(2)}
$$

En remplaçant $\mathbf{z}^{(1)}$ :

$$
\mathbf{z}^{(2)} = W^{(2)} \left(W^{(1)}\mathbf{x} + \mathbf{b}^{(1)} \right) + \mathbf{b}^{(2)}
$$

Donc :

$$
\mathbf{z}^{(2)} = W^{(2)}W^{(1)}\mathbf{x} + W^{(2)}\mathbf{b}^{(1)} + \mathbf{b}^{(2)}
$$

Cette composition reste une transformation linéaire ou affine.

Même en empilant plusieurs couches linéaires, le réseau resterait équivalent à une seule transformation affine.

Les fonctions d’activation introduisent donc la non-linéarité nécessaire pour apprendre des relations complexes.

```text
Transformation affine
+
Activation non linéaire
↓
Frontières et représentations complexes
```

## 3.4. Fonctions d’activation

### 3.4.1. Fonction seuil

La fonction seuil historique peut être définie par :

$$
f(z) =
\begin{cases}
1 & \text{si } z \geq 0 \\
0 & \text{si } z < 0
\end{cases}
$$

Elle n’est pas différentiable en $z=0$ et son gradient est nul ailleurs.

Elle n’est donc pas adaptée à l’apprentissage moderne par descente de gradient.

### 3.4.2. Sigmoid

La fonction sigmoid est définie par :

$$
\sigma(z) = \frac{1}{1+e^{-z}}
$$

Sa sortie appartient à :

$$
\sigma(z) \in ]0,1[
$$

Sa dérivée est :

$$
\sigma'(z) = \sigma(z)\left(1-\sigma(z)\right)
$$

#### Utilisation principale

Elle est principalement utilisée en sortie pour :

- la classification binaire ;
- la classification multilabel.

#### Limites dans les couches cachées

Lorsque $|z|$ devient grand, la sigmoid sature :

$$
z \rightarrow +\infty
\quad \Rightarrow \quad
\sigma(z) \rightarrow 1
$$

$$
z \rightarrow -\infty
\quad \Rightarrow \quad
\sigma(z) \rightarrow 0
$$

Dans ces régions :

$$
\sigma'(z) \approx 0
$$

Les gradients deviennent alors très faibles.

### 3.4.3. Tangente hyperbolique

La fonction tangente hyperbolique est définie par :

$$
\tanh(z) = \frac{e^z-e^{-z}}{e^z+e^{-z}}
$$

Sa sortie appartient à :

$$
\tanh(z) \in ]-1,1[
$$

Sa dérivée est :

$$
\frac{d}{dz}\tanh(z) = 1-\tanh^2(z)
$$

#### Avantage

Les sorties sont centrées autour de zéro.

### Limite

Comme la sigmoid, $\tanh$ peut saturer et produire des gradients très faibles.

### 3.4.4. ReLU

La fonction ReLU est définie par :

$$
\mathrm{ReLU}(z) = \max(0,z)
$$

Autrement dit :

$$
\mathrm{ReLU}(z) =
\begin{cases}
z & \text{si } z>0 \\
0 & \text{si } z\leq 0
\end{cases}
$$

Sa dérivée vaut presque partout :

$$
\mathrm{ReLU}'(z) =
\begin{cases}
1 & \text{si } z>0 \\
0 & \text{si } z<0
\end{cases}
$$

#### Avantages

- calcul simple ;
- activation rapide ;
- limite une partie du problème de vanishing gradient ;
- très utilisée dans les réseaux profonds.

#### Limite — Dying ReLU

Si un neurone reçoit durablement des valeurs négatives :

$$
\mathrm{ReLU}(z)=0
$$

et :

$$
\mathrm{ReLU}'(z)=0
$$

Le neurone peut alors cesser d’apprendre.

### 3.4.5. Leaky ReLU

La Leaky ReLU conserve une faible pente dans la partie négative :

$$
\mathrm{LeakyReLU}(z) =
\begin{cases}
z & \text{si } z>0 \\
\alpha z & \text{si } z\leq 0
\end{cases}
$$

avec généralement :

$$
0 < \alpha \ll 1
$$

Elle réduit le risque de neurones durablement inactifs.

### 3.4.6. GELU

La fonction GELU pondère progressivement l’entrée au lieu d’appliquer une coupure nette.

Une approximation courante est :

$$
\mathrm{GELU}(z) \approx \frac{z}{2} \left[1+ \tanh \left(\sqrt{\frac{2}{\pi}} \left(z+0.044715z^3 \right) \right) \right]
$$

Elle est notamment utilisée dans :

- les Transformers ;
- les Vision Transformers ;
- certaines architectures modernes de vision.

#### 🧠 Réflexe

##### Couches cachées classiques

```text
ReLU ou une variante
```

##### Sortie binaire ou multilabel

```text
Sigmoid
```

##### Sortie multiclasse exclusive

```text
Softmax
```

Le choix de l’activation finale doit être cohérent avec la fonction de perte.

## 3.5. Couche dense

Une couche dense connecte chaque entrée à chaque neurone de sortie.

Pour une observation :

$$
\mathbf{z} = W\mathbf{x} + \mathbf{b}
$$

Pour un batch de $N$ observations :

$$
Z = XW + \mathbf{b}
$$

avec :

$$
X \in \mathbb{R}^{N\times d_{\text{in}}}
$$

$$
W \in \mathbb{R}^{d_{\text{in}}\times d_{\text{out}}}
$$

$$
\mathbf{b} \in \mathbb{R}^{d_{\text{out}}}
$$

$$
Z \in \mathbb{R}^{N\times d_{\text{out}}}
$$

### Nombre de paramètres

Une couche dense possède $d_{\text{in}}d_{\text{out}} + d_{\text{out}}$ paramètres.

Le premier terme correspond aux poids et le second aux biais.

### Exemple

Pour :

$$
d_{\text{in}}=128
\qquad
d_{\text{out}}=64
$$

le nombre de paramètres vaut :

$$
128\times64+64 = 8\,256
$$

## 3.6. Perceptron multicouche

Un **MLP**, ou Multi-Layer Perceptron, empile plusieurs couches.

Pour la couche $\ell$ :

$$
\mathbf{z}^{(\ell)} = W^{(\ell)} \mathbf{a}^{(\ell-1)} + \mathbf{b}^{(\ell)}
$$

puis :

$$
\mathbf{a}^{(\ell)} = f^{(\ell)} \left( \mathbf{z}^{(\ell)} \right)
$$

L’entrée du réseau est :

$$
\mathbf{a}^{(0)}=\mathbf{x}
$$

### Exemple

```text
Entrée x
↓
Linear(100 → 64)
↓
ReLU
↓
Linear(64 → 32)
↓
ReLU
↓
Linear(32 → 10)
↓
Logits
```

La dernière couche produit ici dix scores, par exemple pour dix classes.

## 3.7. Profondeur et largeur

### Profondeur

La profondeur correspond au nombre de transformations successives.

Un réseau profond peut construire une hiérarchie de représentations :

```text
Motifs simples
↓
Combinaisons de motifs
↓
Structures complexes
↓
Décision
```

### Largeur

La largeur correspond au nombre de neurones dans une couche.

Une couche plus large peut représenter davantage de motifs simultanément.

### ⚠️ Plus grand ≠ automatiquement meilleur

Ajouter des couches ou des neurones augmente :

- la capacité du modèle ;
- le coût de calcul ;
- le besoin en données ;
- le risque d’overfitting ;
- la difficulté d’optimisation.

## 3.8. Propagation avant

La propagation avant, ou **forward pass**, calcule la sortie du réseau.

Pour un réseau à $L$ couches :

$$
\mathbf{a}^{(0)} = \mathbf{x}
$$

puis, pour chaque couche $\ell$ :

$$
\mathbf{z}^{(\ell)} = W^{(\ell)} \mathbf{a}^{(\ell-1)} + \mathbf{b}^{(\ell)}
$$

$$
\mathbf{a}^{(\ell)} = f^{(\ell)} \left(\mathbf{z}^{(\ell)} \right)
$$

La dernière sortie permet de calculer la prédiction et la loss.

```text
Entrée
↓
Transformations successives
↓
Logits
↓
Loss ou prédiction
```

## 3.9. Logits, probabilités et classes

### 3.9.1. Logits

Les logits sont les scores bruts produits par la dernière couche.

Exemple :

$$
\mathbf{z} =
\begin{bmatrix}
2.4 & -0.7 & 1.1
\end{bmatrix}
$$

Ils ne sont pas nécessairement compris entre 0 et 1 et leur somme n’est pas nécessairement égale à 1.

### 3.9.2. Probabilités

Les logits sont transformés par une fonction adaptée :

- sigmoid pour des probabilités indépendantes ;
- softmax pour des classes mutuellement exclusives.

### 3.9.3. Classe prédite

En classification multiclasse :

$$
\widehat{y} = \underset{k}{\arg\max}\; z_k
$$

L’argmax des logits est identique à l’argmax des probabilités obtenues par softmax.

#### 🧠 Chaîne complète

```text
Réseau
↓
Logits
↓
Sigmoid ou Softmax
↓
Probabilités
↓
Seuil ou argmax
↓
Classe prédite
```

## 3.10. Classification binaire

La cible possède deux valeurs :

$$
y\in\{0,1\}
$$

Le réseau peut produire un seul logit $z$.

La probabilité de la classe positive est :

$$
p = P(y=1\mid\mathbf{x}) = \sigma(z)
$$

Avec un seuil $\tau$ :

$$
\widehat{y} =
\begin{cases}
1 & \text{si } p\geq\tau \\
0 & \text{si } p<\tau
\end{cases}
$$

Le seuil par défaut est souvent $\tau=0.5$ mais il peut être modifié selon le coût des faux positifs et des faux négatifs.

### Binary Cross-Entropy

Pour une observation :

$$
\mathcal{L}_{\mathrm{BCE}} = - \left[y\log(p) + (1-y)\log(1-p) \right]
$$

Pour $N$ observations :

$$
\mathcal{L}_{\mathrm{BCE}} = - \frac{1}{N} \sum_{i=1}^{N} \left[ y_i \log(p_i) + (1-y_i)\log(1-p_i) \right]
$$

## 3.11. Classification multiclasse

Une observation appartient à une seule classe parmi $K$ :

$$
y\in\{1,\ldots,K\}
$$

Le réseau produit $K$ logits :

$$
\mathbf{z} =
\begin{bmatrix}
z_1 & z_2 & \cdots & z_K
\end{bmatrix}
$$

### Softmax

La probabilité de la classe $k$ est :

$$
P(y=k\mid\mathbf{x}) = \frac{e^{z_k}} {\displaystyle\sum_{j=1}^{K}e^{z_j}}
$$

Les probabilités vérifient :

$$
0\leq P(y=k\mid\mathbf{x})\leq1
$$

et :

$$
\sum_{k=1}^{K} P(y=k\mid\mathbf{x}) = 1
$$

### Prédiction

$$
\widehat{y} = \underset{k}{\arg\max}\; P(y=k\mid\mathbf{x})
$$

### Cross-Entropy multiclasse

Si $y$ est la classe réelle :

$$
\mathcal{L}_{\mathrm{CE}} = -\log P(y\mid\mathbf{x})
$$

Avec un encodage one-hot :

$$
\mathcal{L}_ {\mathrm{CE}} = - \sum_{k=1}^{K} y_k\log(p_k)
$$

Une prédiction fausse et très confiante est fortement pénalisée.

## 3.12. Classification multilabel

Plusieurs classes peuvent être vraies simultanément.

La cible est un vecteur binaire :

$$
\mathbf{y} =
\begin{bmatrix}
y_1 & y_2 & \cdots & y_K
\end{bmatrix}
$$

avec :

$$
y_k\in\{0,1\}
$$

Le réseau produit un logit indépendant par classe :

$$
\mathbf{z} =
\begin{bmatrix}
z_1 & z_2 & \cdots & z_K
\end{bmatrix}
$$

Une sigmoid est appliquée à chaque logit :

$$
p_k = \sigma(z_k)
$$

Les probabilités ne doivent pas avoir une somme égale à 1.

La loss totale peut être écrite :

$$
\mathcal{L} = - \sum_{k=1}^{K} \left[ y_k\log(p_k) + (1-y_k)\log(1-p_k) \right]
$$

#### ⚠️ Softmax incorrect

Softmax impose une compétition entre les classes.

Il convient à une classification multiclasse exclusive, mais pas au multilabel.

## 3.13. Régression

Pour une régression, la cible est continue :

$$
y\in\mathbb{R}
$$

La dernière couche produit généralement une valeur sans sigmoid ni softmax.

##3 Mean Squared Error

$$
\mathrm{MSE} = \frac{1}{N} \sum_{i=1}^{N} \left( y_i-\widehat{y}_i \right)^2
$$

### Mean Absolute Error

$$
\mathrm{MAE} = \frac{1}{N} \sum_{i=1}^{N} \left| y_i-\widehat{y}_i \right|
$$

La MSE pénalise davantage les grosses erreurs, tandis que la MAE y est moins sensible.

## 3.14. Fonction de perte

La fonction de perte mesure l’écart entre la prédiction et la cible.

Pour un modèle de paramètres $\boldsymbol{\theta}$ :

$$
\widehat{y}_ i =
f_{\boldsymbol{\theta}}(\mathbf{x}_i)
$$

La loss empirique moyenne est :

$$
\mathcal{L}(\boldsymbol{\theta}) =
\frac{1}{N}
\sum_{i=1}^{N}
\ell
\left(
y_i,
f_{\boldsymbol{\theta}}(\mathbf{x}_i)
\right)
$$

L’apprentissage cherche :

$$
\boldsymbol{\theta}^{*} =
\underset{\boldsymbol{\theta}}{\arg\min}
\;
\mathcal{L}(\boldsymbol{\theta})
$$

## 3.15. Loss et métrique

### Loss

La loss est utilisée pour calculer les gradients et entraîner le modèle.

Elle doit être différentiable, ou au moins sous-différentiable, par rapport aux paramètres.

Exemples :

- Cross-Entropy ;
- Binary Cross-Entropy ;
- MSE.

### Métrique

La métrique sert à interpréter les performances.

Exemples :

- accuracy ;
- precision ;
- recall ;
- F1 ;
- IoU ;
- Dice.

#### Exemple

Un modèle peut être :

```text
entraîné avec Cross-Entropy
```

et :

```text
évalué avec Macro F1
```

## 3.16. Pourquoi ne pas optimiser directement l’accuracy ?

L’accuracy est définie par :

$$
\mathrm{Accuracy} =
\frac{1}{N}
\sum_{i=1}^{N}
\mathbf{1}
\left(
\widehat{y}_i=y_i
\right)
$$

L’indicatrice $\mathbf{1}(\cdot)$ produit une décision discrète.

Une petite modification des paramètres peut ne provoquer aucun changement d’accuracy.

La Cross-Entropy utilise au contraire les probabilités continues.

#### Exemple

Deux prédictions incorrectes donnent à la classe réelle :

$$
p_1=0.49
$$

et :

$$
p_2=0.01
$$

L’accuracy considère les deux prédictions comme fausses.

Mais :

$$
-\log(0.01) > -\log(0.49)
$$

La Cross-Entropy pénalise donc davantage la prédiction fausse extrêmement confiante.

## 3.17. Rétropropagation

La rétropropagation calcule la dérivée de la loss par rapport à chaque paramètre.

Pour un paramètre $\theta_j$ :

$$
\frac{\partial\mathcal{L}}{\partial\theta_j}
$$

indique comment une petite variation de ce paramètre modifierait la loss.

La rétropropagation applique la règle de dérivation en chaîne à travers toutes les couches.

Si :

$$
\mathcal{L} = \mathcal{L}(a)
$$

$$
a=f(z)
$$

$$
z=wx+b
$$

alors :

$$
\frac{\partial\mathcal{L}}{\partial w} =
\frac{\partial\mathcal{L}}{\partial a}
\cdot
\frac{\partial a}{\partial z}
\cdot
\frac{\partial z}{\partial w}
$$

Comme :

$$
\frac{\partial z}{\partial w}=x
$$

on obtient :

$$
\frac{\partial\mathcal{L}}{\partial w} =
\frac{\partial\mathcal{L}}{\partial a}
\cdot
f'(z)
\cdot
x
$$

## 3.18. Gradient

Le gradient regroupe les dérivées de la loss par rapport à tous les paramètres :

$$
\nabla_{\boldsymbol{\theta}} \mathcal{L} =
\begin{bmatrix}
\frac{\partial\mathcal{L}}{\partial\theta_1} \\
\frac{\partial\mathcal{L}}{\partial\theta_2} \\
\vdots \\
\frac{\partial\mathcal{L}}{\partial\theta_m}
\end{bmatrix}
$$

Il indique la direction d’augmentation la plus rapide de la loss.

Pour réduire la loss, les paramètres sont déplacés dans la direction opposée.

## 3.19. Descente de gradient

La règle de mise à jour est :

$$
\boldsymbol{\theta}_ {t+1} = \boldsymbol{\theta}_ t - \eta \nabla_{\boldsymbol{\theta}} \mathcal{L}
\left(
\boldsymbol{\theta}_t
\right)
$$

avec :

- $\boldsymbol{\theta}_t$ : paramètres actuels ;
- $\eta$ : learning rate ;
- $\nabla_{\boldsymbol{\theta}}\mathcal{L}$ : gradient.

### Batch Gradient Descent

Le gradient est calculé sur les $N$ observations :

$$
\nabla\mathcal{L} =
\frac{1}{N}
\sum_{i=1}^{N}
\nabla\ell_i
$$

#### Avantage

Gradient stable.

#### Limites

- calcul coûteux ;
- une seule mise à jour par epoch ;
- difficile avec de très grands datasets.

### Stochastic Gradient Descent

Une seule observation est utilisée :

$$
\nabla\mathcal{L} \approx \nabla\ell_i
$$

Le gradient est rapide mais très bruité.

### Mini-Batch Gradient Descent

Un sous-ensemble $\mathcal{B}$ est utilisé :

$$
\nabla\mathcal{L}_ {\mathcal{B}} =
\frac{1}{|\mathcal{B}|}
\sum_{i\in\mathcal{B}}
\nabla\ell_i
$$

C’est l’approche habituelle en Deep Learning.

## 3.20. Batch, itération et epoch

### Batch

Sous-ensemble d’observations traité simultanément.

### Batch size

Nombre d’observations dans le batch :

$$
B=|\mathcal{B}|
$$

### Itération

Une itération comprend :

1. un forward pass ;
2. un calcul de loss ;
3. une rétropropagation ;
4. une mise à jour des paramètres.

### Epoch

Une epoch correspond à un passage complet sur le dataset d’entraînement.

Pour $N$ observations et un batch size $B$, le nombre approximatif d’itérations par epoch est :

$$
\left\lceil \frac{N}{B} \right\rceil
$$

### Exemple

Avec :

$$
N=3\,200 \qquad B=32
$$

on obtient :

$$
\frac{3\,200}{32} = 100
$$

itérations par epoch.

## 3.21. Learning rate

Le learning rate $\eta$ contrôle l’amplitude des mises à jour :

$$
\Delta\boldsymbol{\theta} =
-\eta
\nabla_{\boldsymbol{\theta}}
\mathcal{L}
$$

### Learning rate trop faible

- apprentissage lent ;
- nombreuses epochs nécessaires ;
- progression presque invisible.

### Learning rate trop élevé

- oscillations ;
- divergence ;
- loss instable ;
- minimum dépassé.

### Learning rate adapté

- diminution régulière de la loss ;
- convergence efficace ;
- apprentissage stable.

#### 🧠 Réflexe

Le learning rate est souvent l’un des hyperparamètres les plus importants de l’entraînement.

## 3.22. Optimiseurs

### 3.22.1. SGD

La mise à jour SGD est :

$$
\boldsymbol{\theta}_{t+1} = \boldsymbol{\theta}_t - \eta\mathbf{g}_t
$$

avec :

$$
\mathbf{g}_ t = \nabla_{\boldsymbol{\theta}}
\mathcal{L}_{\mathcal{B}_t}
$$

#### Avantages

- simple ;
- peu coûteux en mémoire ;
- bonne généralisation possible.

#### Limite

Il peut converger lentement ou osciller.

### 3.22.2. Momentum

Une écriture simplifiée est :

$$
\mathbf{v}_ t = \beta\mathbf{v}_{t-1} + \mathbf{g}_t
$$

puis :

$$
\boldsymbol{\theta}_{t+1} = \boldsymbol{\theta}_t - \eta\mathbf{v}_t
$$

avec $\beta$ contrôlant la mémoire des directions précédentes.

#### Intuition

Une bille descendant une pente acquiert de l’élan.

Le Momentum :

- accélère dans les directions cohérentes ;
- réduit certaines oscillations ;
- aide à traverser de petites irrégularités.

### 3.22.3. Adam

Adam maintient des estimations des deux premiers moments du gradient.

Premier moment :

$$
\mathbf{m}_ t = \beta_1\mathbf{m}_{t-1} + (1-\beta_1)\mathbf{g}_t
$$

Second moment :

$$
\mathbf{v}_ t = \beta_2\mathbf{v}_{t-1} + (1-\beta_2)\mathbf{g}_t^2
$$

Après correction du biais :

$$
\widehat{\mathbf{m}}_t = \frac{\mathbf{m}_t}{1-\beta_1^t}
$$

$$
\widehat{\mathbf{v}}_t = \frac{\mathbf{v}_t}{1-\beta_2^t}
$$

La mise à jour devient :

$$
\boldsymbol{\theta}_{t+1} = \boldsymbol{\theta}_t -
\eta
\frac{
\widehat{\mathbf{m}}_t
}{
\sqrt{\widehat{\mathbf{v}}_t}
+
\varepsilon
}
$$

#### Avantages

- convergence souvent rapide ;
- adaptation du pas pour chaque paramètre ;
- baseline pratique sur de nombreux problèmes.

#### Limites

- davantage de mémoire ;
- learning rate toujours important ;
- pas systématiquement la meilleure généralisation finale.

## 3.23. Weight decay

La régularisation $L_2$ ajoute une pénalité à la loss :

$$
\mathcal{L}_ {\mathrm{total}} = \mathcal{L}_{\mathrm{data}}
+
\lambda
\lVert
\boldsymbol{\theta}
\rVert_2^2
$$

avec :

- $\lambda$ : force de régularisation ;
- $\lVert\boldsymbol{\theta}\rVert_2^2$ : somme des poids au carré.

#### Effet recherché

- limiter les poids extrêmes ;
- réduire l’overfitting ;
- améliorer la généralisation.

AdamW applique le weight decay séparément de l’adaptation du gradient.

## 3.24. Learning rate scheduler

Le learning rate peut évoluer pendant l’apprentissage :

$$
\eta = \eta_t
$$

Une stratégie courante consiste à utiliser :

```text
learning rate relativement élevé au début
↓
learning rate plus faible à la fin
```

### Intuition

Au début :

- progression rapide ;
- exploration de l’espace des paramètres.

À la fin :

- ajustements plus fins ;
- stabilisation autour d’une bonne solution.

### Stratégies possibles

- réduction par paliers ;
- décroissance exponentielle ;
- cosine annealing ;
- réduction lorsque la validation stagne ;
- warm-up initial.

## 3.25. Initialisation des poids

Tous les poids ne doivent pas être initialisés à la même valeur.

### Problème d’une initialisation identique

Si deux neurones commencent avec les mêmes poids :

- ils produisent les mêmes activations ;
- ils reçoivent les mêmes gradients ;
- ils apprennent la même représentation.

L’initialisation aléatoire brise cette symétrie.

### Initialisation de Xavier

Une variance typique est choisie en fonction du nombre d’entrées et de sorties :

$$
\mathrm{Var}(W)
\approx
\frac{2}
{d_{\mathrm{in}}+d_{\mathrm{out}}}
$$

Elle est adaptée à certaines activations comme $\tanh$.

### Initialisation de He

Pour les activations ReLU :

$$
\mathrm{Var}(W)
\approx
\frac{2}{d_{\mathrm{in}}}
$$

Elle aide à conserver une échelle d’activation raisonnable à travers les couches.

## 3.26. Vanishing gradient

Dans un réseau profond, la règle de dérivation en chaîne multiplie de nombreux termes.

Schématiquement :

$$
\frac{\partial\mathcal{L}} {\partial W^{(1)}} =
\frac{\partial\mathcal{L}}
{\partial \mathbf{a}^{(L)}}
\prod_{\ell=2}^{L}
\frac{\partial\mathbf{a}^{(\ell)}}
{\partial\mathbf{a}^{(\ell-1)}}
\frac{\partial\mathbf{a}^{(1)}}
{\partial W^{(1)}}
$$

Si de nombreux facteurs ont une norme inférieure à 1, le gradient peut devenir extrêmement faible.

```text
Couche de sortie
↓
Multiplication de petits gradients
↓
Premières couches apprennent très lentement
```

### Solutions courantes

- ReLU et variantes ;
- initialisation adaptée ;
- normalisation ;
- connexions résiduelles ;
- architectures mieux conçues.

## 3.27. Exploding gradient

Si les facteurs successifs sont trop grands, la norme du gradient peut exploser :

$$
\left\|
\nabla_{\boldsymbol{\theta}}
\mathcal{L}
\right\|
\rightarrow +\infty
$$

### Conséquences

- mises à jour énormes ;
- loss instable ;
- valeurs infinies ;
- apparition de `NaN`.

### Gradient clipping

Une méthode consiste à limiter la norme du gradient :

$$
\mathbf{g}
\leftarrow
\mathbf{g}
\cdot
\min
\left(
1,
\frac{c}{\lVert\mathbf{g}\rVert}
\right)
$$

avec $c$ la norme maximale autorisée.

## 3.28. Boucle d’entraînement

```text
Initialiser le modèle
↓
Pour chaque epoch
    ↓
    Pour chaque batch
        ↓
        Forward pass
        ↓
        Calcul de la loss
        ↓
        Remise à zéro des gradients
        ↓
        Backpropagation
        ↓
        Mise à jour des paramètres
↓
Évaluation sur Validation
```

### Pseudo-code PyTorch

```python
for epoch in range(num_epochs):
    model.train()

    for X, y in train_loader:
        optimizer.zero_grad()

        logits = model(X)
        loss = criterion(logits, y)

        loss.backward()
        optimizer.step()

    model.eval()

    with torch.no_grad():
        evaluate_on_validation()
```

## 3.29. Pourquoi remettre les gradients à zéro ?

Dans certaines bibliothèques, les gradients s’accumulent :

$$
\mathbf{g}_ {\mathrm{stocké}}
\leftarrow
\mathbf{g}_ {\mathrm{stocké}}
+
\mathbf{g}_{\mathrm{nouveau}}
$$

Sans remise à zéro, les gradients des batches précédents sont ajoutés au gradient actuel.

D’où :

```python
optimizer.zero_grad()
```

avant la nouvelle rétropropagation.

Cette accumulation peut être utilisée volontairement, mais elle doit alors être contrôlée explicitement.

## 3.30. Mode Train et mode Evaluation

Certaines couches se comportent différemment pendant l’entraînement et l’évaluation.

Exemples :

- Dropout ;
- Batch Normalization.

### Entraînement

```python
model.train()
```

### Validation ou Test

```python
model.eval()

with torch.no_grad():
    ...
```

L’absence de calcul des gradients permet :

- de réduire la mémoire ;
- d’accélérer le calcul ;
- d’éviter la construction inutile du graphe de dérivation.

## 3.31. Courbes d’apprentissage

Il faut suivre au minimum :

- loss du Train ;
- loss de Validation ;
- métrique du Train ;
- métrique de Validation.

### Apprentissage cohérent

```text
Train Loss ↓
Validation Loss ↓
```

### Underfitting

```text
Train mauvais
Validation mauvaise
```

Causes possibles :

- modèle trop simple ;
- entraînement trop court ;
- learning rate inadapté ;
- données insuffisamment informatives ;
- erreur d’implémentation.

### Overfitting

```text
Train continue de s’améliorer
Validation se dégrade
```

Le modèle mémorise progressivement des particularités du Train qui ne se généralisent pas.

## 3.32. Early Stopping

L’Early Stopping arrête l’apprentissage lorsque la performance de validation ne s’améliore plus.

Si $\mathcal{L}_{\mathrm{val}}^{(t)}$ est la loss de validation à l’epoch $t$, on conserve :

$$
t^{*} = \underset{t}{\arg\min} \; \mathcal{L}_{\mathrm{val}}^{(t)}
$$

Le modèle final doit utiliser les paramètres enregistrés à l’epoch $t^{*}$.

### Patience

La patience correspond au nombre d’epochs sans amélioration toléré avant l’arrêt.

#### ⚠️ Piège

Le meilleur modèle de validation n’est pas nécessairement celui de la dernière epoch.

## 3.33. Reproductibilité

L’apprentissage contient plusieurs sources d’aléatoire :

- initialisation des poids ;
- mélange des données ;
- data augmentation ;
- composition des batches ;
- certaines opérations matérielles.

Pour améliorer la reproductibilité :

- fixer les seeds ;
- enregistrer les hyperparamètres ;
- conserver le même split ;
- enregistrer les versions logicielles ;
- sauvegarder les checkpoints ;
- répéter les expériences importantes.

#### ⚠️ Seed fixe ≠ reproductibilité absolue

Certaines opérations peuvent rester non déterministes selon le matériel et les bibliothèques.

## 3.34. Choisir la bonne sortie

### Classification binaire

```text
1 logit
+
Binary Cross-Entropy avec logits
```

Mathématiquement :

$$
z\in\mathbb{R} \quad\Rightarrow\quad p=\sigma(z)
$$

### Classification multiclasse

```text
K logits
+
Cross-Entropy
```

Mathématiquement :

$$
\mathbf{z}\in\mathbb{R}^{K} \quad\Rightarrow\quad \mathbf{p}=\mathrm{softmax}(\mathbf{z})
$$

### Classification multilabel

```text
K logits indépendants
+
Binary Cross-Entropy avec logits
```

Mathématiquement :

$$
p_k=\sigma(z_k) \qquad \forall k\in\{1,\ldots,K\}
$$

### Régression scalaire

```text
1 valeur
+
MAE ou MSE
```

### Régression multiple

```text
Plusieurs valeurs continues
+
Loss de régression
```

## 3.35. Activations et losses intégrées

Les implémentations modernes combinent souvent l’activation finale et la loss pour améliorer la stabilité numérique.

### Classification binaire

Une loss comme :

```python
BCEWithLogitsLoss
```

reçoit directement les logits.

Il ne faut pas appliquer manuellement une sigmoid avant cette loss.

### Classification multiclasse

Une Cross-Entropy standard reçoit également directement les logits.

Il ne faut généralement pas appliquer manuellement Softmax avant la loss.

#### 🧠 Réflexe

```text
Pendant l’entraînement
→ transmettre les logits à la loss

Pour interpréter les résultats
→ convertir les logits en probabilités
```

## 3.36. Diagnostic rapide d’un entraînement

### La loss ne diminue pas

Vérifier :

- learning rate ;
- labels ;
- formes des tenseurs ;
- normalisation des entrées ;
- cohérence entre sortie et loss ;
- présence des gradients ;
- paramètres transmis à l’optimiseur.

### La loss devient `NaN`

Vérifier :

- learning rate trop élevé ;
- division par zéro ;
- logarithme invalide ;
- données infinies ;
- gradients explosifs ;
- mauvaise normalisation.

### L’accuracy reste au niveau du hasard

Pour $K$ classes équilibrées, une accuracy proche du hasard vaut approximativement :

$$
\frac{1}{K}
$$

Vérifier :

- association entre entrées et labels ;
- ordre des classes ;
- dernière couche ;
- fonction de perte ;
- mise à jour effective des paramètres.

### Train excellent, Validation mauvaise

→ overfitting probable

## Train et Validation mauvais

→ underfitting, problème de données ou problème d’optimisation

## 3.37. Test d’overfitting sur un petit batch

Un test de débogage très utile consiste à entraîner le réseau sur un nombre minuscule d’exemples.

Exemple :

```text
8 ou 16 observations
```

Un modèle suffisamment flexible devrait pouvoir presque mémoriser ce petit batch :

$$
\mathcal{L}_{\mathrm{train}} \rightarrow 0
$$

ou obtenir une accuracy proche de $100\%$

### S’il n’y parvient pas

Une erreur est possible dans :

- le modèle ;
- la loss ;
- les labels ;
- la boucle d’entraînement ;
- les gradients ;
- le prétraitement.

#### 🧠 Réflexe

```text
Avant un long entraînement
↓
Essayer d’overfit un petit batch
```

## 3.38. 🧠 Cycle complet d’apprentissage

```text
Batch d’entrées
↓
Forward pass
↓
Logits
↓
Calcul de la loss
↓
Backpropagation
↓
Gradients
↓
Optimiseur
↓
Paramètres mis à jour
↓
Batch suivant
```

À la fin de l’epoch :

```text
Évaluation sur Validation
↓
Analyse des courbes
↓
Conserver le meilleur checkpoint
↓
Continuer, corriger ou arrêter
```

## 3.39. 🧠 Si le modèle n’apprend pas

> La loss du Train diminue-t-elle ?

### Non

Vérifier dans cet ordre :

1. données et labels ;
2. formes des tenseurs ;
3. sortie et loss ;
4. learning rate ;
5. gradients ;
6. mise à jour des paramètres ;
7. normalisation.

### Oui, mais uniquement sur le Train

→ overfitting

### Oui sur Train et Validation

→ apprentissage cohérent

### Oui, puis la Validation se dégrade

→ enregistrer le meilleur checkpoint  
→ utiliser l’Early Stopping  
→ renforcer la régularisation

---

## 3.40. À retenir

- Un neurone calcule une combinaison affine suivie d’une activation.
- Les activations introduisent la non-linéarité.
- Une couche dense connecte toutes ses entrées à toutes ses sorties.
- Un réseau profond construit plusieurs niveaux de représentation.
- Le forward pass produit les logits utilisés par la loss.
- La rétropropagation applique la règle de dérivation en chaîne.
- Le gradient indique comment modifier les paramètres.
- L’optimiseur utilise le gradient pour réduire la loss.
- Le learning rate contrôle l’amplitude des mises à jour.
- Un batch produit une mise à jour ; une epoch parcourt tout le Train.
- La sortie et la loss doivent correspondre au type de problème.
- Les losses modernes reçoivent généralement directement les logits.
- Train et Validation doivent être suivis séparément.
- L’overfitting apparaît lorsque le Train progresse tandis que la Validation se dégrade.
- Tester la mémorisation d’un petit batch est un excellent outil de débogage.
