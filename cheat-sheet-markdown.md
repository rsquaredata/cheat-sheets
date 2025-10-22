# Cheat Sheet Markdown
Ce document rassemble les éléments Markdown et HTML que j'utilise le plus dans mes fiches, notebooks et projets VS Code ou Quarto.

---

## 1. Titres

```markdown
# Titre 1
## Titre 2
### Titre 3
#### Titre 4
##### Titre 5
###### Titre 6
```

---

## 2. Texte

### Style

`**Gras**` → **Gras**
`*Italique*` → *Italique*
`<u>Souligné</u>` → <u>Souligné</u>
`~~Barré~~` → ~~Barré~~
`**_Gras + italique_**` → **_Gras + italique_**

### Taille

`<small>Petit</small>` → <small>Petit</small>
`<span style="font-size:80%;">Très petit</span>` → <span style="font-size:80%;">Très petit</span>
`<span style="font-size:130%;">Grand</span>` → <span style="font-size:130%;">Grand</span>

### Couleur

`<span style="color:red;">Rouge</span>` → <span style="color:red;">Rouge</span>
`<span style="color:blue;">Bleu</span>` → <span style="color:blue;">Bleu</span>
`<span style="color:green;">Vert</span>` → <span style="color:green;">Vert</span>
`<span style="color:#E91E63;">Rose (HEX)</span>` → <span style="color:#E91E63;">Rose (HEX)</span>

### Fond et surlignage

`<mark>Surligné (jaune)</mark>` → <mark>Surligné (jaune)</mark>
`<span style="background-color:lightblue;">Fond bleu clair</span>` → <span style="background-color:lightblue;">Fond bleu clair</span>

### Alignement

`<div align="center">Texte centré</div>`

<div align="center">Texte centré</div>

`<div align="right">Texte aligné à droite</div>`

<div align="right">Texte aligné à droite</div>

### Combinaisons

<span style="color:#007ACC; font-size:80%; background-color:#F0F0F0;">
Texte petit, bleu et sur fond gris clair
</span>

```html
<span style="color:#007ACC; font-size:80%; background-color:#F0F0F0;">
Texte petit, bleu et sur fond gris clair
</span>
```
---

## 3. Listes

### À puces
```markdown
- Premier
- Deuxième
  - Sous-élément
```

### Numérotées
```markdown
1. Premier
2. Deuxième
   1. Sous-élément
```

---

## 4. Liens

[Texte du lien](https://exemple.com)

```markdown
[Texte du lien](https://exemple.com)
```

---

## 5. Images

![Texte alternatif](chemin/vers/image.png)

```markdown
![Texte alternatif](chemin/vers/image.png)
```

---

## 6. Code et Syntax Highlighting

### Inline
\`code\` → `code`

### Bloc de code
````
```python
print("Hello, world!")
```
````

---

## 7. Tableaux

| Colonne 1  | Colonne 2  |
|------------|------------|
| Valeur 1   | Valeur 2   |

```markdown
| Colonne 1  | Colonne 2  |
|------------|------------|
| Valeur 1   | Valeur 2   |
```

### Mise en forme

| Colonne 1   | Colonne 2        |
|:-----------:|-----------------:|
|centrée      |alignée à droite  |
|0            |**gras**          |
|`code`       |<small>a</small>  |

```markdown
| Colonne 1   | Colonne 2        |
|:-----------:|-----------------:|
|centrée      |alignée à droite  |
|0            |**gras**          |
|`code`       |<small>a</small>  |
```


---

## 8. Citation

`> Ceci est une citation`

> Ceci est une citation

---

## 9. Inline HTML

```html
<span style="color:purple;">Texte violet</span>
<div align="center">Texte centré</div>
```

---

## 10. Ligne horizontale

Trois tirets ou astérisques :

```
---
```
---

```
***
```
***
---

## 11. Saut de ligne

Deux espaces à la fin d’une ligne →  
Provoquent un retour à la ligne.  

Ou utiliser `<br>` pour forcer le saut de ligne.

---

## 12. Vidéos Youtube

Ajouter une image avec un lien vers la vidéo :

<a href="http://www.youtube.com/watch?feature=player_embedded&v=rlarCLhzfoU
" target="_blank"><img src="http://img.youtube.com/vi/rlarCLhzfoU/0.jpg" 
alt="Ingénieur informaticien" width="240" height="180" border="10" /></a>

```html
<a href="http://www.youtube.com/watch?feature=player_embedded&v=YOUTUBE_VIDEO_ID
" target="_blank"><img src="http://img.youtube.com/vi/YOUTUBE_VIDEO_ID/0.jpg" 
alt="Texte alternatif image" width="240" height="180" border="10" /></a>
```

---

## 13. Formules mathématiques

### Inline
`$e^{i \pi} + 1 = 0$` → $e^{i \pi} + 1 = 0$

### Bloc
```latex
$$
\int_a^b f(x) \ dx = F(b) - F(a)
$$
```

$$
\int_a^b f(x) \ dx = F(b) - F(a)
$$

Le signe *backslash* "`\`" suivi d'une espace sert ici à ajouter l'espace en question entre `f(x)`et `dx`.  
Plus d'info sur les formules mathématiques dans le cheat sheet LaTex.

---

## 14. Ajouter un spoiler

```
Question apparente
<details>
    <summary>Cliquer ici</summary>
Réponse cachée.
</details>
```
Question apparente
<details>
    <summary>Cliquer ici</summary>
Réponse cachée.
</details>

---

## 15. Palette de couleurs (HTML)

### Couleurs de base (nommées)

- `<span style="color:red;">rouge</span>` → <span style="color:red;">rouge</span>
- `<span style="color:blue;">bleu</span>` → <span style="color:blue;">bleu</span>
- `<span style="color:green;">vert</span>` → <span style="color:green;">vert</span>
- `<span style="color:orange;">orange</span>` → <span style="color:orange;">orange</span>
- `<span style="color:purple;">violet</span>` → <span style="color:purple;">violet</span>
- `<span style="color:gray;">gris</span>` → <span style="color:gray;">gris</span>

### Couleurs HEX

- `<span style="color:#E91E63;">rose #E91E63</span>` → <span style="color:#E91E63;"> rose #E91E63</span>
- `<span style="color:#2196F3;">bleu primaire #2196F3</span>` → <span style="color:#2196F3;"> bleu primaire #2196F3</span>
- `<span style="color:#00BCD4;">cyan #00BCD4</span>` → <span style="color:#00BCD4;">cyan #00BCD4</span>
- `<span style="color:#4CAF50;">vert émeraude #4CAF50</span>` → <span style="color:#4CAF50;">vert émeraude #4CAF50</span>
- `<span style="color:#FFC107;">ambre #FFC107</span>` → <span style="color:#FFC107;">ambre #FFC107</span>
- `<span style="color:#FF9800;">orange soutenu #FF9800</span>` → <span style="color:#FF9800;">orange soutenu #FF9800</span>
- `<span style="color:#455A64;">gris foncé #455A64</span>` → <span style="color:#455A64;">gris foncé #455A64</span>

### Fonds / surlignage

- `<span style="background-color:yellow;">Fond jaune</span>` → <span style="background-color:yellow;">Fond jaune</span>
- `<span style="background-color:#E3F2FD;">Fond bleu clair</span>` → <span style="background-color:#E3F2FD;">Fond bleu clair</span>
- `<span style="background-color:#E8F5E9;">Fond vert pâle</span>` → <span style="background-color:#E8F5E9;">Fond vert pâle</span>
- `<span style="background-color:#F5F5F5;">Fond gris clair</span>` → <span style="background-color:#F5F5F5;">Fond gris clair</span>

### Combinaisons utiles

```html
<span style="color:#007ACC; font-size:85%; background-color:#F0F0F0;">
Note discrète bleue sur fond gris clair
</span>
```

<span style="color:#007ACC; font-size:85%; background-color:#F0F0F0;">
Note discrète bleue sur fond gris clair
</span>

---

## Notes

- Sur Mac, `⌘ ⇧ V` pour prévisualiser
