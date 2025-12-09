<!--
Title: "Fouille de données massives : Théorèmes"
Author: rsquaredata
Last updated: 2025-12-09
-->

# Fouilles de données massives (SVM & Noyaux) - Théorèmes

---

##  Théorème de la marge maximale

**Énoncé :**  
La solution optimale d’un SVM linéaire correspond à l’hyperplan qui maximise la marge séparant les deux classes :

$$
\gamma = \frac{2}{\|w\|}
$$

Sous contrainte :
$$
y_i(\langle w, x_i \rangle + b) \ge 1
$$

**Intuition :**  
Maximiser la marge = choisir le séparateur le plus robuste aux erreurs de classification.  
Plus la marge est grande, plus la généralisation est forte (moins de variance).

**Conséquence :**  
Le problème d’optimisation du SVM devient :

$$
\min_{w,b} \frac{1}{2}\|w\|^2 \quad \text{sous } y_i(\langle w, x_i \rangle + b) \ge 1
$$

---

## Dualité forte (Primal vs Dual)

**Énoncé :**  
Sous des hypothèses de convexité, le **problème dual** du SVM admet **la même solution** que le **problème primal**.

$$
\min_{w,b,\xi} L_{\text{primal}}(w,b,\xi)
\quad \Longleftrightarrow \quad
\max_{\alpha} L_{\text{dual}}(\alpha)
$$

**Conséquences :**
- Le problème dual est strictement concave → **solution unique.**  
- On peut retrouver $w$ et $b$ à partir des variables duales $(\alpha_i)$ (via les **conditions KKT**).  
- En pratique, on préfère souvent résoudre le dual (moins coûteux quand $m \lt$).

**Rappel des liens primal/dual :** $w = \sum_i \alpha_i y_i x_i, \quad \sum_i \alpha_i y_i = 0$

---

## Conditions de Karush-Kuhn-Tucker (KKT)

**Énoncé :**  
Les conditions KKT définissent le lien entre les variables primales $(w, b, x_i)$ et duales $(\alpha, \beta)$ :

$$
\begin{cases}
\frac{\partial L}{\partial w} = 0 \Rightarrow w = \sum_i \alpha_i y_i x_i \\
\frac{\partial L}{\partial b} = 0 \Rightarrow \sum_i \alpha_i y_i = 0 \\
\frac{\partial L}{\partial x_i} = 0 \Rightarrow \alpha_i + \beta_i = \frac{C}{m}
\end{cases}
$$

**Interprétation :**
- Ces équations assurent que la solution trouvée est **stationnaire** (point-selle du Lagrangien).  
- Elles permettent de reconstruire le modèle primal à partir du dual.

---

## Théorème de Mercer (1909)

**Énoncé :**  
Soit $X \subset \mathbb{R}^d$ un compact et $K : X \times X \to \mathbb{R}$ une forme bilinéaire symétrique **semi-définie positive (PSD)**.  
Alors il existe :
- une base orthogonale $(\Phi_j)_{j \in \mathbb{N}}$
- une suite $(\lambda_j)_{j \in \mathbb{N}}$, avec $\lambda_j \ge 0*

telles que :

$$
K(x, x') = \sum_{j=1}^\infty \lambda_j \Phi_j(x)\Phi_j(x') = \langle \Phi(x), \Phi(x') \rangle
$$

**Conséquence :**  
→ Toute fonction noyau valide correspond implicitement à un produit scalaire dans un **espace de Hilbert** (feature space).

**En pratique :**

$$
k(x, x') = \langle \phi(x), \phi(x') \rangle
$$

sans jamais calculer explicitement $\phi$ — c’est **l’astuce du noyau.**

---

## Bochner (1930) *(lié à Mercer, pour RFF)*

**Énoncé simplifié :**  
Une fonction $k(x, x')** continue et à valeurs réelles est un **noyau positif** (PSD) *si et seulement si* c’est la **transformée de Fourier** d’une mesure positive.

$$
k(x, x') = \int_{\mathbb{R}^d} e^{i\omega^T(x - x')} p(\omega)\, d\omega
$$

**Application pratique :**  
→ Fondement des **Random Fourier Features (Rahimi & Recht, 2007)**, qui approximent le noyau par projections aléatoires :

$$
k(x, x') \approx \frac{1}{R}\sum_{r=1}^R \cos(\omega_r^T x + b_r)\cos(\omega_r^\top x' + b_r)
$$

---

## Gram matrix & unicité

**Énoncé :**
Dans le dual du SVM :

$$
G_{ij} = y_i y_j \langle x_i, x_j \rangle
$$

La matrice de Gram $G$ est **semi-définie positive**, donc le problème dual est **strictement concave** → **solution unique.**

---

## 💡 intuitions à retenir

| Concept | Idée-clé |
|----------|-----------|
| **Produit scalaire** | Mesure de similarité entre deux vecteurs. Dans un espace transformé (noyau), il devient une similarité implicite. |
| **Matrice PSD** | Matrice $A$ telle que $x^\top A x \ge 0$ pour tout $x$. Garantit la convexité et l’existence d’un minimum global. |
| **Dualité forte** | Primal et dual ont la même valeur optimale - pratique pour résoudre les grands problèmes. |
| **KKT** | Conditions reliant contraintes et optimum : indispensables pour reconstruire $w, b$. |
| **Mercer** | Garantit que le noyau correspond à un vrai produit scalaire dans un espace de plus grande dimension. |
| **Bochner** | Lie les noyaux positifs à la transformée de Fourier - base des approximations aléatoires (RFF). |

---

## Références cours

- Metzler, G. *Fouille de Données Massives — SVM et Noyaux* (slides 2022-2024)  
- Rahimi, A. & Recht, B. (2007). *Random Features for Large-Scale Kernel Machines.*  
- Mercer, J. (1909). *Functions of Positive and Negative Type.*  
- Vapnik, V. (1995). *The Nature of Statistical Learning Theory.*
