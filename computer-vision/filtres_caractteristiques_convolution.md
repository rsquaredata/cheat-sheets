<!--
Title: "Filtres, caractéristiques et convolution"
Author: rsquaredata
Last updated: 2026-09-08
-->
# 2. Filtres, caractéristiques et convolution

---

## 2.1. Des pixels aux caractéristiques visuelles

Une image brute contient des pixels, mais les concepts recherchés sont plus abstraits.

```text
Pixels
↓
Contours
↓
Textures
↓
Formes
↓
Parties d’objets
↓
Objets
```

Un modèle doit donc construire une représentation permettant de passer de valeurs numériques locales à des informations visuelles utiles.

## Exemple

Pour reconnaître un visage, les valeurs brutes des pixels sont peu pratiques.

Des motifs plus informatifs peuvent être :

- contours des yeux ;
- forme du nez ;
- texture de la peau ;
- position relative des éléments ;
- forme globale du visage.

Ces motifs sont appelés des **caractéristiques**, ou features.

## 2.2. Pipeline classique de vision

Avant le Deep Learning, un pipeline de vision séparait généralement deux étapes.

```text
Image
↓
Extraction manuelle de caractéristiques
↓
Vecteur de features
↓
Modèle de Machine Learning
↓
Prédiction
```

### Extraction de caractéristiques

Un expert choisissait une transformation adaptée au problème.

Exemples :

- contours ;
- coins ;
- gradients ;
- couleurs ;
- textures ;
- formes ;
- points d’intérêt.

### Modèle prédictif

Le vecteur obtenu était transmis à un modèle classique :

- régression logistique ;
- SVM ;
- k-NN ;
- arbre de décision ;
- Random Forest ;
- AdaBoost.

#### Exemple

```text
Image d’un piéton
↓
Descripteur de gradients
↓
Vecteur numérique
↓
SVM
↓
Piéton / Pas piéton
```

## 2.3. Caractéristiques manuelles et caractéristiques apprises

### 2.3.1. Caractéristiques manuelles

Elles sont définies par un humain à partir de connaissances sur les images.

Exemples :

- histogramme de couleurs ;
- détection de contours ;
- HOG ;
- SIFT ;
- descripteurs de texture.

#### Avantages

- interprétables ;
- parfois efficaces avec peu de données ;
- calcul éventuellement peu coûteux ;
- intégration possible à des modèles simples.

#### Limites

- dépendantes du problème ;
- demandent une expertise métier ;
- difficiles à adapter à toute la variabilité visuelle ;
- pipeline composé de plusieurs étapes séparées ;
- représentation pas nécessairement optimale pour la tâche finale.

### 2.3.2. Caractéristiques apprises

Un réseau de neurones apprend directement les filtres utiles à la tâche.

```text
Images
+
Labels
↓
Apprentissage
↓
Filtres adaptés aux données
↓
Prédictions
```

Les premières couches apprennent souvent des motifs simples :

- bords ;
- orientations ;
- contrastes ;
- couleurs.

Les couches profondes peuvent représenter :

- textures ;
- formes ;
- parties d’objets ;
- objets complets.

### Différence fondamentale

```text
Vision classique
→ l’humain conçoit les features
→ le modèle apprend la décision

Deep Learning
→ le réseau apprend les features
→ et apprend la décision
```

## 2.4. Filtre spatial

Un **filtre spatial** transforme chaque région locale d’une image.

Il est représenté par une petite matrice appelée :

- filtre ;
- masque ;
- noyau ;
- kernel.

Exemple de kernel `3 × 3` :

```text
[ k₁  k₂  k₃ ]
[ k₄  k₅  k₆ ]
[ k₇  k₈  k₉ ]
```

Le kernel est déplacé sur l’image.

À chaque position :

1. il recouvre une région locale ;
2. les valeurs correspondantes sont multipliées ;
3. les produits sont additionnés ;
4. le résultat devient un pixel de la sortie.

```text
Région locale
×
Kernel
↓
Somme
↓
Une valeur de sortie
```

## 2.5. Calcul d’une convolution locale

Considérons la région :

```text
[ 1  2  0 ]
[ 0  1  3 ]
[ 2  1  0 ]
```

et le kernel :

```text
[ 1  0 -1 ]
[ 1  0 -1 ]
[ 1  0 -1 ]
```

Le calcul est :

```text
1×1 + 2×0 + 0×(-1)
+
0×1 + 1×0 + 3×(-1)
+
2×1 + 1×0 + 0×(-1)
```

Donc :

```text
1 + 0 + 0
+
0 + 0 - 3
+
2 + 0 + 0
=
0
```

La valeur produite à cette position est :

```text
0
```

Le même kernel est ensuite déplacé sur toute l’image.

## 2.6. Feature map

La matrice obtenue après application d’un filtre est appelée **feature map**, ou carte d’activation.

```text
Image
↓
Kernel
↓
Feature map
```

Une feature map indique où le motif recherché par le filtre apparaît dans l’image.

#### Exemple

Si le filtre détecte des bords verticaux :

- valeur élevée → bord vertical probable ;
- valeur faible → motif absent ;
- signe positif ou négatif → direction possible de la transition.

## 2.7. Convolution ou corrélation croisée ?

Mathématiquement, une vraie convolution retourne le kernel avant de l’appliquer.

En Deep Learning, les bibliothèques réalisent généralement une **corrélation croisée** :

```text
kernel utilisé sans retournement
```

Pourtant, l’opération est habituellement appelée « convolution ».

#### Pourquoi cette distinction est-elle peu gênante ?

Dans un CNN, les coefficients du kernel sont appris.

Le réseau peut donc apprendre directement l’orientation nécessaire.

#### 🧠 À retenir

Dans le contexte des CNN :

```text
« convolution » ≈ corrélation croisée avec poids appris
```

## 2.8. Filtres courants en traitement d’image

### 2.8.1. Filtre d’identité

Exemple :

```text
[ 0  0  0 ]
[ 0  1  0 ]
[ 0  0  0 ]
```

Il conserve approximativement l’image originale.

### 2.8.2. Flou moyen

Exemple :

```text
1/9 ×
[ 1  1  1 ]
[ 1  1  1 ]
[ 1  1  1 ]
```

Chaque pixel est remplacé par une moyenne locale.

#### Effets

- réduit certaines variations rapides ;
- diminue le bruit ;
- atténue les détails ;
- rend les contours moins nets.

### 2.8.3. Filtre gaussien

Le filtre gaussien réalise également un lissage, mais donne davantage de poids au centre.

Exemple simplifié :

```text
1/16 ×
[ 1  2  1 ]
[ 2  4  2 ]
[ 1  2  1 ]
```

#### Utilité

- réduction du bruit ;
- prétraitement avant détection de contours ;
- construction de représentations multi-échelles.

### 2.8.4. Filtre de netteté

Exemple :

```text
[  0 -1  0 ]
[ -1  5 -1 ]
[  0 -1  0 ]
```

Il renforce les différences locales.

#### Effets

- accentue les contours ;
- augmente la netteté apparente ;
- peut également amplifier le bruit.

### 2.8.5. Détection de contours

Un contour correspond généralement à une variation importante d’intensité ou de couleur.

```text
Région sombre | Région claire
              ↑
            contour
```

Les filtres dérivatifs mesurent ces variations.

## 2.9. Gradient d’une image

Le **gradient** indique comment l’intensité change dans l’espace.

Il possède :

- une amplitude ;
- une orientation.

### Gradient horizontal

Mesure la variation selon l’axe horizontal.

Il permet notamment de mettre en évidence les contours verticaux.

### Gradient vertical

Mesure la variation selon l’axe vertical.

Il permet notamment de mettre en évidence les contours horizontaux.

### Amplitude du gradient

À partir des composantes `Gₓ` et `Gᵧ` :

```text
|G| = √(Gₓ² + Gᵧ²)
```

Une amplitude élevée indique une variation locale importante.

### Orientation du gradient

```text
θ = arctan(Gᵧ / Gₓ)
```

Elle indique la direction principale de la variation.

## 2.10. Filtres de Sobel

Les filtres de Sobel permettent d’approximer le gradient.

### Sobel horizontal

```text
Gₓ =
[ -1  0  1 ]
[ -2  0  2 ]
[ -1  0  1 ]
```

Il réagit principalement aux variations gauche-droite et met donc en évidence des contours verticaux.

### Sobel vertical

```text
Gᵧ =
[ -1 -2 -1 ]
[  0  0  0 ]
[  1  2  1 ]
```

Il réagit principalement aux variations haut-bas et met donc en évidence des contours horizontaux.

### Combinaison

```text
Image
↓
Sobel Gₓ + Sobel Gᵧ
↓
Amplitude et orientation des contours
```

#### ⚠️ Piège

La convention peut varier selon les bibliothèques et les axes utilisés.

Le plus important est de comprendre que :

- un filtre mesure la variation horizontale ;
- l’autre mesure la variation verticale.

## 2.11. Pourquoi les contours sont-ils utiles ?

Les contours peuvent rester relativement informatifs malgré certaines variations de couleur ou d’éclairage.

Ils permettent de représenter :

- silhouettes ;
- formes ;
- séparations entre objets ;
- structures locales.

Cependant, un contour seul ne permet pas toujours d’identifier un objet.

```text
Contours locaux
↓
Combinaisons de contours
↓
Formes
↓
Objets
```

Cette composition progressive sera centrale dans les CNN.

## 2.12. Kernel Size

La taille du kernel définit l’étendue du voisinage observé.

Exemples :

```text
1 × 1
3 × 3
5 × 5
7 × 7
```

### Petit kernel

#### Avantages

- peu de paramètres ;
- calcul plus rapide ;
- capture de motifs locaux ;
- possibilité d’empiler plusieurs couches.

### Grand kernel

#### Avantages

- observe directement une zone plus large.

#### Limites

- davantage de paramètres ;
- coût de calcul supérieur ;
- risque de perdre de la finesse locale.

#### Pourquoi `3 × 3` est-il fréquent ?

Deux convolutions `3 × 3` successives observent une zone plus large qu’une seule couche, tout en introduisant :

- plusieurs transformations ;
- plusieurs non-linéarités ;
- généralement moins de paramètres qu’un très grand kernel.

## 2.13. Stride

Le **stride** est le pas de déplacement du kernel.

### Stride 1

Le kernel se déplace d’un pixel à chaque étape.

```text
stride = 1
```

→ sortie détaillée  
→ résolution élevée

### Stride 2

Le kernel avance de deux pixels.

```text
stride = 2
```

→ sortie plus petite  
→ calcul réduit  
→ perte d’information spatiale possible

#### 🧠 Idée

Le stride contrôle à la fois :

- le déplacement du filtre ;
- la taille de la sortie ;
- le niveau de sous-échantillonnage.

## 2.14. Padding

Sans ajout autour de l’image, le kernel ne peut pas être centré sur les pixels situés près des bords.

La sortie devient donc plus petite.

Le **padding** ajoute des valeurs autour de l’image.

### Zero Padding

La bordure ajoutée contient des zéros.

```text
0 0 0 0 0
0 [image] 0
0 [image] 0
0 0 0 0 0
```

### Valid Padding

Aucun padding.

```text
padding = 0
```

La sortie est généralement plus petite.

### Same Padding

Le padding est choisi pour conserver approximativement la même taille spatiale lorsque le stride vaut 1.

```text
hauteur de sortie ≈ hauteur d’entrée
largeur de sortie ≈ largeur d’entrée
```

#### Pourquoi ajouter du padding ?

- préserver la résolution ;
- traiter davantage les pixels de bord ;
- empiler plusieurs convolutions sans réduire trop vite l’image.

#### ⚠️ Effet de bord

Les valeurs ajoutées sont artificielles.

Le choix du padding peut donc influencer les activations près des bords.

D’autres stratégies existent :

- réflexion ;
- réplication ;
- padding circulaire.

# 2.15. Taille de sortie d’une convolution

Pour une dimension spatiale :

- `N` : taille de l’entrée ;
- `K` : taille du kernel ;
- `P` : padding ;
- `S` : stride.

La taille de sortie est :

```text
sortie = floor((N + 2P - K) / S) + 1
```

### Exemple 1

```text
N = 5
K = 3
P = 0
S = 1
```

Donc :

```text
sortie = floor((5 - 3) / 1) + 1
        = 3
```

Une entrée `5 × 5` produit une sortie `3 × 3`.

### Exemple 2

```text
N = 5
K = 3
P = 1
S = 1
```

Donc :

```text
sortie = floor((5 + 2 - 3) / 1) + 1
        = 5
```

La taille spatiale est conservée.

### Exemple 3

```text
N = 6
K = 3
P = 1
S = 2
```

Donc :

```text
sortie = floor((6 + 2 - 3) / 2) + 1
        = floor(2.5) + 1
        = 3
```

## 2.16. Convolution sur une image couleur

Une image RGB possède trois canaux.

Un filtre qui produit une feature map doit donc couvrir les trois canaux.

```text
Kernel spatial : K × K
Profondeur     : 3
```

Forme d’un filtre RGB :

```text
K × K × 3
```

Les contributions des trois canaux sont combinées pour produire une valeur de sortie.

```text
Canal rouge
+
Canal vert
+
Canal bleu
↓
Une feature map
```

## 2.17. Plusieurs filtres

Une couche peut apprendre plusieurs kernels.

Chaque kernel produit une feature map différente.

```text
Image RGB
↓
Filtre 1 → Feature map 1
Filtre 2 → Feature map 2
Filtre 3 → Feature map 3
...
Filtre F → Feature map F
```

Si la couche possède `F` filtres :

```text
nombre de canaux de sortie = F
```

#### Exemple

Entrée :

```text
32 × 32 × 3
```

Couche avec 64 filtres :

```text
3 × 3 × 3
```

Sortie spatiale conservée :

```text
32 × 32 × 64
```

Les 64 canaux correspondent à 64 motifs appris.

## 2.18. Paramètres d’une convolution

Supposons :

- kernel `K × K` ;
- `C_in` canaux d’entrée ;
- `C_out` filtres de sortie.

Le nombre de poids est :

```text
K × K × C_in × C_out
```

En ajoutant un biais par filtre :

```text
nombre de paramètres
=
K × K × C_in × C_out + C_out
```

### Exemple

Couche avec :

```text
kernel = 3 × 3
C_in = 3
C_out = 64
```

Nombre de paramètres :

```text
3 × 3 × 3 × 64 + 64
=
1 792
```

#### Comparaison avec une couche dense

Pour une image `224 × 224 × 3`, une connexion dense directe vers 64 neurones demanderait :

```text
224 × 224 × 3 × 64
=
9 633 792 poids
```

La convolution utilise beaucoup moins de paramètres grâce :

- à la connectivité locale ;
- au partage des poids.

---

## 2.19. Connectivité locale

Un neurone convolutif ne regarde pas toute l’image.

Il regarde une région locale.

```text
Pixel de sortie
↓
Petite région de l’entrée
```

Cela permet :

- de capturer des motifs locaux ;
- de réduire le nombre de paramètres ;
- d’exploiter la structure spatiale.

## 2.20. Partage des poids

Le même kernel est appliqué à toutes les positions de l’image.

```text
Même motif recherché
en haut, en bas,
à gauche ou à droite
```

Le modèle n’a donc pas besoin d’apprendre un filtre différent pour chaque position.

#### Conséquences

- moins de paramètres ;
- meilleure efficacité ;
- détection du même motif à plusieurs endroits ;
- meilleure capacité de généralisation.

## 2.21. Équivariance à la translation

Si un objet se déplace dans l’image, la feature map correspondante se déplace également.

Cette propriété est appelée **équivariance à la translation**.

```text
Motif déplacé dans l’entrée
↓
Activation déplacée dans la sortie
```

#### Équivariance ≠ invariance

##### Équivariance

La sortie change de position avec l’entrée.

##### Invariance

La sortie finale reste approximativement identique malgré le déplacement.

Une convolution est surtout équivariante à la translation.

L’invariance partielle est obtenue progressivement grâce à :

- l’agrégation spatiale ;
- le pooling ;
- les strides ;
- les couches profondes ;
- la data augmentation.

## 2.22. Champ réceptif

Le **champ réceptif** d’une unité correspond à la région de l’image d’entrée pouvant influencer cette unité.

### Première couche

Une convolution `3 × 3` observe une petite région.

### Couches successives

En empilant les convolutions, les unités profondes dépendent d’une zone de plus en plus large.

```text
Couche 1 → motifs locaux
Couche 2 → combinaisons locales
Couche 3 → structures plus larges
Couche profonde → contexte étendu
```

#### Intérêt

Le réseau peut apprendre une hiérarchie :

```text
bords
↓
textures
↓
formes
↓
parties d’objets
↓
objets
```

## 2.23. Pooling

Le **pooling** réduit la taille spatiale d’une feature map.

### Max Pooling

Conserve la valeur maximale d’une région.

Exemple sur une région `2 × 2` :

```text
[ 1  5 ]
[ 3  2 ]
```

Résultat :

```text
5
```

#### Intuition

Le motif est-il fortement présent dans cette région ?

### Average Pooling

Calcule la moyenne locale.

Avec la même région :

```text
(1 + 5 + 3 + 2) / 4 = 2.75
```

### Global Average Pooling

Calcule une moyenne sur toute la dimension spatiale de chaque canal.

```text
H × W × C
↓
1 × 1 × C
```

Il est souvent utilisé avant la couche de classification.

#### Avantages

- réduction de dimension ;
- diminution du coût de calcul ;
- agrégation de l’information ;
- moins de paramètres qu’une grande couche dense.

#### Limites

- perte d’information spatiale ;
- détails fins potentiellement supprimés ;
- problématique pour les tâches exigeant une localisation précise.

## 2.24. Pooling vs convolution avec stride

Deux méthodes peuvent réduire la résolution.

### Pooling

```text
Convolution
↓
Pooling
```

La règle de réduction est généralement fixe.

### Convolution avec stride

```text
Convolution stride 2
```

Le sous-échantillonnage est intégré dans une transformation dont les poids sont appris.

Les architectures modernes utilisent souvent des convolutions avec stride à certains endroits, mais le pooling reste important à connaître.

## 2.25. Caractéristiques classiques importantes

Les méthodes classiques restent utiles pour :

- comprendre l’histoire de la vision ;
- travailler avec peu de données ;
- créer des baselines ;
- résoudre certains problèmes contraints ;
- interpréter les informations visuelles.

### 2.25.1. Histogramme de couleurs

Il compte la fréquence des différentes intensités ou couleurs.

#### Avantage

Décrit la distribution globale des couleurs.

#### Limite

Ignore largement leur position.

Deux images très différentes peuvent avoir des histogrammes similaires.

### 2.25.2. HOG — Histogram of Oriented Gradients

HOG décrit la distribution locale des orientations de gradients.

Pipeline simplifié :

```text
Image
↓
Calcul des gradients
↓
Division en cellules
↓
Histogrammes d’orientations
↓
Normalisation par blocs
↓
Vecteur HOG
```

#### Intuition

La forme d’un objet peut être représentée par la distribution de ses contours.

#### Utilisation classique

```text
HOG
+
SVM linéaire
```

Cette combinaison a notamment été utilisée pour détecter des piétons.

#### Limites

- représentation fixée manuellement ;
- sensible à certaines variations ;
- moins flexible qu’un réseau appris de bout en bout.

### 2.25.3. Points d’intérêt

Un point d’intérêt est une région locale facilement identifiable.

Exemples :

- coin ;
- jonction ;
- motif texturé ;
- point stable sous plusieurs vues.

Des méthodes comme SIFT ou ORB construisent des descripteurs autour de ces points.

Applications :

- mise en correspondance d’images ;
- panorama ;
- estimation géométrique ;
- localisation ;
- reconstruction 3D.

## 2.26. Traitement d’image vs apprentissage

### Traitement d’image

Les opérations sont définies explicitement.

Exemples :

- flou ;
- seuillage ;
- morphologie ;
- filtre de Sobel ;
- correction de contraste.

```text
Image
↓
Règles déterminées
↓
Image transformée
```

### Machine Learning

Le modèle apprend une règle de décision à partir d’exemples.

```text
Images + labels
↓
Apprentissage
↓
Prédiction
```

### Deep Learning

Le réseau apprend à la fois :

- la représentation ;
- la décision finale.

```text
Images
↓
Filtres appris
↓
Représentations hiérarchiques
↓
Prédiction
```

#### Les approches ne s’excluent pas

Un pipeline moderne peut combiner :

- traitement d’image ;
- règles métier ;
- réseau de neurones ;
- post-traitement géométrique.

# 2.27. Quand un traitement classique peut suffire

Un réseau profond n’est pas toujours nécessaire.

Une méthode classique peut être préférable si :

- le problème est simple ;
- la règle visuelle est connue ;
- le contraste est stable ;
- l’environnement est contrôlé ;
- très peu de données sont disponibles ;
- l’interprétabilité est prioritaire ;
- les ressources de calcul sont limitées.

### Exemple

Détecter une pastille rouge sur fond blanc dans un environnement industriel contrôlé peut parfois être résolu avec :

```text
Conversion couleur
↓
Seuillage
↓
Nettoyage morphologique
↓
Détection de composante
```

Entraîner un grand réseau serait alors inutilement complexe.

## 2.28. Limites des filtres fixes

Un filtre de Sobel détecte un type précis de variation.

Mais une tâche réelle peut nécessiter des motifs très variés :

- texture d’un matériau ;
- forme d’un œil ;
- roue d’un véhicule ;
- défaut microscopique ;
- combinaison complexe de couleurs.

Il est difficile de concevoir manuellement tous les filtres nécessaires.

#### Solution

Apprendre automatiquement les kernels à partir des données.

```text
Filtres prédéfinis
↓
Filtres appris
```

C’est le principe central des réseaux convolutifs.


## 2.29. 🧠 Réflexe — Comprendre une convolution

```text
Image
↓
Petite région locale
↓
Multiplication par un kernel
↓
Somme
↓
Une activation
↓
Déplacer le kernel
↓
Feature map
```

Toujours identifier :

- taille du kernel ;
- nombre de canaux d’entrée ;
- nombre de filtres ;
- stride ;
- padding ;
- dimensions de sortie ;
- nombre de paramètres.

## 2.30. 🧠 Réflexe — Choisir entre règles et apprentissage

```text
Le motif visuel est-il simple, stable et explicitement définissable ?
```

### Oui

Envisager :

- traitement d’image ;
- seuillage ;
- contours ;
- morphologie ;
- descripteurs manuels ;
- modèle classique.

### Non

```text
Variabilité importante
+
nombreux motifs possibles
+
données disponibles
```

↓

```text
caractéristiques apprises avec un CNN
```

## 2.31. Exercices de compréhension

### Exercice 1

Une entrée possède une taille `32 × 32`.

On applique :

```text
kernel = 3
padding = 0
stride = 1
```

Calcul :

```text
sortie = floor((32 - 3) / 1) + 1
        = 30
```

La sortie possède une taille :

```text
30 × 30
```

### Exercice 2

Même entrée avec :

```text
kernel = 3
padding = 1
stride = 1
```

Calcul :

```text
sortie = floor((32 + 2 - 3) / 1) + 1 = 32
```

La taille est conservée.

### Exercice 3

Une couche reçoit une entrée avec 16 canaux et produit 32 feature maps avec des kernels `3 × 3`.

Nombre de paramètres :

```text
3 × 3 × 16 × 32 + 32 = 4 640
```

### Exercice 4

Pourquoi une convolution utilise-t-elle moins de paramètres qu’une couche dense ?

Réponse :

- connectivité locale ;
- même kernel utilisé à toutes les positions ;
- partage des poids.

### Exercice 5

Pourquoi ne faut-il pas dire qu’une convolution est totalement invariante à la translation ?

Réponse :

La feature map se déplace lorsque le motif se déplace.

La convolution est principalement **équivariante** à la translation. L’invariance partielle est construite par l’architecture complète.

---

## 2.32. À retenir

- Une feature est une représentation utile à la décision.
- Les approches classiques utilisent souvent des features conçues manuellement.
- Les réseaux profonds apprennent leurs propres features.
- Un kernel transforme une région locale de l’image.
- La sortie d’un filtre est une feature map.
- Le stride contrôle le pas et participe à la réduction de résolution.
- Le padding contrôle le traitement des bords et la taille de sortie.
- Plusieurs filtres produisent plusieurs canaux de sortie.
- Le partage des poids réduit fortement le nombre de paramètres.
- La convolution exploite la structure locale des images.
- Les couches successives augmentent le champ réceptif.
- Le pooling agrège l’information mais réduit la précision spatiale.
- Les filtres classiques restent utiles pour comprendre et résoudre certains problèmes simples.
- Un CNN remplace les filtres conçus manuellement par des filtres appris.
