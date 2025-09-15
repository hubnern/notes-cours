---
layout: cours
precedent: 3
suivant: 5
---

## Analyse Cocke-Younger-Kasami

L'algorithme s'applique sur la grammaire normale de Chomky. (il n'y a pas plus de deux éléments à droite).

Construire un arbre, la racine est l'axiome de la grammaire, les feuilles sont les tokens.  
On commence des feuilles. On créé un parent pour deux feuilles si ces deux feuilles sont la réécriture du token parent.
