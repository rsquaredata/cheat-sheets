# SLIDES (10 minutes)

---

## SLIDE 1 - Titre (30 sec)
#### Marin
> Bonjour, merci. Nous allons vous présenter *mmrClustVar*, un package R complet pour le clustering de variables.

*(question anticipée) : "Pourquoi un package plutôt qu'un simple script ?"*
> On a choisi un package plutôt qu‘un script car le sujet demandait une architecture propre, modulaire, extensible, et facilement testable. La structure package + R6 répond parfaitement à ces objectifs.

---

## SLIDE 2 - Objectifs du projet (45 sec)
#### Mazilda
> Le premier objectif était de réimplémenter des méthodes de clustering de variables dans une logique pédagogique.
Le second était de concevoir un package **robuste**[^1], avec une API[^2] homogène, une façade utilisateur, et une application Shiny pour l‘exploration.

*(question anticipée) : "Pourquoi ces méthodes ?"*
> On a choisi des méthodes classiques et complémentaires : k-means, k-modes, k-prototypes et k-medoids, ce qui couvre les cas numérique, catégoriel et mixte.

---

## SLIDE 3 - Cahier des charges (45 sec)
#### Rina
> Le cahier des charges imposait trois méthodes dont une de réallocation[^3] et une pour les variables catégorielles, R6, et une app Shiny.  
> On a étendu le périmètre : quatre méthodes, une façade complète, des outils de diagnostic, et un dataset mixte fait pour la démonstration.

*(question anticipée) : "Pourquoi quatre méthodes ?"*
> Parce que chaque méthode correspond à un type de variable : numérique, catégoriel, mixte, et dissimilarités générales. On voulait couvrir 100 % des cas d‘usage.

---

## SLIDE 4 - Architecture (2 min)
#### Marin (40 sec)
> La classe mère `ClusterBase` gère tout ce qui est commun : préparation des données, standardisation, structures internes, inertie, convergence, résumé, plots, adhésion.  
> Elle fournit aussi des "hooks"[^4] ou methode abstraite que les classes filles spécialisent.

*(question anticipée) : "Pourquoi une classe mère ?*
> Parce que 80 % du travail est identique entre les méthodes, et il était important d‘éviter la duplication. (DRY - Don't repeat yourself)

#### Rina (40 sec)
> Les classes filles (`KMeans`, `KModes`, `KPrototypes`, `KMedoids`) implémentent leur moteur via `run_clustering()`.  
> Chaque méthode définit son prototype, son critère d'affectation et son calcul d‘adhésion.

*(question anticipée) : "Comment se fait la spécialisation ?"*
> Chaque fille réimplémente uniquement ce qui change - par exemple, r$r^2$ pour Kmeans, le simple matching pour Kmodes, et la combinaison pondérée pour k-prototypes.

#### Mazilda (40 sec)
> Enfin, la classe `Interface` joue le rôle de façade : elle choisit automatiquement la méthode, applique `fit`, fournit `summary` et `plot`, et calcule la courbe d‘inertie.

*(question anticipée) : "Pourquoi une façade ?"*
> Pour que l'utilisateur final n‘ait qu‘une seule API, quelle que soit la méthode effectivement utilisée.

---

## SLIDE 5 - Les méthodes (3 min)
#### Marin - K-means (50 sec)
> K-means fonctionne sur les variables numériques : le prototype est la PC1 locale, et la distance est 1 − r².  
> C‘est idéal pour découvrir des axes latents[^5] linéaires.

*(question anticipée) : “Pourquoi la PC1 locale[^6] comme prototype ?”*
> Parce qu‘elle maximise la variance expliquée[^12] dans chaque cluster, ce qui crée un vrai facteur synthétique par groupe.

#### Rina - K-modes (50 sec)
> Pour les variables catégorielles, on utilise un profil modal.  
> La distance, c‘est la proportion de désaccords entre les modalités (simple matching).

*(question anticipée) : "Pourquoi pas Gower[^7] ?"*
> Gower serait redondant ici : le simple matching est le standard dans les algorithmes de type *modes*.

#### Mazilda - K-prototypes (1 min)
> Pour les données mixtes, on combine la artie numérique et la partie catégorielle avec un paramètre λ.  
> C‘est la méthode la plus utile dans notre dataset, puisqu'on a beaucoup de variables mixtes.

*(question anticipée) : "Comment choisir λ ?"*
> On a testé une plage raisonnable, et on a observé que le résultat était stable pour λ ∈ [0.5 ; 2].  
> La valeur par défaut λ=1 est celle du papier de référence de Huang[^13].

#### Marin - K-medoids (20 sec)
> k-medoids s’appuie sur une matrice de dissimilarité générale, sans supposer un espace vectoriel.
#### Rina (20 sec)
> Le prototype de cluster est une variable réelle (un médoïde), ce qui rend la méthode robuste aux valeurs extrêmes et aux distributions bizarres.
#### Mazilda (20 sec)
> Le coût est quadratique en nombre de variables, donc plus lourd, mais c’est une excellente méthode de référence pour valider les solutions des méthodes plus rapides.

*(question anticipée) : "Pourquoi l‘avoir inclus si c‘est lent ?"*
> Parce qu‘il permet de valider les résultats obtenus par les méthodes plus rapides, surtout sur petits p.

---

## SLIDE - Étude de cas metal_universe (2 min)
#### Rina
> On a créé metal_universe, un dataset mixte pour tester toutes les méthodes.  
> Il contient des variables musicales numériques (BPM, énergie, distorsion), des catégories (sous-genre, pays, voix), des variables dérivées, et du bruit.
> Avec K = 5, on identifie clairement les blocs thématiques : rythmique, agressivité/distorsion, socio-géographique, dérivées et bruit.

*(question anticipée) : "Pourquoi un dataset fictif ?"*
> Pour maîtriser la structure latente[^8], afin d'interpréter plus facilement les résultats.

---

## SLIDE - Conclusion
#### Marin (30 sec)
> Le package est cohérent avec le cahier des charges et fournit une implémentation complète des méthodes de clustering de variables.
> L’architecture est modulaire, l’API est homogène, et l’application Shiny permet une exploration interactive des résultats.
#### Rina (30 sec)
> Le dataset *metal_universe* rend l’outil concret et utilisable en pratique, notamment pour l’enseignement ou la démonstration.

---

# INSTALLATION & QUESTIONS TECHNIQUES (20 minutes)

---

## Étape 1 - Installation du package (3 min)
#### Marin installe

```r
install.packages("remotes")
# install directly from GitHub
remotes::install_github("rsquaredata/mmrClustVar")
```
#### Mazilda commente
> Le package suit la structure R standard : dossiers `R/`, `data/`, `inst/`.  
> L‘app Shiny se trouve dans `inst/shiny/mmrClustVar_app`.  
> le fichier NAMESPACE est généré automatiquement via roxygen2.

*(question anticipée) : “Pourquoi installer via GitHub et pas localement ?”
> Parce que c’est le workflow standard pour tester un package dans des conditions proches d’un usage réel, et ça simplifie la distribution.

---
## Étape 2 - Fit simple (5-6 min)
#### Rina

```r
data(mtcars)

actives <- mtcars[, c('mpg', 'disp', 'hp', 'drat', 'wt', 'qsec')]
descriptives <- mtcars[, c('cyl', 'vs', 'am', 'gear')]

obj <- Kmeans$new(K=3)
obj$fit(actives)
obj$summary()
```
> On instancie un objet de la classe `Kmeans` avec 5 clusters.  
> Dans `fit()`, l’objet crée les clusters à partir des variables actives passées en argument. Cette méthode est implémentée dans la classe parente `ClusterBase` et appelle la méthode `run_clustering()` implémentée dans chacun des algorithmes de clustering.  
> On affiche ensuite le résumé de l’instance (méthode implémentée dans la classe parente, ce qui permet d’avoir un rendu uniforme pour chacun des algorithmes).

*(question anticipée) : "Comment est calculée l‘inertie ?"*
> L‘inertie est la somme des distances intra-clusters selon le critère de la méthode : r² pour numérique, simple matching pour cat, ou combinaison pondérée pour le mixte.

*(question anticipée) : "Comment détectez-vous la convergence ?"*
> Par la stabilité des affectations d’une itération à l’autre, avec un nombre maximal d’itérations pour garantir la terminaison.

*(question anticipée) : "Et predict() ?"*
> `predict()` assigne de nouvelles variables en utilisant les prototypes du moteur, sans refitting.

---

## Étape 3 - Courbe d'inertie / elbow (3 min)
#### Mazilda

```r
obj$predict(descriptives)
obj$plot("clusters")
```

> On refitte un modèle pour chaque valeur de K, mais uniquement au niveau prototypes : on ne stocke pas de données individuelles, donc c‘est très rapide et ça reste léger en mémoire.

*(question anticipée) : "Pourquoi ne pas stocker tout dans l‘objet ?"*
> Pour éviter un surpoids mémoire[^9] et pour rester conforme aux bons *design patterns* de R6/scikit-learn : l'bjet garde l'état coutant et les diagnostics utiles, mais pas l'historique complet de tous les fits.

---

## Étape 4 - Membership & interprétation (4-5 min)
#### Rina

```r
obj$plot("membership")
obj$interpret_clusters()
```

> L’adhésion est mesurée par $r^2$ pour les variables numériques et par le matching pour les catégorielles.
> Ça permet de voir quelles variables sont très représentatives de chaque cluster, et lesquelles sont plutôt marginales.

*(question anticipée) : "Pourquoi r² et pas corr² ?"*
> $r^2$ est tout simplement la corrélation au carré : c’est la mesure standard dans ClustOfVar pour quantifier la contribution d’une variable à un axe latent.

---

## Étape 5 - Interface

Nous pouvons également instancier nos algorithmes à partir de l'interface de notre module. Cette interface regroupe les algorithmes en une seule classe et permet notamment d'utiliser la fonction de détection automatique d'algorithmes de clustering selon la nature de notre dataset. Cette interface est utilisée par notre application Shiny.

```r
data("metal_universe")

interface <- Interface$new("auto", K=3)
interface$fit(metal_universe[, 1:5])
interface$summary()
```

> Elle fonctionne de manière similaire à nos classes de base. La différence est qu'on instancie un objet général `Interface`, et on précise l'algorithme à utiliser dans l'initialisation.

```r
interface$get_method()
```

> Ici, l'algorithme K-prototypes a été sélectionné.

---

## Étape 6 - Questions générales (reste du temps)
méthode / archi profonde → Rina  
code R6 → Marin  
λ & app Shiny → Mazilda  
dataset → Rina
<!-- TODO : rédiger --> 

---

# APP SHINY + QUESTIONS (10 min)

---

## Étape 1 - Lancement (1 min)
#### Marin

```r
run_mmrClustVar_app()
```
> L‘app est embarquée dans `inst/shiny`, ce qui permet une distribution[^10] simple. dès que le package est installé, l’utilisateur a l’application à disposition.   

*(question anticipée) : "Pourquoi pas un module séparé ?"*
> Parce qu’un package conçu pour CRAN ou pour un usage partagé embarque ses apps Shiny dans `inst/shiny`, c’est la convention standard.

---

## Étape 2 - Interface générale (3 min)
#### Mazilda
> L‘utilisateur charge un dataset, choisit la méthode et le nombre de clusters K.  
> Il peut ensuite visualiser les clusters, l'inertie, l'adhésion, et différents profils graphiques.  
> L‘architecture sépare clairement le backend (les classes R6 du package) et le frontend (Shiny), ce qui maintient une bonne cohérence du code.  

*(question anticipée) : "Comment communiquent Shiny et R6 ?"*
> Par un objet R6 réactif[^11] stocké dans l'environnement serveur : Shiny ne fait qu’appeler ses méthodes publiques, et se met à jour automatiquement lorsque l’état de l’objet change.

---

## Étape 3 - Analyse sur metal_universe[^19] (5 min)
#### Rina
> On retrouve les blocs identifiés dans la partie précédente.
> Les graphiques membership et profils aident à interpréter chaque cluster en termes de variables musicales.  
> On voit que les variables de bruit ont en général une adhésion faible, ce qui valide la robustesse du modèle.

*(question anticipée) : "Pourquoi ce dataset fait ressortir bien les clusters?"*
> Parce qu'il a été construit pour que les structures numériques et catégorielles soient claires mais pas triviales : il y a une vraie structure, mais aussi du bruit.  

---

# Q&A

### 1. Pourquoi avoir utilisé la PC1 locale comme prototype en k-means ?
Vous n'avez pas l'impression que c'est arbitraire ?
> Parce que la PC1 locale maximise la variance expliquée dans chaque cluster. C‘est le prototype optimal dans le cadre du clustering de variables numériques : il synthétise le comportement des variables du groupe en un seul axe latent. Ce n‘est pas arbitraire, c‘est exactement la définition utilisée dans la littérature, notamment ClustOfVar. 

### 2. Votre λ dans k-prototypes est totalement arbitraire.
Vous dites que "ça marche". Mais scientifiquement, pourquoi ?
> λ sert uniquement à remettre sur la même échelle les composantes numérique et catégorielle. La littérature montre que tant qu‘on reste dans un intervalle raisonnable, le partitionnement est stable[^13]. Nous avons vérifié expérimentalement cette stabilité sur notre dataset. Donc λ n‘est pas un hyperparamètre d‘optimisation, mais un facteur d‘équilibrage. 

### 3. Votre méthode auto n‘a rien d'"intelligent".
En réalité elle choisit juste en fonction des types de colonnes.  
Pourquoi ne pas détecter si les données numériques sont corrélées, ou si les catégories sont nombreuses ?
> Parce que c‘est la décision la plus fiable et la plus déterministe pour l‘utilisateur. Le clustering de variables s‘appuie d‘abord sur la nature des variables : on ne peut pas appliquer k-means sur du catégoriel, ni k-modes sur du numérique. Chercher à inférer la structure avant le clustering introduirait une heuristique qui pourrait être fausse dans certains cas. Ici, la priorité était la robustesse et la transparence. 

### 4. Dans votre Shiny, comment garantissez-vous que la séparation entre logique métier et interface est réelle ?
Je crains que vous n‘ayez du code du moteur directement dans l‘app.
> La logique métier est entièrement encapsulée dans les classes R6. Shiny ne manipule que :
> - un objet `Interface`,
> - ses méthodes publiques : `fit`, `summary`, `plot`, `interpret_clusters`.  

> Il n‘y a aucune fonction algorithmique dans l‘app.  
> L‘app n‘est qu‘une interface : tout le calcul est fait par le package.

### 5. Expliquez-moi exactement comment vous gérez les NA.
Je veux du concret, pas "on enlève les lignes".  
Qu‘est-ce qui se passe si j‘ai 30 % de NA dans une variable numérique ?
> Les NA sont gérées en deux temps[^14] :
>     1. on enlève uniquement les lignes qui empêchent le calcul numérique (prcomp ne tolère pas les NA) ;
>     2. pour les dissimilarités ou matching, les NA sont ignorés dans les comparaisons paire-à-paire.
> Si une variable contient 30 % de NA, elle reste utilisable : seuls les couples observés contribuent aux distances.

### 6. k-medoids est quadratique[^15].
Pourquoi avoir mis une méthode inutile dans un package pédagogique ?
>  k-medoids a deux intérêts pédagogiques forts :
> - c‘est la seule méthode qui fonctionne même lorsque les distances ne proviennent pas d‘un espace vectoriel,
> - c‘est une référence robuste pour valider les solutions des méthodes plus rapides. Avec une centaine de variables maximum, le coût est acceptable.

### 7. Votre predict() est faible[^16].
En réalité, vous réappliquez juste la distance prototype → variable.  
Pourquoi ne pas recalculer une partition locale optimisée pour les nouvelles variables ?
> Parce que predict() ne doit pas modifier la partition : il doit projeter de nouvelles variables sur une structure existante.
> Recalculer une partition locale reviendrait à changer le modèle, ce qui violerait le principe d‘un prédicteur.
> Notre predict() applique donc logiquement le critère d‘affectation utilisé dans le fit. 

### 8. Votre interface R6 est "inspirée de scikit-learn".
Mais est-ce vraiment conforme ?  
Quelles sont les différences fondamentales entre votre design et celui d‘un estimater sklearn ?  
> Elle y ressemble parce qu'on a : un objet instanciable, une méthode `fit`, une méthode `predict` et une interface uniforme entre plusieurs méthodes.
> La différence principale, c'est que scikit-learn utilise des modèles immuables, alors que R6 est mutable ;
> - sklearn sépare totalement fit/predict, alors que R6 permet davantage de flexibilité interne[^17] - même si, dans notre cas, on garde un `predict` pur.

### 9. Expliquez la différence exacte entre inertie 'within' et inertie totale.
Et dites-moi en une phrase comment on déduit la variance expliquée.
>  L‘inertie totale est la somme des distances de chaque variable au prototype global.
> L‘inertie within est la somme des distances aux prototypes de cluster.
> La variance expliquée est simplement : (inertie totale – inertie within) / inertie totale[^18].

### 10. Votre dataset metal_universe est "fictif".
Pourquoi devrais-je lui accorder la moindre valeur scientifique ?
> Il vaut scientifiquement ce pourquoi il a été conçu : tester des méthodes de clustering mixtes dans un cadre contrôlé, où l‘on connaît la structure latente.  
> C‘est exactement le même principe que les jeux de données jouets utilisés en classification ou en clustering.
> Il sert à illustrer, valider, expliquer.

### 11. Votre architecture R6 est jolie, mais elle masque les vrais défauts algorithmiques.
Expliquez-moi en quoi votre design règle un problème réel, et pas seulement esthétique.
> Notre design résout trois problèmes concrets :
> - réduction de duplication : 80% du pipeline est identique entre méthodes.
> - séparation nette des responsabilités : préparation / moteur /interface.
> - extensibilité garantie : ajouter un nouvel algorithme nécessite seulement une classe fille.

> Ce n'est pas esthétique, c'est une manière d'éviter la fragmentation du code.

### 12. Votre implémentation ‘run_clustering‘ dans les classes filles contient du code répétitif.
Pourquoi ne pas avoir tout factorisé dans ClusterBase ?
> Parce que chaque méthode a :  
> - un prototype différent,  
> - une distance différente,  
> - un critère d‘optimisation différent,  
> - une logique d‘adhésion différente.  

> La seules parties factorisables le sont déjà. Déplacer davantage dans la classe mère créerait du couplage et du code trop général, moins lisibles et plus difficiles à maintenir.

### 13. prcomp()[^20] ne fonctionne pas sur toutes les structures de données. Pourquoi l‘avoir choisi ?
> Parce qu‘il fournit exactement ce dont on a besoin :  
> - un axe principal robuste,  
> - une sortie stable et compatible avec la définition du prototype numérique.  

> Les alternatives (SVD manuel, PCA maison) ajoutent de la complexité algorithmique sans gain réel dans ce contexte.

### 14. Votre gestion de NA me semble naïve. Pourquoi ne pas imputer ?
> Parce que l‘imputation introduit un biais dans les structures de corrélation entre variables, ce qui fausse directement le clustering de variables[^21].  
> Ignorer les NA dans les matching, et filtrer uniquement ce qui empêche le calcul matriciel, est la stratégie la plus neutre et théoriquement la plus propre.

### 15. k-medoids est robuste, mais votre distance mixte pour k-medoids n‘est pas métrique. Justifiez.
> Dans un cadre de clustering de variables, la métricité stricte est moins importante que la cohérence de l‘ordre induit. Notre distance combine r² et matching, qui sont tous deux des mesures monotones.
> Ce n‘est pas une distance métrique, mais une **dissimilarité cohérente**, ce qui suffit pour PAM. C‘est explicitement mentionné dans la littérature[^22].

### 16. Votre predict() ne renvoie aucune incertitude. Pourquoi ne pas fournir un score de confiance ?
> Parce que la notion d‘incertitude n‘a pas de base théorique solide dans le clustering de variables : ce n‘est pas un modèle probabiliste[^23].  
> À la place, nous fournissons les **indicateurs d‘adhésion**, qui mesurent la cohérence entre la variable et le prototype. C‘est l‘équivalent interprétable dans ce cadre.

### 17. Pourquoi ne pas avoir mis votre méthode dans un S4 formel avec signatures ? R6 est trop permissif.
> R6 est plus adapté pour les objets comportant un état mutable[^24], comme dans sklearn ou TensorFlow.  
> S4 est pertinent pour des structures mathématiques stables, mais trop rigide pour un pipeline dynamique comme un clustering interactif.  
> R6 permet :  
> - une façade simple,  
> - une API unifiée,  
> - des objets modifiables en Shiny.  

> C‘est le choix le plus cohérent pour un outil utilisateur final.

### 18. Votre méthode auto ne vérifie pas l‘homogénéité des données.  
Pourquoi ne pas tester si les variables numériques sont sur des échelles trop différentes, ou si certaines sont triviales ?
> Parce que ce n‘est **pas** à la méthode de choisir si l‘utilisateur veut standardiser ou exclure des variables triviales.  
> On fournit l‘option `scale = TRUE` qui règle automatiquement le problème des échelles.  
> Quant aux variables triviales (variance faible)[^25], c‘est une décision d‘analyse de données, pas une décision algorithmique.  
> Notre rôle est de donner un outil, pas de prendre les décisions analytiques à la place de l‘utilisateur.

### 19. Votre package n‘est pas optimisé pour les très grands p.  
Est-ce un échec de conception ?
> Non, le projet cible un usage pédagogique pour p < 200.  
> Optimiser pour des structures massives impliquerait des algorithmes totalement différents (mini-batch, approximations, sketching)[^26], ce qui sortirait du cadre académique.  
> Notre design est optimal **pour le périmètre choisi**.

### 20. Votre app Shiny externalise tout.  
Pourquoi ne pas avoir mis un mode ‘expert‘ avec tuning avancé ?
> Parce que l‘objectif principal était l‘exploration pédagogique.  
> Le tuning avancé (sélection automatique de λ, bootstrap de stabilité, visualisations complexes)[^27] serait possible, mais non conforme à la simplicité demandée dans le cahier des charges.  
> Nous avons optimisé pour la clarté, pas pour l‘exhaustivité.

---

[^1]:
    Package robuste : package qui  
        - ne plante pas même si le user fournit des données imparfaites,  
        - isole les responsabilités (archi propre, no spaghetti code),  
        -  gère correctemnt les erreurs (messages clairs, pas de crash silencieux),  
        -  donne des résultats cohérents, reproductibles, même sur différents jeux de données,  
        -  accepte l'entrée utilisateur san exiger un format parfait (ex.: types cohérents, NA gérés),  
        -  support aisément l'extension

[^2]: API (*Application Programming Interface*) : **ensemble cohérent de méthodes** par lesquelles un utilisateur interagit avec un logiciel. Le package `mmrClustVar`fournit une API car l'utilisateur n'a besoin de connaître que `fit()`, `predict()`, `summary()`, `plot()`, `get_clusters()`, etc. ; ces méthodes ont les **mêmes noms et la même logique** pour toutes les méthodes de clustering, donc une **interface uniforme** qui masque la complexité interne.

[^3]: Méthode de réallocation : algorithme qui, à chaque itération, réattribue chaque variable à un cluster ; nos 4 méthodes ont une étape de réallocation.

[^4]: Hook : point du code prévu pour être surchargé ou réimplémenté par une classe fille (ex.: `ClusterBase$run_clustering()` est un hook, la classe mère le déclare et les classes filles fournissent leur version.

[^5]: Axe latent : dimension cachée qui n'est pas directement observées, mais qui résume plusieurs variables corrélées (ex.: en musique, BPM + énergie + loudness → un axe latent "intensité") ; la PC1 (première composante principale) est un axe latent.

[^6]: PC1 locale : la première composante principale calculée uniquement sur les variables du cluster. Elle représente l'axe principal de variation dans ce cluster. Ex. : Cluster = {BPM, loudness, énergie} → PC1 locale = axe qui résume ces trois variables.

[^7]: Gower : mesure de distance mixte qui compare les nums, compare les cat, mélange tout dans une distance unique. Très utilisée pour les données mixtes.

[^8]: Structure latente : organisation interne **non observable directement** mais que le clustering cherche à révéler. Ex.: pour metal_universe, des groupes logiques "agressivité", "origine géographique" ; on ne les donnes pas explicitement, ils **existent dans les corrélations**.

[^9]: Surpoids mémoire : stocker un modèle par valeur de K dans le même objet → dangereux si K=1..100 → explosion RAM. Donc on calcule et on jette chaque modèle dans la boucle - pas de stocjage permanent.

[^10] : Distribution : quand on exporte/installe le package, l'app Shiny est incluse automatiquement, donc le package est "distribuable" (on peut donner le package à qqn et il a aussi l'app).

[^11] : Objet réactif : dans Shiny, objet recalculté automatiquement quand ses entrées changent ; on ne recharge pas l'app, tout se met à jour. Notre objet R6 est stocké dans `server`, conservé d'un clic à l'autre, et déclenche des recalculs réactifs quand on sélectionne un autre K, un autre dataset, etc.`

[^12]: La PC1 locale maximise la variance expliquée : la premi§er composante principale est l'axe qui **maximise la variance projetée** des données. Donc, dans un clluster, PC1 locale = axe qui explique le plus de variance parmi les variables du cluster.

[^13]: Huang, *Clustering large data sets with mixed numeric and categorical values*, 1997 : λ est décrit comme un **coefficient d'équilibrage**, pas un hyperparamètre à tuner, et les résultats sont stables tant que λ reste dans une plage raisonnable.

[^14]:
    Gestion des NA dans le code à deux endroits :
        - pour `prcomp` : la classe mère filtre les lignes NA avant PCA
        - pour matching / distance : dans les internes de calcul des distances catégorielles et mixtes

[^15]:
    k-medoids est quadratique parce que :
        - il compare **toutes les paires d'éléments** pour choisir les médianes → coût en O(n²).
        - PAM = *Partition Around Medoids* → double boucle pour choisir le meilleur médoïde.
    On a donc p² comparaisons faites pour p variables → quadratique.

[^16]: "Votre `predict()`est faible" : cette critique veut dire que `predict()` **ne donne pas d'indicateur de confiance**, juste une affectation. Le clustering de variables n'étant pas un modèle probabiliste, il n'y a pas de vraie "confiance".

[^17]: Flexibilité de R6 : dans skleanr, `fit`construit un modèle et `predict`doit être pur, sans modifier l'objet. Dans R6, un objet est **mutable** (peut **changer d'état**), donc `fit`/`predict` pourraient modifier l'objet (pas dans notre package, `predict` est pur).

[^18]: Théorème de Huygens : variance totale = variance intra + variance inter ; donc si l'inertie intra diminue et l'inertie inter augmente → clusterisation stable.

[^19]:
    Caractéristiques du dataset metal_universe :
        - 100 groupes de metal (existants)  
        - ~ 100 variables :  
          - numériques : BPM, énergie, loudness, spectral features  
          - catégorielles : sous-genre, pays, type de voix, thème  
          - bruit : variables décorrélées  
        - structure latente claire (5 blocs)

[^20]:
  `prcomp`: fonction R standard pour calculer une ACP. `prcomp()` calcule la **décomposition en composantes principales** d'un jeu de données numérique, càd qu'elle :  
      - **centre** les variables  
      - **standardise** (si on lui demande `scale = TRUE`)  
      - calcule une **décomposition SVD**
      - produit :
          - les **axes principaux** (PC1, PC2, ...)  
          - la **variance expliquée** par chaque axe  
          - les **scores projetés**  
          - les **loadings** (poids de chaque variable dans chaque axe)  
    On utilise `prcomp()` dans mmrClustVar parce que dans un clustering de variables, un prototype de cluster est une PC1 locale, et c'est propre et stable de calculer la PC1 avec. Donc on prend les variables du cluster, on applique `prcomp` et on prend la **première composante = axe latent** qui résume le cluster (c'est *exactement* la définition de ClustofVar).

[^21]: Imputer = remplacer les NA (par mean imputation, regression imputation, knn imputation, etc.) C'est mauvais ici car l'imputation **change les corrélations**, donc **déforme la structure** du clustering de varaibles (qui dépend uniquemebt des corrélations ou distances). Pas d'imputation = respect de la structure réelle.

[^22]:
    Distance mixte de k-medoids :
        - Une distance métrique est une distance qui respecte 4 propriétés :
            - positivité  
            - identité des indiscernables  
            - symétrie  
            - inégalité triangulaire  
        Or la combinaison r² + matching ne garantit pas l'inégalité triangulaire, donc notre distance mixte pour k-medoids n'est pas une vraie métrique.
        - Ordre induit : même quand la distance n'est pas métrique, elle impose un **ordre des dissimilarités** (si A est plus proche de B que C, l'ordre est valide).
        - Mesure monotonone : r² décroissant et matching sont tous deux monotones → cohérence de l’ordre.
        - PAM (*Partitioning Around Medoids*) : algorithme de k-medoids qui accepte n’importe quelle **dissimilarité**, même non métrique (Kaufman & Rousseeuw, *Finding Groups in Data*, 1990).

[^23]: Incertitude : en classification probabiliste, `predict()` peut renvoyer : pribablité d'appartenance, intervalle de confiance, score de vraisemblance. Dans le clustering, ce n'est pas possible : il n'y a pas de vraisemblance → pas de probas → pas d'incertitude. Donc on donne l'**adhésion** (r² ou matching), qui est ici l'équivalent interprétable.

[^24]: Mutable = modifiable après création.

[^25]: Variables triviales = variance faible : Une variable quasi constante → n’apporte aucune information → dégrade la PCA. Ex.: score = 0, 0, 0, 1, 0 → variance quasi nulle → axe latent inutile.

[^26]:
    Méthodes utilisées pour scaler sur très grands p :
        - **mini-batch k-means** : calcule les centres sur des petits sous-échantillons.  
        - **approximation** : algorithms rapides mais approximatifs.  
        - **sketching** : projette les données dans un espace plus petit tout en préservant les distances.

[^27]:
    - Bootstrap de stabilité : on refait le clustering sur plusieurs échantillons bootstrappés ; si les clusters restent similaires → modèle stable (cf. test de stabilité dans le notebook de tests).
    - Exemples de visualisations complexes : matrice de corrélations par cluster ; heatmaps des adhésions ; graphes bipartites variables-clusters ; arbres hiérarchiques sur prototypes.
    - Sélection automatique de λ : en théorie, il s'agit de chercher la valeur de λ qui **maximise la stabilité** ou **minimise l'inertie intra** dans un espace mixte. Conceptuellement : on teste λ sur une grille, on évaue l'inertie tital ou la stabilité, et on choisit le λ qui rend les clusters les plus stables.











