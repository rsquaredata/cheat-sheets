<!--
Title: "Concepts fondamentaux de ML - Optimisation et régularisation - Fonctions convexes/lipschitziennes"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Fonctions convexes/ Fonctions lipschitziennes

Les concepts de fonctions convexes et de fonctions lipschitziennes sont des fondements mathématiques essentiels dans le contexte de l'optimisation et de la régularisation en Machine Learning. ls garantissent la solubilité des problèmes d'optimisation et sont au cœur de l'établissement des bornes théoriques de généralisation.

## 1. Fonctions convexes : La clé de l'optimisation en ML

La convexité est une propriété structurelle des fonctions qui est fondamentale pour l'apprentissage machine.

### Définition et caractérisation

Une fonction $f: \mathbb{R}^d \to \mathbb{R}$ est dite **convexe** si, pour tout $x, x' \in \mathbb{R}^d$ et pour tout $t \in [0,1]$, elle vérifie l'inégalité suivante :

$$
f\left( tx + (1-t)x') \right) \le tf(x) + (1-t)f(x').
$$

Cette définition signifie graphiquement que la corde reliant deux points de la fonction se situe toujours au-dessus de la courbe.

Pour les fonctions deux fois continûment différentiables, la convexité est caractérisée par la matrice Hessienne ($H$). Une fonction $f$ est convexe si et seulement si sa Hessienne, $\nabla^2 f(u)$, est une matrice **semi-définie positive (PSD)**, c'est-à-dire $u^\top Hu \ge 0$ pour tout $u$.

### Importance dans l'optimisation et le risque

La convexité est capitale pour l'optimisation des modèles d'apprentissage :
1. **Garantie du minimum global** : Pour les fonctions convexes, tout minimum local est aussi un minimum global. Cela signifie que lorsqu'un algorithme d'optimisation trouve un point où le gradient s'annule (point critique), il est assuré que ce point est la solution optimale recherchée.
2. **Solubilité des problèmes** : Le problème central du ML est la **minimisation du risque empirique régularisé** (RRM). Pour le Soft Margin SVM (Séparateur à Vaste Marge), la fonction objectif (basée sur la *Hinge Loss*) est un problème d'optimisation convexe. L'absence de convexité rendrait la minimisation du risque 0−1 **NP-difficile**.
3. **Algorithmes de descente de gradient** : Les algorithmes basés sur la descente de gradient, comme ceux utilisés en régression logistique, bénéficient de la convexité pour garantir leur convergence.

## 2. Fonctions lipschitziennes : La clé de la stabilité

Le concept de fonction lipschitzienne est directement lié à la mesure de la robustesse d'un algorithme et est essentiel pour la théorie de la généralisation basée sur la stabilité uniforme

### Définition

Une fonction $f: \mathbb{R}^d \to \mathbb{R}$ est dite **lipschitzienne** s’il existe une constante $C \gt 0$ telle que pour tout $x, x' \in \mathbb{R}$, nous avons :

$$
\Vert f(x) - f(x') \Vert \le C \Vert x-x' \Vert.
$$

La constante $C$ est appelée constante de Lipschitz et borne la pente maximale de la fonction.

### Rôle dans la $\sigma$-admissibilité et la stabilité

La lipschitzianité est cruciale pour l'établissement des garanties théoriques, notamment par la propriété de $\sigma$-admissibilité :

1. **Définition de la $\sigma$-admissibilité** : Une fonction de perte $\ell(w,z)$ est dite $\sigma$-admissible si elle est à la fois convexe par rapport à son premier argument ($w$) et lipschitzienne par rapport à cet argument, avec une constante de Lipschitz $\sigma$.
2. **Lien avec la stabilité uniforme** : L'hypothèse de $\sigma$-admissibilité est fondamentale pour prouver qu'un algorithme d'apprentissage, comme le SVM, est uniformément stable. La stabilité uniforme quantifie à quel point l'hypothèse apprise change lorsqu'un seul exemple est modifié dans l'ensemble d'entraînement.

## 3. Application à la régularisation et aux bornes de généralisation

La combinaison de la convexité et de la lipschitzianité sous la forme de la $\sigma$-admissibilité a un impact direct sur la constante de stabilité uniforme $\beta$ et donc sur la qualité de la généralisation.

- **Détermination de $\beta$ **: Dans le cadre des problèmes d'optimisation convexes régularisés (comme les SVM), la constante de stabilité uniforme $\beta$ est directement fonction de la constante de Lipschitz $\sigma$ de la *loss* $\ell$ et du paramètre de régularisation $\lambda$.
- **La formule de stabilité** : Pour le SVM, on montre que $\beta = \frac{2 \sigma^2}{\lambda m}$.
- **Contrôle de la généralisation** : La stabilité uniforme permet d'établir une borne de généralisation (*théorème des bornes via stabilité uniforme*) qui s'écrit $R_{\ell(\theta_S) \le R_{\ell}^S}(\theta_S) + 2 \beta + (4m \beta + K) \sqrt{\ln(\delta^{-1})/2m}$. Puisque $\beta$ est directement proportionnel à $\sigma^2$ (provenant de la lipschitzianité de la perte) et inversement proportionnel au paramètre de régularisation $\lambda$, le choix de $\lambda$ permet de régler la stabilité et d'obtenir une **borne plus serrée**. Si $\lambda$ augmente, $\beta$ diminue, et la borne de généralisation s'améliore, ce qui est cohérent avec l'objectif de la régularisation : limiter la complexité pour mieux généraliser.

En résumé, la **convexité** assure qu'une solution optimale peut être trouvée efficacement, tandis que la propriété de **lipschitzianité** de la fonction de perte, couplée à la convexité, assure que cette solution est stable et donc qu'elle aura une bonne capacité de **généralisation** sur des données inconnues.


