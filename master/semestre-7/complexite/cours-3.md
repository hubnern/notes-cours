---
layout: cours
usemathjax: true
precedent: 2
suivant: 4
---


## Réduction polynomiale

Une réduction polynomiale d'un problème $$A_1$$ vers une problème $$A_2$$ est une fonction de réduction de $$A_1$$ vers $$A_2$$, qui est calculable en temps polynomiale. On note $$A_1 \leq_p A_2$$.

Les réductions polynomiales sont transitives : Si $$A_1 \leq_p A_2$$ et $$A_2 \leq_p A_3$$ alors  $$A_1 \leq_p A_3$$.

Si $$A_1 \leq_p A_2$$ et $$A_2$$ possède un algorithme polynomial, alors $$A_1$$ en a un aussi.

La classe des problèmes résolubles en temps polynomial est fermée par réductions polynomiales.

## Classe NP

Une classe **NP** est une classe de problèmes. Informellement un problème appartient à NP si :
- On peut vérifier une solution données en temps polynomiale
- S'il existe une solution, alors il en existe toujours une de taille polynomiale

Les problèmes NP dont difficiles car le nombre de solution est potentiellement exponentiel.  
La recherche naïve peut donc demander un temps exponentiel.

### Définition formelle

Soit A un problème. Un vérificateur de A est un algorithme V qui prend en entrée des paires (x,y), où x est une instance de A, et vérifie que y est bien une preuve (ou certificat) montrant que x est une instance positive.

Le problème A possède un vérificateur polynomial V si :
- Il existe un polynôme p() tel que pour toute instance x de A, x est une instance positive si et seulement si il existe une preuve y de taille au plus p(taille(x)) et tel que V accepte (x,y).
- L'algorithme V est polynomial dans taille(x).

> Exemple : Un vérificateur pour SAT prend en entrée une formule CNF et une valuation de ses variables, et décide si la valuation rend la formule vraie.

### P vs NP

La classe P contient tous les problèmes qui ont des algorithmes polynomiaux.  
Évidemment $$P\subseteq NP$$.  
La question $$P = NP$$ est ouverte et représente une des questions majeures de l'informatique.

### Problèmes NP-complets

Un problème est NP-complet si :
- A appartient à NP
- tout problème A' appartenant à NP se réduit à A par une réduction polynomiale : $$A' \leq_p A$$

On va montrer qu'il existe des problèmes NP-complets.  
Si on trouve un algorithme polynomial pour un problème NP-complet, alors $$NP \subseteq P$$, donc $$P = NP$$.  
Supposons que $$A$$ est NP-complet et que $$A \in P$$. Alors, pour tout problème $$A'\in NP : A' \leq_p A$$ (car $$A$$ est NP-difficile), donc $$A' \in P$$ (car $$P$$ est clos par les réduction polynomiales).

Problème X est NP-difficie : Tout problème de la classe NP se réduit au problème X.
