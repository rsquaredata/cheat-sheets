<!--
Title: "Concepts fondamentaux de ML - Risques et généralisation - Bornes de généralisation - Stabilité uniforme (Constante β)"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Stabilité uniforme (Constante $\beta$)

L'approche basée sur la **stabilité uniforme** est un cadre théorique essentiel pour établir des bornes de généralisation dans le contexte du Machine Learning, particulièrement pour les problèmes d'optimisation convexes régularisés.

Cette méthode, contrairement à celles qui utilisent la dimension VC ou la complexité de Rademacher, ne repose pas directement sur une mesure de la complexité de l'espace d'hypothèse $H$.

## 1. Définition du concept de stabilité uniforme

L'idée fondamentale de la stabilité est qu'un algorithme est considéré comme stable si sa sortie (l'hypothèse apprise) change très peu suite à une petite modification de l'ensemble d'entraînement.

Plus précisément, la Stabilité Uniforme est définie par la constante de stabilité uniforme $\beta$ :

Un algorithme d’apprentissage $A$ est uniformément stable avec une constante de stabilité uniforme $\beta \gt 0$ par rapport à une fonction de perte $\ell$ et un paramètre $\theta$ si :

$$
\forall \text{ } S \text{, } \forall \text{ } i \text{, } 1 \le i \le m \text{, } \sup_x \lvert \ell(\theta_S,x) - \ell(\theta_{S^i},x) \rvert \le \beta,
$$

où :
- $S$ est l'ensemble d'apprentissage de taille $m$.
- $\theta_S$ sont les paramètres du modèle appris avec $S$.
- $\theta_{S^i}$ sont les paramètres du modèle appris avec l'échantillon $S^i$, obtenu en remplaçant le $i$-ème exemple de $S$ par un autre exemple $x'_i$ tiré de $D$.

La constante $\beta$ est cruciale car elle encapsule toutes les propriétés de la fonction de perte $\ell$ ainsi que celles du terme de régularisation du problème d'optimisation.

## 2. Le rôle de $\beta$ dans la borne de généralisation

La stabilité uniforme est l'hypothèse clé qui permet d'appliquer l'**inégalité de McDiarmid** pour obtenir une borne de généralisation.

Le **théorème des bornes via stabilité uniforme** établit la borne suivante pour le risque réel $R_{\ell}(\theta_S$) en fonction du risque empirique $R_{\ell}^S(\theta_S)$, avec une probabilité d'au moins $1-\delta$ :

$$
R_{\ell}(\theta_S) \le R_{\ell}^S(\theta_S) + 2 \beta + (4m \beta + K) \sqrt{\frac{\ln(1/\delta)}{2m}}.
$$

## 3. Application aux SVM : Détermination de la constante $\beta$

Utilisons les Séparateurs à Vaste Marge (SVM) comme exemple d'application pour démontrer la stabilité uniforme.

Pour un problème d'optimisation régularisé, comme le Soft Margin SVM avec la *Hinge Loss*, où la fonction objectif est de la forme $F_S(w) = R^S(w)+ \lambda N(w)$, on cherche à déterminer la valeur de $\beta$.

En supposant que la fonction de perte $\ell$ est $\sigma$-admissible (convexe et lipschitzienne par rapport à son premier argument $w$), la constante de stabilité uniforme $\beta$ pour cet algorithme est donnée par l'expression suivante :

$$
\beta = \frac{2 \sigma^2}{\lambda m}.
$$

Ici, $\sigma$ est la constante de Lipschitz de la *loss* $\ell$, $m$ est la taille de l'échantillon, et $\lambda$ est le paramètre de régularisation issu de la formulation $R^S(w) + \lambda N(w)$.

## 4. Interprétation de la constante $\beta$ et son impact sur la généralisation

L'expression $\beta = \frac{2 \sigma^2}{\lambda m}$ met en évidence l'impact des hyper-paramètres et de la taille de l'échantillon sur la généralisation.

- **Dépendance en $m$ (taille de l'échantillon)** : La constante $\beta$ est inversement proportionnelle à $m$. Cela signifie que plus l'ensemble de données d'entraînement est grand, plus la stabilité uniforme de l'algorithme est grande ($\beta$ est petit).
- **Dépendance en $\lambda$ (régularisation)** : La constante $\beta$ est inversement proportionnelle à l'hyper-paramètre de régularisation $\lambda$.
  -  Si $\lambda$ est grand (régularisation forte), $\beta$ est petit, ce qui implique que l'algorithme est très stable, et la borne de généralisation est plus serrée. Ceci est cohérent avec la théorie : une régularisation forte limite la complexité du modèle, réduisant ainsi le risque de surapprentissage.
  -  Si $\lambda$ est petit (régularisation faible), $\beta$ est grand, indiquant une instabilité et une borne de généralisation plus lâche, augmentant le risque de surapprentissage.
  
Le rôle de $\beta$ est donc de quantifier la robustesse du processus d'apprentissage par rapport aux variations des données d'entrée, garantissant que le risque empirique est une bonne approximation du risque réel. L'évolution de la borne de généralisation est directement contrôlée par $\beta$, soulignant l'importance de choisir un $\lambda$ approprié pour maximiser la stabilité et donc la capacité de généralisation.

L'approche de la stabilité uniforme (via $\beta$) est donc considérée comme plus informative en pratique, car elle intègre explicitement le terme de régularisation, essentiel dans les modèles modernes.
