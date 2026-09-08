<!--
Title: "Computer Vision"
Author: rsquaredata
Last updated: 2026-09-08
-->

# 1. Comprendre une image et formuler un problème de vision

---

## 1.1. De l'image à l'information

La **Computer Vision**, ou vision par ordinateur, cherche à extraire automatiquement une information utile à partir d'images ou de vidéos.

Une image ne fournit pas directement à la machine des concepts comme : "chat", "piéton", "tumeur", "route", "produit défectueux". Elle fournit uniquement un ensemble organisé de valeurs numériques


```text
Valeurs de pixels
↓
Structure visuelle
↓
Objets et relations
↓
Information utile
↓
Décision
```

Le rôle d’un système de vision est de construire ce passage entre pixels et information.

### Exemples

#### Industrire

```
Photographie d'une pièce
↓
Détection d'un défaut
↓
Rejet ou validation de la pièce
```

#### Médecine

```
Image médicale
↓
Localisation d'une région suspecte
↓
Aide au diagnostic
```

#### Transport

```
Image d'une caméra
↓
Détection des véhicules et piétons
↓
Aide à la conduite
```

#### Commerce

```
Photographie d'un produit
↓
Identification de sa catégorie
↓
Indexation du catalogue
```



## 1.2. Pourquoi la vision est-elle difficile ?

Un objet réel peut produire des images très différentes.

### 1.2.1. Variations d'apparence

L'apparence notamment avec :

- la position ;
- l'orientation ;
- la distance ;
- l'échelle :
- l'éclairage ;
- la caméra ;
- l'arrière-plan ;
- le flou ;
- les occultations ;
- les conditions météorologiques.

Une voiture photographiée de nuit et partiellement cachée doit toujours être reconnue comme une voiture.

### 1.2.2. Variabilité intra-classe

Deux éléments appartenant à la même classe peuvent être visuellement très différents.

```
Citadine
Camionnette
Berline
Voiture de sport
↓
Classe : voiture
```

### 1.2.3. Similarité inter-classe

Des classes différentes peuvent se ressembler.

Exemples :
- chien et loup ;
- vélo et moto ;
- fissure et simple texture ;
- lésion bénigne et lésion maligne.

### 1.2.4. Ambiguïté

Une image ne contient pas toujours suffisamment d’information pour déterminer la bonne réponse.

Causes possibles :
- résolution insuffisante ;
- objet presque entièrement caché ;
- classes visuellement indiscernables ;
- mauvaise qualité d’acquisition ;
- besoin d’un contexte absent de l’image.

#### 🧠 Réflexe

Une mauvaise prédiction ne signifie pas toujours que le modèle est insuffisant.  
Il faut aussi demander :
> L'information nécessaire à la décision est-elle réellement visible dans l'image ?

## 1.3. Représentation numérique d'une image

### 1.3.1. Pixel

Un **pixel** est une unité élémentaire d’une image numérique.

Il représente une mesure effectuée à une position donnée.

Cette mesure peut correspondre à :
- une intensité lumineuse ;
- une couleur ;
- une profondeur ;
- une température ;
- une réponse spectrale.

### 1.3.2. Image en niveaux de gris

Une image en niveaux de gris peut être représentée par une matrice :

```
hauteur x largeur
```

Chaque case contient une intensité.

Avec un codage classique sur 8 bits :

```
0   → noir
255 → blanc
```
Les valeurs intermédiaires représentent différents niveaux de gris.

### 1.3.3. Image RGB

Une image couleur RGB utilise trois canaux :
- R : rouge ;
- G : vert ;
- B : bleu.

Sa forme est généralement :
```
hauteur × largeur × 3
```
Un pixel est représenté par trois valeurs :
```
[R, G, B]
```
Exemples :
```
[255, 0, 0]     → rouge
[0, 255, 0]     → vert
[0, 0, 255]     → bleu
[255, 255, 255] → blanc
[0, 0, 0]       → noir
```

### 1.3.4. Autres types d'images

Toutes les images ne sont pas limitées à RGB.

#### RGBA

Un quatrième canal représente la transparence.

#### RGB-D

Une carte de profondeur est associée à l’image couleur.

#### Imagerie médicale

Les valeurs peuvent représenter une mesure physique et utiliser une profondeur numérique supérieure à 8 bits.

#### Imagerie multispectrale

Chaque pixel peut contenir de nombreux canaux correspondant à différentes bandes spectrales.

#### Images thermiques

Les valeurs représentent une mesure liée à la température.

#### ⚠️ Piège

Il ne faut pas supposer automatiquement que :
```
image = RGB avec valeurs entre 0 et 255
```
Le sens, l’unité et la plage des valeurs dépendent du dispositif d’acquisition.

## 1.4. Une image possède une structure spatiale

Une image n’est pas simplement une liste de variables.

Les pixels voisins ont généralement une relation :
- ils peuvent appartenir au même objet ;
- ils peuvent former une texture ;
- une variation brutale peut représenter un bord ;
- un motif local peut représenter une forme.

```
Pixel isolé       → information limitée
Pixel + voisinage → bord, texture, motif ou forme
```

Cette structure spatiale justifie l’utilisation :
- de filtres ;
- de convolutions ;
- de réseaux convolutifs ;
- de mécanismes d’attention adaptés aux images.

#### Idée fondamentale

Un modèle de vision doit exploiter à la fois :
- le contenu visuel ;
- la position des éléments ;
- leurs relations spatiales.

## 1.5. Images et tenseurs

Les bibliothèques de Deep Learning représentent généralement les images par des tenseurs.

Pour un ensemble d’images :
```
N × C × H × W
```
ou
```
N × H × W × C
```
avec :
- N : nombre d'images ;
- C : nombre de canaux ;
- H : hauteur ;
- W : largeur.

### Convention PyTorch fréquente
```
N × C × H × W
```
### Convention TensorFlow/Keras fréquente
```
N × H × W × C
```

#### ⚠️ Piège

Un mauvais ordre des dimensions peut :
- provoquer une erreur ;
- inverser les canaux et les dimensions ;
- rendre l’entrée incohérente ;
- dégrader silencieusement les résultats.

Toujours vérifier la forme attendue par le modèle.

## 1.6. Les grandes tâches de Computer Vision

La première décision d’un projet de vision n’est pas le choix de l’architecture. Il faut d’abord déterminer la sortie attendue.
> Que doit produire le système ? → Une classe ; plusieurs classes ; des boîtes ; un masque ; des points ; une valeur par pixel ; du texte.

### 1.6.1. Classification d'images

#### Question
> Que contient principalement cette image ?

Le modèle produit une classe pour l’image entière.
```
Image
↓
Modèle
↓
Classe
```

Exemples :
- type d’animal ;
- catégorie d’un produit ;
- présence ou absence d’une maladie ;
- pièce conforme ou défectueuse.

#### Annotation nécessaire
```
Une image → une classe
```

#### Limite

Le modèle indique ce que contient l’image, mais pas où se trouve l’objet.

### 1.6.2. Classification multiclasse

Une seule classe est choisie parmi plusieurs possibilités.

```
chat
chien
lapin
cheval
```
Chaque image appartient à une seule de ces classes.

La sortie utilise souvent une distribution de probabilités :

```
chat   : 0.85
chien  : 0.10
lapin  : 0.04
cheval : 0.01
```

### 1.6.3. Classification multilabel

Plusieurs catégories peuvent être vraies simultanément.

Exemple :
```
Image d’une rue
↓
personne = oui
voiture  = oui
vélo     = oui
chien    = non
```
#### Différence fondamentale

Multiclasse

```
une classe parmi plusieurs
```

Multilabel

```
plusieurs classes simultanément
```

### 1.6.4. Localisation

La localisation consiste à prédire :
- la classe de l'objet principal ;
- sa position dans l'image.

La position est généralement représentée par une *boîte englobante**, ou bounding box.

```
Image
↓
Classe : chien
Boîte : position du chien
```

### 1.6.5. Détection d'objets

#### Question
> Quels objets sont présent et où se trouvent-ils ?

Le modèle produit plusieurs détections.

Chaque détection contient généralement :
- une classe ;
- une bounding box ;
- un score de confiance.

```
Objet 1 → personne + boîte + confiance
Objet 2 → vélo + boîte + confiance
Objet 3 → voiture + boîte + confiance
```

#### Annotation nécessaire

Pour chaque objet :

```
classe + coordonnées de la boîte
```

#### Classification vs détection

```
Classification → une réponse globale pour l’image
Détection      → une réponse pour chaque objet localisé
```

### 1.6.6. Segmentation sémantique

#### Question

> À quelle classe appartient chaque pixel ?

Le modèle produit un masque de classes.

Exemple :

```
pixel → ciel
pixel → route
pixel → voiture
pixel → piéton
```

Tous les objets appartenant à une même classe partagent le même label.

Deux voitures différentes sont simplement représentées comme :

```
classe voiture
```

### 1.6.7. Segmentation d'instances

La segmentation d’instances distingue chaque objet séparément.

```
voiture 1
voiture 2
voiture 3
```

Chaque objet possède :
- une classe ;
- un masque ;
- un identifiant d'instance.

#### Sémantique vs instances

```
Segmentation sémantique  → quels pixels appartiennent à la classe voiture ?
Segmentation d’instances → quels pixels appartiennent à chaque voiture ?
```

### 1.6.8. Segmentation panoptique

La segmentation panoptique combine :
- la segmentation sémantique ;
- la segmentation d'instances.

Elle distingue généralement deux types de contenu.

#### Things

Objets individualisables :
- personne ;
- voiture ;
- vélo ;
- animal.

#### Stuff

Régions sans instance clairement séparée :
- ciel ;
- route ;
- herbe ;
- mur.

Chaque pixel reçoit une classe et, cela a un sens, un identifiant d'instance.

### 1.6.9. Estimation de pose

Le modèle prédit des points clés.

Pour une personne :
- tête ;
- épaules ;
- coudes ;
- poignets ;
- hanches ;
- genoux ;
- chevilles.

Applications :
- sport ;
- ergonomie ;
- analyse du mouvement ;
- interaction humain-machine ;
- santé.

### 1.6.10. Estimation de profondeur

Le modèle estime une distance pour chaque pixel.

```
Image
↓
Modèle
↓
Carte de profondeur
```

Applications :
- robotique ;
- conduite autonome ;
- reconstruction 3D ;
- réalité augmentée.

### 1.6.11. Suivi d'objets

Le suivi, ou **tracking**, consiste à retrouver les mêmes objets au fil d'une vidéo/

```
Frame 1 → personne 7
Frame 2 → personne 7
Frame 3 → personne 7
```

Il faut combiner :
- détection ;
- apparence ;
- position ;
- mouvement ;
- cohérence temporelle.

### 1.6.12. Reconnaissance optique de caractères

L'OCR cherche à extraire du texte depuis une image.

```
Document ou photographie
↓
Détection des zones de texte
↓
Reconnaissance des caractères
↓
Texte numérique
```

Applications :
- documents scannés ;
- factures ;
- plaques d'immatriculation ;
- formulaires ;
- écriture manuscrite.

### 1.6.13. Description d'image

L'**image captioning** consiste à produire une phrase décrivant une image.

```
Image
↓
Représentation visuelle
↓
Modèle de langage
↓
« Un chien court dans un parc »
```

Cette tâche relie :
- Computer Vision ;
- traitement automatique du langage ;
- modèles multimodaux.

## 1.7. Choisir la tâche à partir du besoin réel

### Exemple 1 - Contrôle industriel

Besoin :
> Savoir si une pièce possède un défaut.

Si une réponse globale suffit → classification binaire.  
Si la position du défaut est nécessaire → détection.  
Si sa forme et sa surface doivent être mesurées → segmentation.

### Exemple 2 - Analyse médicale

Besoin :
> déterminer si une image présente une anomalie.

Selon le niveau de précision attendu :
- présence / absence → classification
- position approximative → détection
- contour précis → segmentation

### Exemple 3 - Comptage de véhicules

Une classification indiquant "véhicules présents" ne suffit pas.

Pour compter les véhicules individuellement → détection d'objets.  
Pour les suivre dans une vidéo → détection + tracking.

#### 🧠 Réflexe

Ne pas choisir une tâche plus complexe que nécessaire.

Une annotation par masque est beaucoup plus coûteuse qu’une annotation par classe.

La bonne question est :
> Quelle sortie minimale permet de prendre la décision métier ?

## 1.8. Les annotations déterminent ce que le modèle peut apprendre

| Annotation disponible           | Tâche naturelle                      |
|---------------------------------|--------------------------------------|
| Une classe par image            | Classification                       |
| Plusieurs labels par image      | Classification multilabel            |
| Classe et boîte de chaque objet | Détection                            |
| Classe de chaque pixel          | Segmentation sémantique              |
| Masque de chaque objet          | Segmentation d'instances             |
| Coordonnées de points           | Estimation de pose                   |
| Texte associé à l'image         | Description ou recherche multimodale |

### Coût des annotations

Plus une annotation est précise, plus elle est généralement :
- longue à produire ;
- coûteuse ;
- difficile à vérifier ;
- sensible aux désaccords entre annotateurs.

```
Label d’image
↓
Bounding box
↓
Points clés
↓
Masque par pixel
```

Le coût augmente généralement en descendant.

### Qualité des annotations

Des annotations incohérentes limitent les performances possibles du modèle.

Il faut définir :
- les classes ;
- les cas limites ;
- les objets à annoter ;
- les objets à ignorer ;
- la gestion des occultations ;
- la précision attendue ;
- une procédure de contrôle.

## 1.9. Construire un dataset représentatif

Le dataset doit représenter les conditions réelles d’utilisation.

### Variations à couvrir

- éclairage ;
- saison ;
- météo ;
- type de caméra ;
- résolution ;
- distance ;
- angle de vue ;
- arrière-plan ;
- taille des objets ;
- occultation ;
- qualité d’image ;
- diversité des utilisateurs ou des sites.

### Exemple

Un modèle entraîné uniquement sur des photographies lumineuses peut échouer :
- la nuit ;
- sous la pluie ;
- avec une autre caméra ;
- lorsque l'objet est partiellement masqué.

#### 🧠 Réflexe

Avant d’augmenter la complexité du modèle, vérifier si les données d’entraînement couvrent réellement les conditions de production.

## 1.10. Séparer Train, Validation et Test

### Train

Utilisé pour apprendre les paramètres du modèle.

### Validation

Utilisé pour :
- choisir l'architecture ;
- régler les hyperparamètres ;
- sélectionner le seuil ;
- décider quand arrêter l'entraînement.

### Test

Utilisé uniquement pour estimer la performance finale.

Le Test ne doit pas guider les décisions de modélisation.

### 1.10.1. Le danger des images presque identiques

Un split aléatoire image par image peut provoquer une fuite.

Exemples :
- frames consécutives d'une vidéo
- plusieurs photographies du même objet ;
- images du même patient ;
- images du même site ;
- versions augmentées d'une même image.

Si des images presque identiques apparaissent dans Train et Test :
→ le modèle peut mémoriser leur contexte ;  
→ la performance mesurée devient trop optimiste.

### 1.10.2. Split par groupe

Selon le problème, il faut séparer les données par :
- vidéo;
- patient ;
- individu ;
- objet physique ;
- caméra ;
- site ;
- session d'acquisition ;
- période temporelle.

#### Exemples

Exemple médical : toutes les images d'un patient doivent rester dans le même ensemble.  
Exemple vidéo : toutes les frames d'une même séquence doivent rester dans le même ensemble.  
Exemple industriel : toutes les images d'une même pièce doivent rester dans le même ensemble.

## 1.11. Pipeline général d'un projet de Computer Vision

```
Définir le besoin
↓
Identifier la sortie attendue
↓
Choisir la tâche
↓
Définir les annotations
↓
Collecter des données représentatives
↓
Construire Train / Validation / Test
↓
Créer une baseline
↓
Entraîner le modèle
↓
Évaluer avec les métriques adaptées
↓
Analyser les erreurs
↓
Tester la robustesse
↓
Déployer
↓
Surveiller les performances
```

## 1.12. Questions à se poser avant de modéliser

### Sur le besoin

- Quelle décision dépendra du modèle ?
- Une réponse globale suffit-elle ?
- Faut-il localiser ou délimiter un objet ?
- Une erreur peut-elle avoir une conséquence grave ?

### Sur les données

- Combien d’images sont disponibles ?
- Les classes sont-elles équilibrées ?
- Les images représentent-elles la production ?
- Certaines images proviennent-elles des mêmes groupes ?
- Les annotations sont-elles fiables ?

### Sur la production

- Quelle latence est acceptable ?
- Le traitement doit-il être en temps réel ?
- Le modèle fonctionnera-t-il sur serveur, téléphone ou système embarqué ?
- Les images futures ressembleront-elles aux images d’entraînement ?
- Une prédiction incertaine peut-elle être envoyée à un humain ?

## 1.13. Quelle tâche choisir ?

| Sortie à produire | Tâche |
|--------------------------------|-----------------|
| Une classe pour l'image entière | Classification |
| Plusieurs catégories simutanées | Classification multilabel |
| Une classe et une position pour chaque objet | Détection |
| Une classe pour chaque pixel                 | Segmentation sémantique |
| Un masque séparé pour chaque objet           | Segmentation d'instances |
| Des articulations ou points précis           | Estimation de pose       |
| Une distance par pixel                       | Estimation de profondeur |
| La trajectoire des objets dans une vidéo     | Tracking                 |
| Du texte présent dans une image              | OCR                      |
| Une description textuelle de l'image         | Modèle vision-langage    |

## 1.14. À retenir

- Une image est une structure numérique organisée spatialement.
- Un pixel peut représenter une couleur, une intensité ou une autre mesure physique.
- Les dimensions et l’ordre des canaux doivent être vérifiés.
- La variabilité des conditions rend la reconnaissance visuelle difficile.
- Le choix de la tâche dépend de la sortie métier attendue.
- Classification, détection et segmentation ne répondent pas à la même question.
- Le niveau d’annotation disponible limite les tâches possibles.
- Les annotations détaillées sont plus coûteuses.
- Le dataset doit représenter les futures conditions d’utilisation.
- Les images corrélées doivent être regroupées lors du split.
- Un modèle complexe ne compense pas automatiquement un dataset mal construit.





























