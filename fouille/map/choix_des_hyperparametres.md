<!--
Title: "Procédures d'apprentissage - Entraînement et tuning - Choix des hyperparamètres (C, λ, k, γ)"
Author: rsquaredata
Last updated: 2025-12-03
-->

# Choix des hyperparamètres


Le **choix des hyperparamètres** est l'étape centrale du **tuning** (réglage fin) et est fondamental pour la 
*procédure d'apprentissage* des modèles de *Machine Learning* (ML), car ces paramètres déterminent la complexité du 
modèle et gèrent le compromis entre la minimisation du risque empirique et la capacité de généralisation.

Il existe plusieurs hyper-paramètres clés utilisés dans les algorithmes fondamentaux de ML, ainsi qu'une méthodologie 
rigoureuse employée pour les optimiser.

## 1. Hyperparamètres spécifiques aux Séparateurs à Vaste Marge (SVM)

Les SVM sont des algorithmes fondamentaux dont l'apprentissage dépend de plusieurs hyper-paramètres majeurs : 
$C$, $\lambda$ et $\gamma$.

### 1.1. Le paramètre de régularisation $C$ (Soft Margin SVM)

Dans la formulation du Soft Margin SVM, le paramètre $C$ est l'hyperparamètre qui fait l'équilibre entre la 
**maximisation de la marge** et la **pénalisation des erreurs** (variables slacks $\eta_i$).

- **Rôle** : $C$ contrôle l'importance que l'on accorde aux erreurs effectuées par le modèle par rapport aux poids du 
modèle $w$ (sa complexité). Si $C$ est très grand, le modèle sera plus sensible aux erreurs d'entraînement et cherchera 
une marge plus petite; si $C$ est petit, la régularisation sera plus forte, privilégiant une marge vaste au détriment 
de quelques erreurs d'entraînement.

- **Tuning requis** : Le paramètre $C doit être **tuné** durant le processus d'apprentissage.

- **Exemples pratiques**: Pour l'apprentissage d'un SVM avec noyau linéaire, il est demandé de tuner $C$ parmi un 
ensemble de valeurs, typiquement {0.1,0.5,1,2,4} en utilisant la validation croisée (5-CV).

### 1.2. La constante de régularisation $\lambda$ (Formulation RRM)

Le paramètre $\lambda$ est la constante de régularisation utilisée dansla formulation générale du problème de 
**minimisation du risque empirique régularisé** (RRM) : $\frac{1}{m} \sum \ell (w, w_i) + \lambda f(w)$.

- **Relation avec $C$** : Dans le cas du SVM, $\lambda* est inversement proportionnel à $C$ selon la relation 
$\lambda = \frac{1}{2}C$.

- **Contrôle de complexité** : $\lambda$ permet de contrôler la taille ou la complexité de l'espace d'hypothèses en 
contraignant la norme des paramètres.

	- Plus $\lambda$ est grand, plus on donne de l'importance au contrôle de la complexité (régularisation) ; le risque 
est d'arriver à du sous-apprentissage (*under-fitting*).

	- Plus $\lambda$ est petit, plus on se concentre sur l'ajustement aux données, augmentant le risque de 
sur-apprentissage (*over-fitting*).

- **Généralisation théorique** : La borne de généralisation établie sur la base de la stabilité uniforme montre que la 
constante de stabilité $\beta$ est inversement proportionnelle à $\lambda ($\beta = \frac{2 \sigma^2}{\lambda m}). 
Cela signifie que l'ajustement de $\lambda$ est directement lié à la solidité des garanties théoriques de 
généralisation.

- **Boosting et approximation* : $\lambda$ est également utilisé dans des algorithmes plus complexes comme *XGBoost** 
(Gradient Tree Boosting) pour contrôler le nombre et le poids des feuilles, servant de terme de régularisation. Dans le 
Gradient Boosting avec RFF (Random Fourier Features), $\lambda$ est aussi un hyperparamètre de régularisation qui doit 
être tuné, bien qu'il soit parfois fixé (par exemple, $\lambda = 1$) pour les implémentations initiales.

### 1.3. Le Paramètre $\gamma$ (noyau gaussien)

Le paramètre $\gamma$ est spécifique au noyau gaussien (aussi appelé noyau radial ou `rbf` sous Python) et contrôle la 
similarité entre deux exemples.

- **Rôle** : $\gamma$ contrôle l'importance que l'on donne à la distance entre les points. Si $\gamma$ est grand, on 
accorde moins d'importance à la distance entre les exemples, ce qui tend à lisser la frontière de décision.

- **Tuning nécessaire** : Pour un SVM avec noyau gaussien, il est indispensable de tuner simultanément l'hyperparamètre 
$C$ et le paramètre \gamma$.

- **Exemples pratiques** : On tune $\gamma$ parmi des valeurs telles que $\{0.01, 0.1, 1, 10\}$.

## 2. Hyperparamètres du $k$-Plus Proches Voisins ($k$)

L'algorithme du k-Plus Proches Voisins (k-NN) est non paramétrique, mais il dépend fortement du paramètre $k$, le 
nombre de voisins considérés pour la prédiction.

- **Rôle** : La valeur de $k$ influence la prédiction par vote majoritaire. Un petit $k$ (par exemple $k=1$) rend 
l'algorithme très sensible aux variations locales, tandis qu'un grand $k$ tend à lisser la décision et peut assigner 
la classe majoritaire à toutes les données si $k$ est proche de la taille de l'échantillon.

- **Tuning** : En pratique, le choix de la meilleure valeur de $k$ est généralement fait à l'aide d'une procédure de 
**cross-validation**.

## 3. Hyperparamètres dans le Deep Learning et les arbres de décision

D'autres algorithmes possèdent de nombreux hyperparamètres structuraux et d'optimisation qui doivent être réglés :

- **Réseaux de neurones (Deep Learning via H2O**/Keras) : La phase de *tuning* est exploratoire et touche à la fois la 
structure et l'optimisation. Les hyperparamètres incluent :

	- **Structure** : Le nombre de couches cachées et le nombre de neurones par couche (par exemple, trois couches de 
40, 20 puis 10 neurones).

	- **Fonction d'activation** : Par exemple, la fonction **"Tanh"**.

	- **Optimisation** : Le nombre d'**epochs* (par exemple, 50 epochs), le **pas d'apprentissage* (par exemple, 
$0.01$), la taille minimale des *batches* (avec équilibrage des classes, taille minimale $50$), l'utilisation du 
**dropout*, et l'introduction de termes de **régularisation** $L_2$.

- **Arbres de décision** : Pour éviter le sur-apprentissage, les arbres ne sont pas construits jusqu'à l'obtention de 
feuilles pures. Pour cela, on joue sur des hyperparamètres tels que la **profondeur maximale de l'arbre**, le **nombre 
minimum d'exemples dans une feuille**, ou le **gain minimum** requis pour effectuer une séparation. 

## 4. Méthodologie du tuning : La validation croisée ($k$-fold CV)

L'approche pour déterminer la valeur optimale de tous ces hyperparamètres est standardisée et s'inscrit dans le 
protocole expérimental.

- **Nécessité du tuning** : Le réglage des hyperparamètres est vital pour trouver le bon compromis entre l'erreur sur 
l'ensemble d'entraînement et le risque de généralisation.

- **Procédure standard** : La méthode privilégiée est la **validation croisée en $k$ plis (_$k$-fold CV_)**. Cette 
méthode permet de **parcourir la totalité de l'ensemble d'entraînement** pour la validation, accédant ainsi à une plus 
grande partie de la distribution sous-jacente des données, ce qui tend à donner de meilleurs résultats pratiques.

- **Règle de base** : Dans la phase exploratoire (comme pour le tuning des réseaux de neurones), il est crucial de 
**ne  changer les paramètres que un par un** pour observer l'effet de chaque modification sur le score.

- **Documentation** : Le protocole expérimental doit explicitement spécifier le **range des hyperparamètres employés** 
ainsi que la façon dont ils sont optimisés (par exemple, *"cross-validation en k-folds*, *simple validation*, ou 
choix de les fixer).

---

En résumé, que ce soit par l'ajustement de $C$ et $\gamma$ pour les SVM, ou par la recherche du meilleur $k$ pour les 
k-NN, le **choix des hyperparamètres** est indissociable d'un processus de tuning rigoureux, majoritairement basé sur 
la validation croisée, et doit être justifié par l'impact que ces paramètres ont sur la complexité et la performance 
du modèle.







