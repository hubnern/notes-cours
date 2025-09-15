---
layout: cours
usemathjax: true
precedent: 6
suivant: 8
---

### Minimum Spanning Tree

*Arbre couvrant de poids minimum*

Entrée : $$(V, d)$$ *avec inégalité triangulaire*  
Sortie : une tournée

1. Arbre couvrant de poids minimum sur le graphe complet défini par $$V$$ et les arêtes valuées par $$d$$.
2. La tournée définie par l'ordre de visite des sommets de T lors d'un parcours en profondeur de T.

Montrons qu'il s'agit d'une 2-approximation :
- Complexité polynomiale
- Facteur d'approximation <= 2.

La longueur de la tournée optimale pour $$(V, d)$$ est plus grande que le poids du MST du graphe complet sur $$(V, d)$$. $$OPT(V, d) > d(T) \Leftrightarrow OPT(V, d) \geq d(T) + d(e)$$

La longueur obtenue par le parcours de T est au plus 2d(T) : $$ApproxMST(V, d) \leq 2d(T)$$

### Union-Find

Pour implémenter Kruskal. Problème de maintient des composantes connexes d'un graphe.  
On résout ce problème via une structure de données (Union-Find) qui manipule 2 objets :
- Éléments (points, sommets)
- Ensembles (composantes connexes)

Elle manipule aussi des opérations :
- Fusionner deux ensembles
- Trouver l'ensemble C contenant un élément donné u

On représente les ensembles par des arbres (enracinés)
