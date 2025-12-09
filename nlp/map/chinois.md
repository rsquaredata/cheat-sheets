<!--
Title: "NLP - Fondations textuelles - Mots et tokenisation  - Défis et mots - Définition ambiguë"
Author: rsquaredata
Source: https://web.stanford.edu/~jurafsky/slp3/ed3book_aug25.pdf
Last updated: 2025-12-09
-->

# Ambiguïté du mot chinois et unités sub-lexicales en TAL

La **définition ambiguë des mots en chinois** est un défi qui s'inscrit dans le cadre plus large des **problèmes rencontrés lors de l'utilisation du mot comme unité de traitement** dans le domaine du TAL (Traitement Automatique du Langage).

## Définition ambiguë en chinois

L'ambiguïté de la définition des mots en chinois provient du fait que le système d'écriture de cette langue, tout comme le japonais et le thaï, n'utilise **pas d'espaces pour marquer les limites potentielles des mots** (c'est-à-dire qu'il n'y a pas de mots orthographiques).

En chinois, les mots sont composés de **caractères (appelés _hanzi_)**. Chaque caractère représente généralement une seule unité de sens (un morphème) et est prononçable comme une seule syllabe. Bien que la longueur moyenne d'un mot chinois soit d'environ 2,4 caractères, le fait d'identifier ce qui constitue un mot est complexe.

Les sources illustrent cette complexité en présentant différentes interprétations pour la même séquence de caractères, "姚明进入总决赛" (Yao Ming reaches the finals) :

1. **Définition "Chinese Treebank" (3 mots)** : Cette norme traite les noms chinois (nom de famille suivi du prénom) comme un seul mot :
    ◦ 姚明 (YaoMing)
    ◦ 进入 (reaches)
    ◦ 总决赛 (finals)
2. **Norme "Peking University" (5 mots)** : Cette norme sépare les noms en unités distinctes et considère certains adjectifs comme des mots à part entière :
    ◦ 姚 (Yao)
    ◦ 明 (Ming)
    ◦ 进入 (reaches)
    ◦ 总 (overall)
    ◦ 决赛 (finals)
3. **Approche basée sur les caractères** : Il est également possible de choisir d'**ignorer complètement les mots et d'utiliser les caractères** comme éléments de base (7 caractères dans cet exemple). Cette approche fonctionne relativement bien pour les applications en chinois, car les caractères se situent déjà à un "niveau sémantique raisonnable pour la plupart des applications".

## Contexte des défis des mots

Ce problème de définition constitue l'un des deux principaux défis qui empêchent les modèles de langage et les modèles de TAL de s'appuyer sur le concept de mot comme unité de traitement fondamentale, notamment dans les systèmes multilingues.

Le premier défi est cette **absence de mots orthographiques clairs** dans de nombreuses langues. Le second défi majeur est que **le nombre de mots (types) augmente sans cesse** (*Heaps' Law* ou *Herdan's Law*), ce qui signifie qu'un modèle informatique serait constamment confronté à des mots inconnus, quelle que soit la taille de son vocabulaire.
En raison de ces deux problèmes — l'ambiguïté de la définition du mot et la croissance illimitée du vocabulaire —, les modèles de langage et les autres modèles de TAL utilisent généralement des unités de traitement plus petites appelées sous-mots (*subwords* ou *tokens*), induites par des algorithmes tels que BPE (Byte-Pair Encoding). Ces unités peuvent être recombinées pour modéliser des mots que le système n'a jamais rencontrés auparavant.




