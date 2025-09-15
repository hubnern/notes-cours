---
layout: cours
usemathjax: true
precedent: 1
suivant: 3
---

# Extension de la logique avec des opérateurs de points fixes

## Définitions

Avec $$E$$ un ensemble discret avec des inclusions relatives, $$F : E \to E$$, et $$E_1 \subseteq E \land E_2 \subseteq E$$.

$$f$$ est monotone ssi $$E_1 \subseteq E_2 \to f(E_1) \subseteq f(E_2)$$.  
$$f$$ est anti monotone ssi $$E_1 \subseteq E_2 \to f(E_1) \subseteq f(E_2)$$.

...

## Propriétés

...

$$f_1(f_2(X))$$ est monotone

...

Soit $$f : E \to E$$ une fonction monotone croissante :
- $$\emptyset \subseteq f(\emptyset) \subseteq f^2(\emptyset) \subseteq ... \subseteq f^n(\emptyset) \subseteq ... \subseteq E$$
- $$E$$ un ensemble fini discret $$\Rightarrow \exists i \setminus \forall n \geq i, f^n(\emptyset) = f^i(\emptyset)$$
- $$f^i(\emptyset)$$ est la plus petite sollution de l'équation $$X = f(X)$$

**Résultat** : Si $$E$$ est fini et discret et $$f$$ est monotone décroissante, alors l'équation $$X = f(X)$$ a un point fixe plus grand calculable par itération.  
**Remarque** : $$f(\emptyset) = \emptyset$$ donc $$\emptyset$$ est aussi une solution de $$X = f(X)$$.

## Peut-on savoir si une fonction est monotone ?

Avec un graphe $$G(S, T)$$, $$f(X)$$ un formule logique sur $$S$$ ou $$T$$, ...

### Propositions constantes et élémentaires

...

### Opérateurs logiques

$$f(X)$$ une formule avec `src, tgt, rsrc, rtgt` :
- $$T_1 \subseteq T_2 \Rightarrow ...$$ : monotone
- .
- .
- .

$$f$$ est monotone, donc  une solution de $$X = f(X)$$ peut être calculée par intération.

### Opérateurs d'ensembles

$$f(X)$$ une formule avec $$\cup$$, $$\cap$$, $$\setminus$$ :
- $$X_1 \subseteq X_2, Y_1 \subseteq Y_2 \Rightarrow X_1 \cup Y_1 \subseteq X_2 \cup Y_2$$ : monotone
- $$X_1 \subseteq X_2, Y_1 \subseteq Y_2 \Rightarrow X_1 \cap Y_1 \subseteq X_2 \cap Y_2$$ : monotone
- $$X_1 \subseteq X_2, Y_1 \subseteq Y_2 \Rightarrow X_1 \setminus Y_1 ? X_2 \setminus Y_2$$ : on ne peut rien dire

