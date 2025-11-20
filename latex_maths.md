# LaTeX Cheat Sheet - Maths

---

## 1. Inline et Display

| Code | Résultat |
|------|----------|
| ``$a^2 + b^2 = c^2$`` | $a^2 + b^2 = c^2$ |
| ``\[ a^2 + b^2 = c^2 \]`` | \[ a^2 + b^2 = c^2 \] |

---

## 2. Exposants, indices, fractions, racines

| Code | Résultat |
|------|----------|
| ``$a_i$`` | $a_i$ |
| ``$a_{i,j}$`` | $a_{i,j}$ |
| ``$x^2$`` | $x^2$ |
| ``$x_1^2$`` | $x_1^2$ |
| ``$x^{n+1}$`` | $x^{n+1}$ |
| ``$\frac{a}{b}$`` | $\frac{a}{b}$ |
| ``$\sqrt{x}$`` | $\sqrt{x}$ |
| ``$\sqrt[3]{x}$`` | $\sqrt[3]{x}$ |

---

## 3. Parenthèses, valeurs absolues, normes, produit scalaire, etc.

| Code | Résultat |
|------|----------|
| ``$(x+1)$`` | $(x+1)$ |
| ``$\left( \frac{a}{b} \right)$`` | $\left( \frac{a}{b} \right)$ |
| ``$\left\| x \right\|$`` | $\left\| x \right\|$ |
| ``$\vert x \vert$`` | $\vert x \vert$ |
| ``$\Vert x \Vert$`` | $\Vert x \Vert$ |
| ``$\langle x,y \rangle$`` | $\langle x,y \rangle$ |
| ``$\lfloor x \rfloor$`` | $\lfloor x \rfloor$ |
| ``$\lceil x \rceil$`` | $\lceil x \rceil$ |

---

## 4. Sommes, produits, intégrales, limites

| Code | Résultat |
|------|----------|
| ``$\sum_{i=1}^n x_i$`` | $\sum_{i=1}^n x_i$ |
| ``$\prod_{k=1}^n a_k$`` | $\prod_{k=1}^n a_k$ |
| ``$\bigcup A_n$`` | $\bigcup A_n$ |
| ``$\bigcap A_n$`` | $\bigcap A_n$ |
| ``$\int_a^b f(x)\ dx$`` | $\int_a^b f(x)\ dx$ |
| ``$\iint$`` | $\iint$ |
| ``$\iiint$`` | $\iiint$ |
| ``$\lim_{x \to 0} \frac{\sin x}{x}$`` | $\lim_{x \to 0} \frac{\sin x}{x}$ |

---

## 5. Ensembles, appartenance, logique

| Code | Résultat |
|------|----------|
| ``$\mathbb{R}$`` | $\mathbb{R}$ |
| ``$x \in A$`` | $x \in A$ |
| ``$x \notin A$`` | $x \notin A$ |
| ``$A \subset B$`` | $A \subset B$ |
| ``$A \subseteq B$`` | $A \subseteq B$ |
| ``$A \cup B$`` | $A \cup B$ |
| ``$A \cap B$`` | $A \cap B$ |
| ``$\forall x, \exists y$`` | $\forall x, \exists y$ |
| ``$\Rightarrow$`` | $\Rightarrow$ |

---

## 6. Comparaisons, équivalences

| Code | Résultat |
|------|----------|
| ``$a \lt b$`` | $a \lt b$ |
| ``$a \le b$`` | $a \le b$ |
| ``$a \gt b$`` | $a \gt b$ |
| ``$a \ge b$`` | $a \ge b$ |
| ``$a \ll b$`` | $a \ll b$ |
| ``$a \gg b$`` | $a \gg b$ |
| ``$a \approx b$`` | $a \approx b$ |
| ``$a \sim b$`` | $a \sim b$ |
| ``$a \equiv b$`` | $a \equiv b$ |
| ``$a \neq b$`` | $a \neq b$ |

---

## 7. Vecteurs, matrices, transposées, gradient

| Code | Résultat |
|------|----------|
| ``$\vec{x}$`` | $\vec{x}$ |
| ``$\mathbf{x}$`` | $\mathbf{x}$ |
| ``$x^\top$`` | $x^\top$ |
| ``$\nabla$`` | $\nabla$ |
| ``$\begin{pmatrix} a & b \\ c & d \end{pmatrix}$`` |  |
| ``$\begin{bmatrix} 1 & 2 \\ 3 & 4 \end{bmatrix}$`` |  |

---

## 8. Fonctions, cas, dérivées, indicatrice

| Code | Résultat |
|------|----------|
| ``$f : \mathbb{R} \to \mathbb{R}, x \mapsto y$`` | $f : \mathbb{R} \to \mathbb{R}, x \mapsto y$ |
| ``$f'(x)$`` | $f'(x)$ |
| ``$\frac{df}{dx}$`` | $\frac{df}{dx}$ |
| ``$\frac{\partial f}{\partial x}$`` | $\frac{\partial f}{\partial x}$ |
| ``$\mathbb{1}_A = \begin{cases} 1 & \text{si } x \in A \\ 0 & \text{sinon} \end{cases}$`` |  |

---

## 9. Probabilités

| Code | Résultat |
|------|----------|
| ``$\mathbb{P}(A)$`` | $\mathbb{P}(A)$ |
| ``$\mathbb{E}[X]$`` | $\mathbb{E}[X]$ |
| ``$\mathrm{Var}(X)$`` | $\mathrm{Var}(X)$ |
| ``$X \sim \mathcal{N}(\mu, \sigma^2)$`` | $X \sim \mathcal{N}(\mu, \sigma^2)$ |
| ``$\mathbf{1}_{A}$`` | $\mathbf{1}_{A}$ |

---

## 10. Alignement d'équations

| Code | Résultat |
|------|----------|
| ```\begin{align} f(x) &= x^2 + 1 \\ &= x^2 + x + (1-x) \end{align}``` | |

---

## 11. Lettres grecques

| Code | Résultat |
|------|----------|
| ``$\alpha, \beta, \gamma, \delta$`` | $\alpha, \beta, \gamma, \delta$ |
| ``$\lambda, \mu, \theta$`` | $\lambda, \mu, \theta$ |
| ``$\sigma, \Sigma$`` | $\sigma, \Sigma$ |
| ``$\varepsilon$`` | $\varepsilon$ |
| ``$\theta^\star$`` | $\theta^\star$ |

---

## 12. Calligraphie

| Code | Résultat |
|------|----------|
| ``$\mathbb{1}$`` ``$\mathbb{C}$`` ``$\mathbb{N}$`` ``$\mathbb{Q}$`` ``$\mathbb{Z}$`` | $\mathbb{1}$ $\mathbb{C}$ $\mathbb{N}$ $\mathbb{Q}$ $\mathbb{Z}$ |
| ``$\mathcal{A}$`` ``$\mathcal{L}$`` ``$\mathcal{M}$`` | $\mathcal{A}$ $\mathcal{L}$ $\mathcal{M}$ |
| ``$\mathfrak{a}$`` ``$\mathfrak{i}$`` ``$\mathfrak{p}$`` ``$\mathfrak{q}$`` | $\mathfrak{a}$ $\mathfrak{i}$ $\mathfrak{p}$ $\mathfrak{q}$ |
| ``$\dot{a}$`` | $\dot{a}$ |
| ``$\ddot{a}$`` | $\ddot{a}$ |
| ``$\hat{a}$`` | $\hat{a}$ |
| ``$\tilde{a}$`` | $\tilde{a}$ |
| ``$\bar{a}$`` | $\bar{a}$ |

---

## 13. Autres symboles

| Code | Résultat |
|------|----------|
| ``$\infty$`` | $\infty$ |
| ``$x_1 \cdot x_2 \cdot x_3 \cdots x_n$`` | $x_1 \cdot x_2 \cdot x_3 \cdots x_n$ |
| ``1, 2, \ldots, n$`` | $1, 2, \ldots, n$ |
| ``$f \circ g$`` | $f \circ g$ |
| ``$\pm \mp$`` | $\pm \mp$ |
| ``$\times \div$`` | $\times \div$ |


## 14. Packages utiles (préambule)

À mettre avant `\begin{document}` :

```tex
\usepackage{amsmath, amssymb, amsfonts}
\usepackage{mathtools}
\usepackage{bm} % pour \bm{x}
