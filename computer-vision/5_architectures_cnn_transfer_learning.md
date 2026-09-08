<!--
Title: "Architectures CNN et Transfer Learning"
Author: rsquaredata
Last updated: 2026-09-08
-->
# 5. Architectures CNN et Transfer Learning

---

## 5.1. Pourquoi étudier plusieurs architectures ?

Une architecture CNN détermine notamment :

- le nombre de couches ;
- la largeur des couches ;
- les tailles de kernels ;
- les réductions de résolution ;
- les connexions entre couches ;
- le coût de calcul ;
- le nombre de paramètres ;
- la facilité d’optimisation.

L’histoire des CNN correspond à une succession de réponses à plusieurs difficultés :

```text
Comment reconnaître des motifs complexes ?
↓
Comment entraîner des réseaux plus profonds ?
↓
Comment limiter le nombre de paramètres ?
↓
Comment préserver et réutiliser l’information ?
↓
Comment réduire le coût de calcul ?
```

L’objectif n’est pas de mémoriser toutes les couches de chaque architecture.

Il faut surtout retenir **l’idée nouvelle introduite par chacune**.

## 5.2. Backbone et tête de prédiction

Une architecture de vision est souvent séparée en deux parties.

### Backbone

Le backbone extrait une représentation visuelle.

```text
Image
↓
Blocs convolutifs
↓
Feature maps
↓
Représentation visuelle
```

### Head

La tête transforme cette représentation en sortie adaptée à la tâche.

#### Classification

```text
Backbone
↓
Global Average Pooling
↓
Couche dense
↓
Classes
```

#### Détection

```text
Backbone
↓
Detection Head
↓
Classes + bounding boxes
```

#### Segmentation

```text
Backbone
↓
Segmentation Head
↓
Masque par pixel
```

#### 🧠 Idée fondamentale

Un même backbone peut être réutilisé pour différentes tâches en remplaçant la tête.

## 5.3. LeNet-5

LeNet-5 est une architecture historique conçue pour reconnaître des chiffres manuscrits.

Pipeline simplifié :

```text
Image
↓
Convolution
↓
Pooling
↓
Convolution
↓
Pooling
↓
Couches denses
↓
Classe
```

### Contributions importantes

- utilisation de convolutions ;
- partage des poids ;
- extraction hiérarchique de caractéristiques ;
- réduction progressive de la résolution.

### Limites

- réseau peu profond ;
- conçu pour de petites images simples ;
- capacité insuffisante pour de grands problèmes modernes.

#### À retenir

LeNet montre la structure fondamentale d’un CNN de classification.

## 5.4. AlexNet

AlexNet a marqué le retour des réseaux convolutifs profonds à grande échelle.

### Idées importantes

- entraînement sur GPU ;
- activation ReLU ;
- data augmentation ;
- Dropout ;
- réseau plus profond et plus large ;
- entraînement sur un grand dataset.

### Structure générale

```text
Image
↓
Plusieurs convolutions
↓
Pooling
↓
Couches denses
↓
Softmax
```

### Limites

- grandes couches denses ;
- nombre élevé de paramètres ;
- architecture aujourd’hui peu efficace ;
- premiers kernels relativement grands.

#### À retenir

AlexNet a montré qu’un CNN profond entraîné sur beaucoup de données et avec suffisamment de calcul pouvait dépasser les méthodes classiques fondées sur des features manuelles.

## 5.5. VGG

VGG adopte une architecture régulière construite principalement avec de petits kernels :

$$
3\times3
$$

### Principe

```text
Plusieurs convolutions 3×3
↓
Pooling
↓
Nombre de canaux augmenté
↓
Répéter
```

### Pourquoi empiler des convolutions $\(3\times3\)$ ?

Deux convolutions $\(3\times3\)$ successives produisent un champ réceptif de \(5\times5\).

Trois convolutions $\(3\times3\)$ produisent un champ réceptif de \(7\times7\).

Cette stratégie apporte :

- davantage de non-linéarités ;
- une architecture régulière ;
- des représentations intermédiaires ;
- moins de paramètres qu’un unique grand kernel dans de nombreux cas.

### VGG-16 et VGG-19

Le nombre indique le nombre de couches possédant des paramètres.

#### Avantages

- architecture simple ;
- facile à comprendre ;
- représentations visuelles utiles ;
- importante historiquement pour le transfer learning.

#### Limites

- très grand nombre de paramètres ;
- consommation mémoire élevée ;
- calcul coûteux ;
- grandes couches denses finales.

#### À retenir

VGG démontre l’intérêt d’empiler de petites convolutions dans une architecture profonde et régulière.

## 5.6. Inception

Une image contient des motifs à différentes échelles.

Un petit kernel détecte des détails locaux, tandis qu’un kernel plus grand observe davantage de contexte.

### Module Inception

Plusieurs transformations sont appliquées en parallèle :

```text
Entrée
├── Convolution 1×1
├── Convolution 3×3
├── Convolution 5×5
└── Pooling
↓
Concaténation des sorties
```

Mathématiquement :

$$
Y =
\mathrm{Concat}
\left(
f_{1\times1}(X),
f_{3\times3}(X),
f_{5\times5}(X),
f_{\mathrm{pool}}(X)
\right)
$$

### Convolutions $\(1\times1\)$

Elles réduisent le nombre de canaux avant les opérations coûteuses.

Sans réduction, une convolution possède approximativement :

$$
K^2C_{\mathrm{in}}C_{\mathrm{out}}
$$

paramètres.

Une projection \(1\times1\) réduit d’abord \(C_{\mathrm{in}}\) vers une dimension plus petite \(C_r\).

Le coût devient approximativement :

$$
C_{\mathrm{in}}C_r
+
K^2C_rC_{\mathrm{out}}
$$

### Avantages

- traitement multi-échelle ;
- calcul mieux contrôlé ;
- réutilisation des convolutions \(1\times1\).

### Limite

L’architecture est plus complexe à concevoir et à implémenter que VGG.

#### À retenir

Inception traite simultanément l’image à plusieurs échelles et utilise des convolutions \(1\times1\) pour limiter le coût.

## 5.7. Pourquoi les réseaux très profonds sont-ils difficiles à entraîner ?

On pourrait penser que l’ajout de couches améliore toujours les performances.

En pratique, un réseau profond peut devenir plus difficile à optimiser.

### Problème de dégradation

Un réseau plus profond peut obtenir une erreur d’entraînement supérieure à celle d’un réseau plus court.

Ce phénomène ne provient pas nécessairement de l’overfitting.

Le réseau devrait théoriquement pouvoir apprendre des couches identité :

$$
H(\mathbf{x})=\mathbf{x}
$$

mais cette identité peut être difficile à optimiser directement.

## 5.8. ResNet

ResNet introduit les **connexions résiduelles**, ou skip connections.

Au lieu d’apprendre directement une transformation $\(H(\mathbf{x})\)$, le bloc apprend un résidu :

$$
F(\mathbf{x}) = H(\mathbf{x})-\mathbf{x}
$$

La sortie devient :

$$
\mathbf{y} = F(\mathbf{x})+\mathbf{x}
$$

### Schéma

```text
x ───────────────────────────────────────────┐
│                                            │
└──→ Convolution → BatchNorm → ReLU          │
                         ↓                   │
                    Convolution → BatchNorm  │
                         ↓                   │
                         + ←─────────────────┘
                         ↓
                       ReLU
                         ↓
                       Sortie
```

Si les dimensions changent :

```text
x ──→ Convolution 1×1 / stride ──────────────┐
│                                            │
└──→ Convolution → BatchNorm → ReLU          │
                         ↓                   │
                    Convolution → BatchNorm  │
                         ↓                   │
                         + ←─────────────────┘
                         ↓
                       ReLU
                         ↓
                       Sortie
```

### Pourquoi cela aide-t-il ?

Si la meilleure transformation est proche de l’identité, il suffit d’apprendre :

$$
F(\mathbf{x})\approx0
$$

La sortie devient alors :

$$
\mathbf{y}\approx\mathbf{x}
$$

Les connexions courtes facilitent également la circulation :

- de l’information ;
- des gradients.

### Gradient à travers un bloc résiduel

Si :

$$
\mathbf{y} = F(\mathbf{x})+\mathbf{x}
$$

alors :

$$
\frac{\partial\mathbf{y}}{\partial\mathbf{x}} = \frac{\partial F(\mathbf{x})}{\partial\mathbf{x}} + I
$$

Le terme identité $\(I\)$ fournit un chemin direct au gradient.

## 5.9. Changement de dimensions dans ResNet

L’addition exige des tenseurs compatibles.

Si :

$$
F(\mathbf{x})
\in
\mathbb{R}^{C_{\mathrm{out}}\times H_{\mathrm{out}}\times W_{\mathrm{out}}}
$$

mais :

$$
\mathbf{x}
\in
\mathbb{R}^{C_{\mathrm{in}}\times H_{\mathrm{in}}\times W_{\mathrm{in}}}
$$

une projection peut être appliquée :

$$
\mathbf{y} = F(\mathbf{x}) + W_s\mathbf{x}
$$

La projection $\(W_s\)$ utilise souvent une convolution $\(1\times1\)$, éventuellement avec stride.

### Blocs Basic et Bottleneck

#### Basic Block

Utilisé notamment dans ResNet-18 et ResNet-34 :

```text
3×3
↓
3×3
+
connexion résiduelle
```

#### Bottleneck Block

Utilisé dans les ResNet plus profonds :

```text
1×1 → réduction des canaux
↓
3×3 → traitement spatial
↓
1×1 → restauration des canaux
+
connexion résiduelle
```

#### À retenir

ResNet est une architecture fondamentale parce que les connexions résiduelles permettent d’entraîner efficacement des réseaux beaucoup plus profonds.

## 5.10. DenseNet

DenseNet pousse plus loin l’idée de réutilisation des représentations.

Chaque couche reçoit les sorties de toutes les couches précédentes.

Pour la couche $\(\ell\)$ :

$$
\mathbf{x}_ {\ell} =
H_{\ell}
\left(
[\mathbf{x}_0,\mathbf{x}_ 1,\ldots,\mathbf{x}_{\ell-1}]
\right)
$$

où $\([\cdot]\)$ représente une concaténation selon les canaux.

### Différence avec ResNet

#### ResNet

Addition :

$$
\mathbf{y} = F(\mathbf{x})+\mathbf{x}
$$

#### DenseNet

Concaténation :

$$
\mathbf{y} = [\mathbf{x},F(\mathbf{x})]
$$

### Avantages

- réutilisation des features ;
- circulation facilitée des gradients ;
- chaque couche peut accéder aux représentations antérieures ;
- efficacité en nombre de paramètres dans certaines configurations.

### Limites

- nombreuses concaténations ;
- consommation mémoire importante ;
- implémentation plus complexe.

#### À retenir

ResNet additionne les représentations ; DenseNet les concatène.

## 5.11. MobileNet

MobileNet vise les environnements contraints :

- téléphone ;
- caméra embarquée ;
- edge computing ;
- robot ;
- appareil à faible consommation.

### Convolution standard

Coût approximatif :

$$
K^2C_{\mathrm{in}}C_{\mathrm{out}}HW
$$

### Convolution depthwise

Un filtre spatial est appliqué séparément à chaque canal.

Coût :

$$
K^2C_{\mathrm{in}}HW
$$

### Convolution pointwise

Une convolution $\(1\times1\)$ combine ensuite les canaux.

Coût :

$$
C_{\mathrm{in}}C_{\mathrm{out}}HW
$$

### Coût total

$$
K^2C_{\mathrm{in}}HW
+
C_{\mathrm{in}}C_{\mathrm{out}}HW
$$

Cette décomposition réduit fortement le calcul.

### Compromis

```text
Précision
↔
Latence
↔
Mémoire
↔
Consommation énergétique
```

#### À retenir

MobileNet remplace une convolution standard par une convolution depthwise suivie d’une convolution pointwise.

## 5.12. EfficientNet

Augmenter la capacité d’un CNN peut signifier :

- augmenter sa profondeur ;
- augmenter sa largeur ;
- augmenter la résolution d’entrée.

### Scaling séparé

#### Profondeur

Davantage de couches.

#### Largeur

Davantage de canaux.

#### Résolution

Images plus grandes.

### Compound Scaling

EfficientNet fait évoluer ces trois dimensions conjointement.

Pour un coefficient global $\(\phi\)$ :

$$
\text{profondeur} = \alpha^{\phi}
$$

$$
\text{largeur} = \beta^{\phi}
$$

$$
\text{résolution} = \gamma^{\phi}
$$

avec une contrainte approximative :

$$
\alpha\beta^2\gamma^2 \approx 2
$$

L’objectif est d’obtenir une croissance équilibrée du coût de calcul.

### Avantages

- bon compromis entre précision et efficacité ;
- plusieurs tailles de modèles ;
- transfer learning performant.

#### À retenir

EfficientNet augmente de manière coordonnée la profondeur, la largeur et la résolution.

## 5.13. ConvNeXt

ConvNeXt modernise un CNN en réutilisant plusieurs choix de conception popularisés par les Vision Transformers, tout en conservant des convolutions.

Évolutions importantes :

- blocs résiduels ;
- convolutions depthwise ;
- kernels plus grands ;
- normalisation adaptée ;
- architecture organisée en stages ;
- fonctions d’activation modernes ;
- réduction du nombre d’activations et de normalisations inutiles.

#1# Intérêt

ConvNeXt montre que les CNN restent compétitifs lorsqu’ils bénéficient de choix d’architecture et d’entraînement modernes.

##1# À retenir

L’opposition n’est pas simplement :

```text
CNN ancien
vs
Transformer moderne
```

Des CNN modernisés restent très performants.

# 14. Comparaison synthétique

| Architecture | Idée principale | Limite principale |
|---|---|---|
| LeNet | Pipeline CNN fondamental | Faible capacité |
| AlexNet | CNN profond entraîné sur GPU | Architecture lourde |
| VGG | Empilement régulier de $\(3\times3\)$ | Beaucoup de paramètres |
| Inception | Traitement multi-échelle | Architecture complexe |
| ResNet | Connexions résiduelles | Coût des variantes profondes |
| DenseNet | Concaténation des features | Consommation mémoire |
| MobileNet | Convolutions séparables | Compromis précision-capacité |
| EfficientNet | Scaling composé | Entraînement et réglage sophistiqués |
| ConvNeXt | CNN modernisé | Modèles parfois coûteux |

## 5.15. Transfer Learning

Le **Transfer Learning** consiste à réutiliser les connaissances apprises sur une première tâche pour résoudre une nouvelle tâche.

```text
Grand dataset source
↓
Pré-entraînement
↓
Poids appris
↓
Nouvelle tâche
↓
Adaptation
```

### Exemple

Un réseau pré-entraîné sur un grand dataset apprend des représentations générales :

- contours ;
- textures ;
- couleurs ;
- formes ;
- parties d’objets.

Ces représentations peuvent être réutilisées pour :

- classer des espèces ;
- reconnaître des produits ;
- détecter des défauts ;
- analyser des images médicales.

## 5.16. Pourquoi le Transfer Learning fonctionne-t-il ?

Les premières couches apprennent souvent des motifs relativement généraux.

Les couches profondes deviennent plus spécifiques à la tâche source.

```text
Premières couches
→ caractéristiques générales

Couches intermédiaires
→ textures et formes

Couches profondes
→ concepts spécifiques
```

On peut donc conserver une partie du réseau et adapter les couches les plus spécialisées.

## 5.17. Stratégie 1 — Feature Extraction

Le backbone est gelé :

$$
\theta_{\mathrm{backbone}} = \text{constant}
$$

Seule la nouvelle tête est entraînée :

$$
\theta_{\mathrm{head}} \leftarrow \theta_{\mathrm{head}} - \eta \nabla_{\theta_{\mathrm{head}}} \mathcal{L}
$$

### Pipeline

```text
Backbone pré-entraîné gelé
↓
Features
↓
Nouvelle tête entraînable
↓
Classes de la nouvelle tâche
```

### Avantages

- entraînement rapide ;
- peu de mémoire ;
- risque d’overfitting réduit ;
- adapté aux petits datasets.

### Limite

Le backbone ne s’adapte pas au nouveau domaine.

## 5.18. Stratégie 2 — Fine-Tuning

Le fine-tuning met à jour une partie ou la totalité du backbone.

$$
\theta_{\mathrm{backbone}} \leftarrow \theta_{\mathrm{backbone}} - \eta_{\mathrm{backbone}} \nabla_{\theta_{\mathrm{backbone}}}
\mathcal{L}
$$

La tête est également entraînée :

$$
\theta_{\mathrm{head}} \leftarrow \theta_{\mathrm{head}} -
\eta_{\mathrm{head}}
\nabla_{\theta_{\mathrm{head}}}
\mathcal{L}
$$

On utilise souvent :

$$
\eta_{\mathrm{backbone}}
<
\eta_{\mathrm{head}}
$$

afin de modifier prudemment les représentations pré-entraînées.

## 5.19. Fine-tuning progressif

Une stratégie fréquente :

### Étape 1

- charger le modèle pré-entraîné ;
- remplacer la tête ;
- geler le backbone ;
- entraîner la nouvelle tête.

### Étape 2

- dégeler les derniers blocs ;
- utiliser un faible learning rate ;
- poursuivre l’entraînement.

### Étape 3 éventuelle

- dégeler davantage de couches ;
- diminuer encore le learning rate ;
- surveiller étroitement la validation.

```text
Tête
↓
Derniers blocs
↓
Backbone plus complet
```

## 5.20. Discriminative Learning Rates

Toutes les couches ne doivent pas nécessairement utiliser le même learning rate.

Exemple :

$$
\eta_{\mathrm{premières\ couches}}
<
\eta_{\mathrm{dernières\ couches}}
<
\eta_{\mathrm{head}}
$$

Les premières couches sont modifiées prudemment, tandis que la nouvelle tête apprend plus rapidement.

## 5.21. Quand geler ou fine-tuner ?

Deux critères sont particulièrement importants :

- quantité de données ;
- similarité entre domaine source et domaine cible.

### Peu de données et domaine similaire

```text
Geler le backbone
+
entraîner la tête
```

### Davantage de données et domaine similaire

```text
Fine-tuner les derniers blocs
```

### Peu de données et domaine très différent

Situation difficile.

Approches possibles :

- commencer par un backbone gelé ;
- dégeler progressivement ;
- forte régularisation ;
- data augmentation ;
- pré-entraînement plus proche du domaine.

### Beaucoup de données et domaine différent

```text
Fine-tuning étendu
```

voire entraînement depuis zéro si le volume et les ressources sont suffisants.

## 5.22. Remplacer la tête de classification

Supposons un réseau pré-entraîné sur 1 000 classes.

Sa dernière couche produit :

$$
\mathbf{z}\in\mathbb{R}^{1000}
$$

Pour une nouvelle tâche à $\(K\)$ classes, elle est remplacée par :

$$
W_{\mathrm{new}}
\in
\mathbb{R}^{K\times d}
$$

et :

$$
\mathbf{b}_{\mathrm{new}}
\in
\mathbb{R}^{K}
$$

La nouvelle sortie est :

$$
\mathbf{z}_ {\mathrm{new}} =
W_{\mathrm{new}}\mathbf{h}
+
\mathbf{b}_{\mathrm{new}}
$$

avec $\(\mathbf{h}\)$ la représentation produite par le backbone.

## 5.23. Exemple PyTorch

```python
from torch import nn
from torchvision.models import (
    resnet18,
    ResNet18_Weights,
)


weights = ResNet18_Weights.DEFAULT
model = resnet18(weights=weights)

for parameter in model.parameters():
    parameter.requires_grad = False

num_features = model.fc.in_features
model.fc = nn.Linear(num_features, num_classes)
```

Seule la nouvelle couche `model.fc` possède ici des paramètres entraînables.

## 5.24. Dégeler les derniers blocs

```python
for parameter in model.layer4.parameters():
    parameter.requires_grad = True
```

On peut utiliser des learning rates différents :

```python
optimizer = AdamW([
    {
        "params": model.layer4.parameters(),
        "lr": 1e-5,
    },
    {
        "params": model.fc.parameters(),
        "lr": 1e-3,
    },
])
```

Le dernier bloc est adapté lentement, tandis que la tête apprend plus rapidement.

## 5.25. Prétraitement attendu

Un modèle pré-entraîné attend un prétraitement précis :

- ordre des canaux ;
- résolution ;
- plage des pixels ;
- moyenne par canal ;
- écart-type par canal.

Une standardisation fréquente prend la forme :

$$
x'_ {c,i,j} =
\frac{
x_{c,i,j}-\mu_c
}{
\sigma_c
}
$$

avec $\(\mu_c\)$ et $\(\sigma_c\)$ définis par le pré-entraînement.

#### ⚠️ Piège

Utiliser les bons poids avec un mauvais prétraitement peut fortement dégrader les résultats.

Lorsque la bibliothèque fournit les transformations associées aux poids, il est préférable de les réutiliser.

## 5.26. Batch Normalization et couches gelées

Geler les poids avec :

```python
parameter.requires_grad = False
```

n’est pas exactement équivalent à placer le module en mode évaluation.

Batch Normalization possède :

- des paramètres appris ;
- des moyennes et variances accumulées.

En mode Train, ces statistiques peuvent continuer à évoluer.

#### Conséquence

Lors d’un feature extraction strict, il faut contrôler :

- les gradients ;
- le mode Train/Eval ;
- le comportement des BatchNorm ;
- le comportement du Dropout.

## 5.27. Domain Shift

Le domaine cible peut être différent du domaine de pré-entraînement.

Exemples :

```text
Photographies naturelles
→ radiographies

Photographies naturelles
→ images satellites

Images nettes
→ caméra industrielle bruitée
```

Plus le domaine cible est différent, moins les features pré-entraînées sont nécessairement adaptées.

### Types de décalage

- couleurs différentes ;
- textures différentes ;
- résolution différente ;
- objets différents ;
- capteur différent ;
- distribution différente.

#### Réflexe

Ne pas supposer que « pré-entraîné » signifie automatiquement « adapté ».

La validation sur le domaine cible reste indispensable.

## 5.28. Catastrophic Forgetting

Un fine-tuning trop agressif peut détruire rapidement les représentations utiles du modèle pré-entraîné.

Ce phénomène est appelé **oubli catastrophique**.

Il est favorisé par :

- learning rate trop élevé ;
- petit dataset ;
- entraînement trop long ;
- dégel immédiat de tout le réseau.

### Limitation

- faible learning rate ;
- dégel progressif ;
- régularisation ;
- Early Stopping ;
- conservation du meilleur checkpoint.

## 5.29. Pré-entraînement supervisé et auto-supervisé

### Pré-entraînement supervisé

Le backbone apprend à partir de labels.

```text
Image
+
Classe
↓
Représentation
```

### Pré-entraînement auto-supervisé

Le modèle construit un objectif à partir des données elles-mêmes.

Exemples de principes :

- rapprocher les représentations de vues d’une même image ;
- distinguer des images différentes ;
- reconstruire des parties masquées ;
- apprendre des représentations invariantes aux augmentations.

### Intérêt

- utiliser de grandes quantités d’images non annotées ;
- apprendre des représentations transférables ;
- réduire la dépendance aux labels humains.

## 5.30. Linear Probing

Le **linear probing** évalue la qualité d’une représentation.

### Procédure

1. geler complètement le backbone ;
2. extraire les représentations ;
3. entraîner uniquement une couche linéaire ;
4. mesurer la performance.

```text
Backbone gelé
↓
Représentation
↓
Classifieur linéaire
```

Une bonne performance indique que les classes sont déjà relativement séparables dans l’espace de représentation.

## 5.31. Transfer Learning vs entraînement depuis zéro

### Transfer Learning

À privilégier lorsque :

- dataset petit ou moyen ;
- backbone pré-entraîné disponible ;
- ressources limitées ;
- besoin d’une baseline forte rapidement.

### Entraînement depuis zéro

Envisageable lorsque :

- dataset très grand ;
- domaine très spécifique ;
- architecture particulière ;
- ressources importantes ;
- pré-entraînement disponible peu pertinent.

#### 🧠 Réflexe

Dans la majorité des projets de vision appliquée, commencer par un modèle pré-entraîné constitue une excellente baseline.

## 5.32. Choisir une architecture

### Priorité à la précision

Envisager :

- ResNet plus profond ;
- EfficientNet ;
- ConvNeXt ;
- architectures modernes pré-entraînées.

### Priorité à la vitesse

Envisager :

- ResNet-18 ;
- MobileNet ;
- EfficientNet de petite taille ;
- modèles spécifiquement optimisés.

### Mémoire limitée

Envisager :

- MobileNet ;
- petite résolution ;
- modèle compact ;
- quantification ultérieure.

### Petit dataset

```text
Modèle pré-entraîné
+
feature extraction
+
fine-tuning prudent
```

### Besoin d’une baseline simple

```text
ResNet-18 pré-entraîné
```

constitue souvent un bon point de départ.

## 5.33. Comparer équitablement les modèles

Pour comparer deux architectures, conserver autant que possible :

- même split ;
- même prétraitement ;
- même data augmentation ;
- même métrique ;
- même protocole de validation ;
- budget d’entraînement comparable ;
- même règle d’Early Stopping.

Mesurer également :

- nombre de paramètres ;
- taille du fichier ;
- temps d’inférence ;
- mémoire ;
- débit ;
- performance par classe.

#### ⚠️ Accuracy seule

Deux modèles ayant la même accuracy peuvent présenter des coûts très différents.

Le meilleur modèle dépend aussi des contraintes de production.

## 5.34. 🧠 Réflexe — Ce qu’il faut retenir des architectures

```text
LeNet
→ structure CNN fondamentale

AlexNet
→ CNN profond + GPU + ReLU

VGG
→ empilement régulier de convolutions 3×3

Inception
→ traitement multi-échelle

ResNet
→ addition + connexions résiduelles

DenseNet
→ concaténation + réutilisation des features

MobileNet
→ convolutions séparables

EfficientNet
→ scaling profondeur / largeur / résolution

ConvNeXt
→ CNN modernisé
```

## 5.35. 🧠 Réflexe — Stratégie de Transfer Learning

```text
Ai-je un modèle pré-entraîné pertinent ?
↓
Oui
↓
Remplacer la tête
↓
Geler le backbone
↓
Entraîner la tête
↓
Évaluer
↓
Dégeler les derniers blocs si nécessaire
↓
Fine-tuning avec faible learning rate
↓
Conserver le meilleur checkpoint
```

## 5.36. Questions de diagnostic

Avant de fine-tuner, vérifier :

- le nombre de classes ;
- la nouvelle couche de sortie ;
- le prétraitement attendu ;
- les paramètres réellement entraînables ;
- le learning rate de chaque groupe ;
- le mode des BatchNorm ;
- la représentativité du split ;
- l’équilibre des classes.

Pendant le fine-tuning, surveiller :

- Train Loss ;
- Validation Loss ;
- métriques par classe ;
- divergence éventuelle ;
- overfitting ;
- oubli catastrophique.

## 5.37. Exercices

### Exercice 1 — Bloc résiduel

Si :

$$
\mathbf{y} = F(\mathbf{x})+\mathbf{x}
$$

et que :

$$
F(\mathbf{x})=\mathbf{0}
$$

alors :

$$
\mathbf{y}=\mathbf{x}
$$

Le bloc représente exactement l’identité.

### Exercice 2 — Dimensions incompatibles

Supposons :

$$
\mathbf{x}
\in
\mathbb{R}^{64\times56\times56}
$$

et :

$$
F(\mathbf{x})
\in
\mathbb{R}^{128\times28\times28}
$$

L’addition directe est impossible.

Il faut transformer le raccourci, par exemple avec une convolution $\(1\times1\)$ de stride 2 :

$$
W_s\mathbf{x}
\in
\mathbb{R}^{128\times28\times28}
$$

La sortie devient :

$$
\mathbf{y} =
F(\mathbf{x})
+
W_s\mathbf{x}
$$

### Exercice 3 — Nouvelle tête

Un backbone produit un vecteur de 512 features.

La nouvelle tâche possède 8 classes.

La tête dense possède :

$$
512\times8+8 = 4\,104
$$

paramètres.

### Exercice 4 — Choix de stratégie

Dataset :

- 800 images ;
- tâche proche des photographies naturelles ;
- modèle pré-entraîné disponible.

Stratégie initiale :

```text
Backbone gelé
+
nouvelle tête
+
forte data augmentation raisonnable
```

Puis, si nécessaire :

```text
Dégeler les derniers blocs
+
faible learning rate
```

### Exercice 5 — Domaine éloigné

Dataset :

- images médicales ;
- backbone pré-entraîné sur photographies naturelles.

Le Transfer Learning reste testable, mais il faut :

- mesurer sa pertinence ;
- envisager un pré-entraînement plus proche ;
- comparer feature extraction et fine-tuning ;
- ne pas supposer que les features sont parfaitement transférables.

---

## 5.38. À retenir

- Une architecture est un compromis entre capacité, coût et facilité d’entraînement.
- LeNet établit le pipeline CNN fondamental.
- VGG popularise l’empilement de petits kernels.
- Inception traite plusieurs échelles en parallèle.
- ResNet apprend un résidu et fournit un chemin direct à l’information et aux gradients.
- DenseNet concatène les représentations précédentes.
- MobileNet réduit le coût avec des convolutions séparables.
- EfficientNet fait varier conjointement profondeur, largeur et résolution.
- ConvNeXt montre que les CNN modernisés restent performants.
- Le backbone extrait les features et la tête produit la sortie.
- Le Transfer Learning réutilise un backbone pré-entraîné.
- Le feature extraction entraîne seulement la nouvelle tête.
- Le fine-tuning adapte une partie ou la totalité du backbone.
- Le learning rate du backbone doit généralement être inférieur à celui de la tête.
- Le prétraitement doit correspondre aux poids pré-entraînés.
- Le domain shift limite parfois le transfert.
- La meilleure architecture dépend autant des contraintes de production que de la précision.
