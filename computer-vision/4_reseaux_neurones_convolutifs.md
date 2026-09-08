<!--
Title: "Réseaux de neurones convolutifs"
Author: rsquaredata
Last updated: 2026-09-08
-->
# 4. Réseaux de neurones convolutifs

---

## 4.1. Pourquoi ne pas utiliser uniquement un réseau dense ?

Une image RGB de taille $224\times224$ contient :

$$
224\times224\times3 = 150\,528
$$

valeurs.

Si elle est aplatie puis connectée à une couche dense de 1 000 neurones, le nombre de poids est :

$$
150\,528\times1\,000 = 150\,528\,000
$$

En ajoutant les biais :

$$
150\,528\,000+1\,000 = 150\,529\,000
$$

paramètres.

Cette seule couche contient donc plus de 150 millions de paramètres.

### Problèmes

- coût mémoire élevé ;
- calcul important ;
- risque d’overfitting ;
- perte explicite de la structure spatiale ;
- position de chaque pixel traitée séparément ;
- faible réutilisation des motifs.

### Exemple

Un bord vertical peut apparaître à n’importe quel endroit.

Une couche dense doit apprendre séparément son utilité pour chaque position.

Un CNN utilise le même filtre dans toute l’image.

```text
Même motif
+
mêmes poids
+
toutes les positions
```

## 4.2. Principes d’un CNN

Un **Convolutional Neural Network** repose principalement sur :

### Connectivité locale

Chaque unité observe une petite région de l’entrée.

### Partage des poids

Le même kernel est appliqué à toutes les positions.

### Représentations hiérarchiques

Les couches successives combinent des motifs de complexité croissante.

### Réduction progressive de résolution

La taille spatiale diminue tandis que le nombre de canaux augmente souvent.

```text
Grande résolution
+
peu de canaux
↓
Petite résolution
+
nombreux canaux
```

## 4.3. Entrée d’une couche convolutive

Pour un batch d’images, la représentation PyTorch habituelle est :

$$
X\in\mathbb{R}^{N\times C_{\mathrm{in}}\times H_{\mathrm{in}}\times W_{\mathrm{in}}}
$$

avec :

- $N$ : batch size ;
- $C_{\mathrm{in}}$ : nombre de canaux d’entrée ;
- $H_{\mathrm{in}}$ : hauteur ;
- $W_{\mathrm{in}}$ : largeur.

Pour des images RGB :

$$
C_{\mathrm{in}}=3
$$

Exemple :

$$
X\in\mathbb{R}^{32\times3\times224\times224}
$$

correspond à un batch de 32 images RGB de taille $224\times224$.

## 4.4. Filtres convolutifs

Une couche possédant $C_{\mathrm{out}}$ filtres utilise un tenseur de poids :

$$
W
\in
\mathbb{R}^{
C_{\mathrm{out}}
\times
C_{\mathrm{in}}
\times
K_H
\times
K_W
}
$$

Chaque filtre traverse tous les canaux d’entrée et produit une feature map.

```text
C_in canaux
↓
Un filtre
↓
Une feature map
```

Avec $C_{\mathrm{out}}$ filtres :

```text
C_out feature maps
```

La sortie possède donc la forme :

$$
Y
\in
\mathbb{R}^{
N
\times
C_{\mathrm{out}}
\times
H_{\mathrm{out}}
\times
W_{\mathrm{out}}
}
$$

## 4.5. Calcul d’une activation convolutive

Pour une sortie au canal $c$, à la position $(i,j)$ :

$$
z_{c,i,j} =
b_c
+
\sum_{d=1}^{C_{\mathrm{in}}}
\sum_{u=0}^{K_H-1}
\sum_{v=0}^{K_W-1}
W_{c,d,u,v}
X_{d,iS_H+u-P_H,jS_W+v-P_W}
$$

avec :

- $W_{c,d,u,v}$ : poids du filtre ;
- $b_c$ : biais associé au filtre ;
- $S_H,S_W$ : strides ;
- $P_H,P_W$ : paddings.

Une activation non linéaire est ensuite appliquée :

$$
a_{c,i,j} = f(z_{c,i,j})
$$

Avec ReLU :

$$
a_{c,i,j} = \max(0,z_{c,i,j})
$$

## 4.6. Dimensions de sortie

Pour une convolution 2D :

$$
H_{\mathrm{out}} = \left\lfloor \frac{H_{\mathrm{in}} + 2P_H - D_H(K_H-1) - 1}{ S_H} \right\rfloor + 1
$$

$$
W_{\mathrm{out}} = \left\lfloor \frac{W_{\mathrm{in}} + 2P_W - D_W(K_W-1) - 1}{S_W}\right\rfloor + 1
$$

avec :

- $P$ : padding ;
- $S$ : stride ;
- $D$ : dilation ;
- $K$ : taille du kernel.

Sans dilation, $D=1$, donc :

$$
H_{\mathrm{out}} =
\left\lfloor
\frac{
H_{\mathrm{in}}+2P_H-K_H
}{
S_H
}
\right\rfloor
+
1
$$

### Exemple 1 — Taille conservée

Entrée :

$$
H_{\mathrm{in}}=W_{\mathrm{in}}=32
$$

Paramètres :

$$
K=3,\qquad P=1,\qquad S=1
$$

Alors :

$$
H_{\mathrm{out}} = \left\lfloor \frac{32+2-3}{1} \right\rfloor+1 = 32
$$

La sortie conserve une taille spatiale de :

$$
32\times32
$$

### Exemple 2 — Réduction par stride

Avec :

$$
K=3,\qquad P=1,\qquad S=2
$$

on obtient :

$$
H_{\mathrm{out}} = \left\lfloor \frac{32+2-3}{2} \right\rfloor+1 = 16
$$

La résolution devient :

$$
16\times16
$$

## 4.7. Nombre de paramètres

Une convolution standard contient $K_HK_WC_{\mathrm{in}}C_{\mathrm{out}}$ poids.

Avec un biais par filtre :

$$
N_{\mathrm{paramètres}} =
K_HK_WC_{\mathrm{in}}C_{\mathrm{out}}
+
C_{\mathrm{out}}
$$

### Exemple

Pour :

$$
K_H=K_W=3
$$

$$
C_{\mathrm{in}}=3
$$

$$
C_{\mathrm{out}}=64
$$

le nombre de paramètres est :

$$
3\times3\times3\times64+64 = 1\,792
$$

#### Point fondamental

Le nombre de paramètres ne dépend pas directement de la hauteur et de la largeur de l’image.

Le même kernel est réutilisé à toutes les positions.

## 4.8. Feature maps

Chaque filtre apprend à répondre à un motif particulier.

Dans les premières couches, ces motifs peuvent ressembler à :

- bords horizontaux ;
- bords verticaux ;
- diagonales ;
- transitions de couleur ;
- textures simples.

Dans les couches intermédiaires :

- coins ;
- courbes ;
- textures complexes ;
- motifs répétitifs ;
- parties d’objets.

Dans les couches profondes :

- roues ;
- yeux ;
- visages ;
- silhouettes ;
- objets complets.

```text
Pixels
↓
Bords et couleurs
↓
Textures et formes
↓
Parties d’objets
↓
Concepts visuels
```

#### ⚠️ Interprétation prudente

Cette hiérarchie est une intuition utile, mais tous les canaux ne correspondent pas nécessairement à un concept facilement nommable.

## 4.9. Bloc convolutif élémentaire

Un bloc simple peut être constitué de :

```text
Convolution
↓
Normalisation éventuelle
↓
Activation
↓
Pooling ou réduction éventuelle
```

Exemple :

```text
Conv2D
↓
BatchNorm
↓
ReLU
↓
MaxPool
```

L’ordre exact dépend de l’architecture.

## 4.10. Pooling

### Max Pooling

Pour une région $\mathcal{R}_{i,j}$ :

$$
y_{i,j} =
\max_{(u,v)\in\mathcal{R}_ {i,j}} x_{u,v}
$$

Il conserve l’activation la plus forte.

### Average Pooling

$$
y_{i,j} =
\frac{1}{|\mathcal{R}_ {i,j}|}
\sum_{(u,v)\in\mathcal{R}_ {i,j}}
x_{u,v}
$$

Il conserve la réponse moyenne.

### Effets

- réduction de la résolution ;
- réduction du coût de calcul ;
- agrégation locale ;
- augmentation du champ réceptif effectif ;
- perte d’information spatiale.

## 4.11. Convolution avec stride ou pooling ?

## Pooling

La règle de réduction est fixe :

```text
maximum ou moyenne
```

### Convolution avec stride

La réduction est réalisée par des filtres appris.

```text
Convolution stride 2
```

#### Comparaison

##### Pooling

- simple ;
- aucun paramètre appris ;
- comportement prédéfini.

##### Convolution avec stride

- transformation apprise ;
- davantage de paramètres ;
- réduction intégrée à la convolution.

Les architectures modernes utilisent souvent des convolutions avec stride, sans rendre le pooling inutile pour autant.

## 4.12. Champ réceptif

Le champ réceptif d’une unité est la région de l’entrée pouvant influencer cette unité.

Pour la couche $\ell$, si :

- $r_{\ell}$ est la taille du champ réceptif ;
- $j_{\ell}$ est le saut entre deux unités voisines dans l’image initiale ;
- $K_{\ell}$ est la taille du kernel ;
- $S_{\ell}$ est le stride ;

alors :

$$
j_{\ell} = j_{\ell-1}S_{\ell}
$$

et :

$$
r_{\ell} = r_{\ell-1} + (K_{\ell}-1)j_{\ell-1}
$$

Initialement :

$$
r_0=1
\qquad
j_0=1
$$

### Exemple

Première convolution $3\times3$, stride 1 :

$$
r_1 = 1+(3-1)\times1 = 3
$$

Deuxième convolution $3\times3$, stride 1 :

$$
r_2 = 3+(3-1)\times1 = 5
$$

Deux convolutions $3\times3$ successives produisent donc un champ réceptif de :

$$
5\times5
$$

## 4.13. Pourquoi empiler de petits kernels ?

Une convolution $5\times5$ possède, par paire de canaux $5\times5=25$ poids.

Deux convolutions $3\times3$ possèdent $2\times3\times3=18$ poids par chemin comparable entre canaux.

Elles introduisent aussi deux activations non linéaires au lieu d’une.

#### Avantages

- moins de paramètres dans de nombreuses configurations ;
- davantage de non-linéarité ;
- représentations intermédiaires ;
- profondeur accrue.

## 4.14. Padding et informations de bord

Avec un kernel impair et un stride 1, un padding courant est :

$$
P = \frac{K-1}{2}
$$

Pour $K=3$ :

$$
P=1
$$

Pour $K=5$ :

$$
P=2
$$

Cela conserve la taille spatiale.

### Limite du zero padding

Les zéros ajoutés ne proviennent pas de l’image.

Ils peuvent créer des effets artificiels près des bords.

Selon le problème, on peut utiliser :

- zero padding ;
- reflection padding ;
- replication padding ;
- circular padding.

## 4.15. Équivariance à la translation

Soit $T_{\Delta}$ une translation de l’entrée et $f$ une convolution.

Idéalement :

$$
f(T_{\Delta}X) = T_{\Delta}f(X)
$$

Cela signifie qu’un motif déplacé dans l’entrée produit une activation déplacée dans la feature map.

### Équivariance ≠ invariance

#### Équivariance

La représentation se déplace avec l’objet.

#### Invariance

La prédiction reste identique malgré le déplacement.

L’invariance partielle est construite grâce à :

- pooling ;
- réduction spatiale ;
- Global Average Pooling ;
- data augmentation ;
- profondeur du réseau.

#### ⚠️ Limite pratique

Stride, padding, pooling et effets de bord empêchent une équivariance parfaite dans toutes les situations.

## 4.16. Global Average Pooling

Pour une feature map $A_c$ de taille $H\times W$ :

$$
g_c =
\frac{1}{HW}
\sum_{i=1}^{H}
\sum_{j=1}^{W}
A_{c,i,j}
$$

Chaque canal est réduit à une valeur.

```text
N × C × H × W
↓
Global Average Pooling
↓
N × C
```

### Avantages

- très peu de paramètres ;
- réduction du risque d’overfitting ;
- compatible avec différentes tailles spatiales dans certaines architectures ;
- remplace de grandes couches denses.

## 4.17. Flatten

L’opération Flatten transforme les dimensions spatiales et les canaux en un seul vecteur.

```text
N × C × H × W
↓
Flatten
↓
N × (C × H × W)
```

### Exemple

Une sortie :

$$
64\times7\times7
$$

devient un vecteur de taille :

$$
64\times7\times7 = 3\,136
$$

Si ce vecteur est connecté à 1 024 neurones, la couche dense possède :

$$
3\,136\times1\,024+1\,024 = 3\,212\,288
$$

paramètres.

#### Conséquence

Une grande partie des paramètres d’un ancien CNN peut se trouver dans les couches denses finales.

Le Global Average Pooling permet souvent de réduire ce coût.

## 4.18. Tête de classification

Une architecture de classification comporte généralement deux parties.

## #Backbone

Extrait les caractéristiques visuelles.

```text
Image
↓
Blocs convolutifs
↓
Représentation
```

### Classification Head

Transforme la représentation en logits.

```text
Représentation
↓
Pooling ou Flatten
↓
Couche dense
↓
K logits
```

Pour $K$ classes :

$$
\mathbf{z}\in\mathbb{R}^{K}
$$

La probabilité de la classe $k$ est :

$$
p_k =
\frac{e^{z_k}}
{\sum_{j=1}^{K}e^{z_j}}
$$

## 4.19. Architecture CNN simple

Pour une classification à dix classes :

```text
Entrée RGB : 3 × 32 × 32
↓
Conv 3×3, 32 filtres, padding 1
↓
ReLU
↓
MaxPool 2×2
↓
32 × 16 × 16
↓
Conv 3×3, 64 filtres, padding 1
↓
ReLU
↓
MaxPool 2×2
↓
64 × 8 × 8
↓
Global Average Pooling
↓
64
↓
Linear(64 → 10)
↓
10 logits
```

## 4.20. Implémentation PyTorch

```python
import torch
from torch import nn


class SimpleCNN(nn.Module):
    def __init__(self, num_classes: int):
        super().__init__()

        self.features = nn.Sequential(
            nn.Conv2d(
                in_channels=3,
                out_channels=32,
                kernel_size=3,
                padding=1,
            ),
            nn.ReLU(),
            nn.MaxPool2d(kernel_size=2),

            nn.Conv2d(
                in_channels=32,
                out_channels=64,
                kernel_size=3,
                padding=1,
            ),
            nn.ReLU(),
            nn.MaxPool2d(kernel_size=2),
        )

        self.pool = nn.AdaptiveAvgPool2d((1, 1))
        self.classifier = nn.Linear(64, num_classes)

    def forward(self, x):
        x = self.features(x)
        x = self.pool(x)
        x = torch.flatten(x, start_dim=1)
        logits = self.classifier(x)
        return logits
```

Pour un batch :

$$
X\in\mathbb{R}^{N\times3\times32\times32}
$$

la sortie possède la forme :

$$
Z\in\mathbb{R}^{N\times K}
$$

où $K$ est le nombre de classes.

## 4.21. Vérifier les dimensions

Pour l’architecture précédente :

### Entrée

$$
N\times3\times32\times32
$$

### Première convolution

$$
N\times32\times32\times32
$$

### Premier pooling

$$
N\times32\times16\times16
$$

### Deuxième convolution

$$
N\times64\times16\times16
$$

### Deuxième pooling

$$
N\times64\times8\times8
$$

### Global Average Pooling

$$
N\times64\times1\times1
$$

### Flatten

$$
N\times64
$$

### Couche finale

$$
N\times K
$$

#### 🧠 Réflexe

Après chaque couche, suivre explicitement :

$$
C\times H\times W
$$

Cela évite une grande partie des erreurs d’architecture.

## 4.22. Convolution $1\times1$

Une convolution $1\times1$ observe une position spatiale à la fois, mais combine les canaux.

Pour une entrée de $C_{\mathrm{in}}$ canaux et $C_{\mathrm{out}}$ filtres :

$$
N_{\mathrm{paramètres}} =
C_{\mathrm{in}}C_{\mathrm{out}}
+
C_{\mathrm{out}}
$$

### Utilisations

- réduire le nombre de canaux ;
- augmenter le nombre de canaux ;
- combiner les informations entre canaux ;
- créer des bottlenecks ;
- adapter les dimensions d’une connexion résiduelle.

#### Exemple

Passer de 256 à 64 canaux :

$$
256\times64+64 = 16\,448
$$

paramètres.

## 4.23. Convolution dilatée

La dilation espace les positions échantillonnées par le kernel.

La taille effective d’un kernel est :

$$
K_{\mathrm{eff}} = D(K-1)+1
$$

Pour :

$$
K=3
\qquad
D=2
$$

on obtient :

$$
K_{\mathrm{eff}} = 2(3-1)+1 = 5
$$

Le filtre possède toujours neuf poids par paire de canaux, mais couvre une région équivalente à $5\times5$.

### Intérêt

- augmenter le champ réceptif ;
- conserver la résolution ;
- utile notamment en segmentation.

### Limite

Une forte dilation peut produire un échantillonnage trop dispersé et manquer certains motifs locaux.

## 4.24. Convolution depthwise separable

Une convolution standard coûte approximativement :

$$
K^2C_{\mathrm{in}}C_{\mathrm{out}}
$$

multiplications par position.

Une convolution séparable décompose l’opération en deux étapes.

### Depthwise Convolution

Un filtre spatial est appliqué séparément à chaque canal.

Coût :

$$
K^2C_{\mathrm{in}}
$$

### Pointwise Convolution

Une convolution $1\times1$ combine les canaux.

Coût :

$$
C_{\mathrm{in}}C_{\mathrm{out}}
$$

### Coût total

$$
K^2C_{\mathrm{in}}
+
C_{\mathrm{in}}C_{\mathrm{out}}
$$

au lieu de :

$$
K^2C_{\mathrm{in}}C_{\mathrm{out}}
$$

### Intérêt

- modèle plus léger ;
- calcul réduit ;
- utile sur mobile et systèmes embarqués.

#### Limite

La réduction de calcul peut parfois diminuer la capacité du modèle.

## 4.25. Batch Normalization dans un CNN

Pour un canal donné, Batch Normalization normalise les activations à partir de statistiques calculées sur le mini-batch.

Pour une activation $x$ :

$$
\widehat{x} =
\frac{x-\mu_{\mathcal{B}}}
{\sqrt{\sigma_{\mathcal{B}}^2+\varepsilon}}
$$

puis :

$$
y = \gamma\widehat{x} + \beta
$$

avec :

- $\mu_{\mathcal{B}}$ : moyenne du batch ;
- $\sigma_{\mathcal{B}}^2$ : variance du batch ;
- $\gamma,\beta$ : paramètres appris.

### Intérêts

- stabiliser les activations ;
- faciliter l’optimisation ;
- permettre parfois un learning rate plus élevé ;
- introduire un léger effet régularisant.

### Mode évaluation

En évaluation, Batch Normalization utilise des statistiques accumulées pendant l’entraînement.

D’où l’importance de :

```python
model.train()
```

et :

```python
model.eval()
```

#### ⚠️ Petit batch

Avec un batch très petit, les statistiques peuvent devenir instables.

D’autres normalisations peuvent être utilisées :

- Layer Normalization ;
- Group Normalization ;
- Instance Normalization.

## 4.26. Dropout dans un CNN

Pendant l’entraînement, Dropout annule aléatoirement certaines activations.

Avec une probabilité d’annulation $p$ :

$$
m_i
\sim
\mathrm{Bernoulli}(1-p)
$$

et :

$$
\widetilde{a}_i =
\frac{m_i}{1-p}a_i
$$

### Objectif

Réduire la dépendance excessive entre neurones et limiter l’overfitting.

### En évaluation

Dropout est désactivé.

#### Usage

Dans les CNN modernes, Dropout est souvent utilisé :

- dans la tête de classification ;
- dans certains blocs spécifiques ;
- lorsque l’overfitting est important.

Il n’est pas obligatoire après chaque convolution.

## 4.27. Softmax et Cross-Entropy

Pour une classification multiclasse, la dernière couche produit des logits :

$$
\mathbf{z} =
\begin{bmatrix}
z_1 & \cdots & z_K
\end{bmatrix}
$$

La Cross-Entropy pour la classe réelle $y$ est :

$$
\mathcal{L} =
-\log
\left(
\frac{e^{z_y}}
{\sum_{j=1}^{K}e^{z_j}}
\right)
$$

Sous une forme numériquement stable :

$$
\mathcal{L} =
-z_y
+
\log
\left(
\sum_{j=1}^{K}e^{z_j}
\right)
$$

#### ⚠️ PyTorch

Avec :

```python
nn.CrossEntropyLoss()
```

le modèle doit produire directement les logits.

Ne pas appliquer Softmax dans `forward()` avant cette loss.

## 4.28. Initialisation et apprentissage des kernels

Au début, les kernels contiennent des valeurs initialisées aléatoirement.

Pendant l’entraînement :

```text
Images
↓
Convolutions
↓
Logits
↓
Loss
↓
Backpropagation
↓
Gradients des kernels
↓
Mise à jour
```

Pour un poids $w$ d’un kernel :

$$
w_{t+1} = w_t - \eta \frac{\partial\mathcal{L}}{\partial w_t}
$$

Les filtres ne sont donc pas définis manuellement : ils sont appris pour réduire la loss finale.

## 4.29. Visualiser les filtres et activations

### Filtres de première couche

Ils peuvent parfois être visualisés directement comme de petites images.

Ils apprennent souvent :

- orientations ;
- contrastes ;
- couleurs ;
- motifs simples.

### Feature maps

Visualiser les feature maps permet d’observer les régions qui activent certains canaux.

### Limite

Une activation forte ne fournit pas toujours une explication complète.

Pour analyser une décision, on peut également utiliser :

- saliency maps ;
- Grad-CAM ;
- occlusion ;
- méthodes d’attribution.

Ces méthodes sont étudiées dans le chapitre sur l’explicabilité.

## 4.30. Erreurs fréquentes

### Mauvais nombre de canaux

Le modèle attend :

$$
C_{\mathrm{in}}=3
$$

mais reçoit une image en niveaux de gris :

$$
C_{\mathrm{in}}=1
$$

### Mauvais ordre des dimensions

```text
N × H × W × C
```

fourni à une couche attendant :

```text
N × C × H × W
```

### Mauvaise taille après Flatten

La dimension passée à la couche dense ne correspond pas à la sortie réelle du backbone.

### Softmax appliquée deux fois

Softmax est appliquée dans le modèle puis de nouveau implicitement par la loss.

### Images entières

Une conversion entière empêche une normalisation et certaines opérations attendues en virgule flottante.

### Résolutions incohérentes

Les images d’un batch n’ont pas la même taille.

### Oubli du mode Evaluation

Dropout et Batch Normalization se comportent comme pendant l’entraînement.

## 4.31. Diagnostiquer un CNN

### Vérifier une propagation avant

Avant l’entraînement :

1. créer un petit batch ;
2. l’envoyer dans le modèle ;
3. vérifier la forme des logits ;
4. vérifier que les valeurs sont finies.

```python
X = torch.randn(4, 3, 32, 32)
logits = model(X)

print(logits.shape)
print(torch.isfinite(logits).all())
```

Pour dix classes, la forme attendue est :

```text
torch.Size([4, 10])
```

### Vérifier le nombre de paramètres

```python
num_parameters = sum(
    p.numel()
    for p in model.parameters()
    if p.requires_grad
)
```

### Overfit d’un petit batch

Le CNN doit pouvoir presque mémoriser un petit batch.

S’il n’y parvient pas, vérifier :

- labels ;
- dimensions ;
- loss ;
- gradients ;
- learning rate ;
- normalisation ;
- mise à jour des paramètres.

## 4.32. 🧠 Réflexe — Construire un CNN simple

```text
Entrée N × C × H × W
↓
Convolution
↓
Activation
↓
Réduction spatiale
↓
Répéter plusieurs blocs
↓
Global Average Pooling
↓
Couche de sortie
↓
Logits
```

À chaque étape, noter :

$$
C\times H\times W
$$

Puis vérifier :

- dimensions ;
- nombre de paramètres ;
- champ réceptif ;
- perte de résolution ;
- cohérence avec la tâche.

## 4.33. 🧠 Réflexe — Résolution et sémantique

Dans un CNN de classification, on observe souvent :

```text
Profondeur ↑
↓
Résolution spatiale ↓
Nombre de canaux ↑
Champ réceptif ↑
Niveau sémantique ↑
```

### Première partie du réseau

- position précise ;
- bords ;
- textures ;
- détails locaux.

### Partie profonde

- contexte ;
- formes complexes ;
- catégories ;
- concepts plus abstraits.

#### Conséquence

Pour une classification globale, une forte réduction spatiale est acceptable.

Pour une segmentation ou une détection précise, il faut préserver ou reconstruire l’information spatiale.

## 4.34. Exercices

### Exercice 1 — Dimensions

Entrée :

$$
64\times64
$$

Convolution :

$$
K=5,\qquad P=2,\qquad S=1
$$

Sortie :

$$
\left\lfloor \frac{64+4-5}{1} \right\rfloor+1 = 64
$$

La résolution est conservée.

### Exercice 2 — Stride

Entrée :

$$
64\times64
$$

Convolution :

$$
K=3,\qquad P=1,\qquad S=2
$$

Sortie :

$$
\left\lfloor \frac{64+2-3}{2} \right\rfloor+1 = 32
$$

### Exercice 3 — Paramètres

Convolution :

$$
C_{\mathrm{in}}=32
$$

$$
C_{\mathrm{out}}=64
$$

$$
K=3
$$

Avec biais :

$$
3\times3\times32\times64+64 = 18\,496
$$

paramètres.

### Exercice 4 — Forme complète

Entrée :

$$
N\times3\times128\times128
$$

Couche :

```text
Conv2D(3 → 32, kernel=3, padding=1)
```

Sortie :

$$
N\times32\times128\times128
$$

Après Max Pooling $2\times2$ :

$$
N\times32\times64\times64
$$

### Exercice 5 — Global Average Pooling

Entrée :

$$
N\times256\times7\times7
$$

Après Global Average Pooling :

$$
N\times256\times1\times1
$$

Après Flatten :

$$
N\times256
$$

---

## 4.35. À retenir

- Un CNN exploite la structure spatiale des images.
- La connectivité locale et le partage des poids réduisent le nombre de paramètres.
- Chaque filtre produit une feature map.
- Le nombre de filtres détermine le nombre de canaux de sortie.
- Kernel, padding, stride et dilation déterminent les dimensions spatiales.
- Les couches profondes possèdent un champ réceptif plus large.
- La convolution est principalement équivariante à la translation.
- Pooling et stride réduisent la résolution, mais peuvent supprimer des détails.
- Global Average Pooling remplace avantageusement de grandes couches denses dans de nombreux classifieurs.
- Les convolutions $1\times1$ combinent les canaux à faible coût.
- Les convolutions séparables réduisent fortement le coût de calcul.
- Batch Normalization facilite souvent l’optimisation.
- Les logits doivent être transmis directement à la Cross-Entropy.
- Le suivi explicite des formes $C\times H\times W$ est indispensable.
