---
layout: cours
usemathjax: true
precedent: 1
suivant : 3
---

## Temps de calculs

le temps de calcul d'un algorithme P se mesure en fonction de la taille de l'entrée. Soit I une entrée de P.
- $$t_p(I)$$ désigne le temps de calcul de P sur I. Le temps de calcul est le nombre d'instructions élémentaires exécutées.
- La fonction $$T_p : \mathbb{N} \to \mathbb{N}$$ est définie par :  
$$T_p(n) = max\{t_p(I)\mid taille(I) = n\}$$  
Il s'agit d'un temps de calcul au pire des cas.

Un algorithme P est polynomial s'il existe un polynôme $$p(n)$$ tel que $$T_p(n) \leq p(n), \ \forall n$$.  
Un algorithme P est exponentiel s'il existe un polynôme $$p(n)$$ tel que $$T_p(n) \leq 2^{p(n)}$$

Problèmes faciles sur les graphes (algorithmes polynomiaux) : Accessibilité, Connexité, Graphes eulériens, 2-colorabilité.  
Problèmes difficiles : SAT, graphes hamiltoniens, 3-colorabilité.  
Problèmes encore plus difficiles : Certains jeux à deux joueurs, QSAT, Savoir si l'interaction de n automates finis est non-vide.  
Problèmes non résolubles : Terminaison de programmes, PCP, Eternity.  

## Logique propositionnelle

étant donné des variables booléennes $$x_1, x_2, ...$$ :
- Un littéral est soit une variable $$x_i$$, soit la négation d'une variable $$\neg x_i$$.
- Une clause est une disjonction de littéraux. (Ex : $$x_1 \lor \neg x_2 \lor \neg x_4 \lor x_5$$)
- Une 3-clause est une clause avec 3 littéraux. (Ex : $$x_1\lor\neg x_2\lor\neg x_4$$)
- Une formule CNF (Conjonctive Normal Form) est une conjonction de clause.
- Une formule 3-CNF est une conjonction de 3-clauses. (Ex : $$(x_1\lor x_2 \lor x_3)\land (x_4 \lor x_5 \lor x_6) $$)

## SAT

Le problème SAT est le suivant :  
Données : Une formule CNF sur des variables $$x_1, x_2, ..., x_n$$.  
Question : Existe-t-il une valuation des variables $$\sigma : \{x_i \mid 1 \leq i \leq n\} \to \{0, 1\}$$ qui rend la formule vraie ?


Idée : brute-force, on génère toutes les valuations et on regarde si il y en a une qui correspond. Le temps de calcul est $$2^n\ O(\mid\varphi\mid)$$

## 3-SAT

Le problème 3-SAT est le suivant :  
Données : Une formule 3-CNF sur des variables $$x_1, x_2, ..., x_n$$.  
Question : Existe-t-il une valuation des variables $$\sigma : \{x_i \mid 1 \leq i \leq n\} \to \{0, 1\}$$ qui rend la formule vraie ?

Le problème 3-SAT est donc moins général que SAT.

*On peut utiliser une algorithme pour SAT afin de résoudre 3-SAT, mais est-ce que l'inverse est possible ?*

## Réduction

Soient $$A$$ et $$B$$ deux problèmes. On note $$X \subseteq D$$ l'ensemble des instances positives de $$A$$, et $$Y \subseteq D'$$ l'ensemble des instances positives de $$B$$.  
Une réduction de $$A$$ vers $$B$$ est une fonction calculable $$f:D\to D'$$ telle que $$x\in X \Leftrightarrow f(x)\in Y$$.  
On note $$A\leq B$$ ($$A$$ se réduit à $$B$$).  
Une réduction $$f:D\to D'$$ est polynomiale si $$f$$ se calcule en temps polynomiale. On note $$A\leq_p B$$ s'il existe une réduction polynomiale de $$A$$ vers $$B$$.

### SAT vers 3-SAT

Pour résoudre SAT avec les algorithmes de 3-SAT, on réduit l'instance $$\varphi$$ SAT à une instance $$\tilde{\varphi}$$ 3-SAT telle que si l'une est satisfaisable alors l'autre l'est aussi.

Remarques :  
- La construction de $$\tilde{\varphi}$$ à partir de $$\phi$$ sera faite en temps polynomial par rapport à la taille de $$\varphi$$.
- Il est garantit que l'algorithme P pour 3-SAT peut être utilisé pour résoudre SAT.

> On demande que la "traduction de $$\varphi$$ vers $$\tilde{\varphi}$$ soit faisable en temps polynomial car on s'intéresse aux problèmes P vs NP.

### Problème clique

Une clique pour un graphe est un ensemble de sommets tous relié 2 à 2.

Question : existe-t-il une clique de G de taille k.

**Réduction clique vers SAT** :

Pour la réduction on utilise des variables booléennes de la forme $$v_i \mid v\in V, 1\leq i\leq k$$.  
Intuition : $$v_i$$ prend la valeur 1 si $$v$$ est le même sommet d'une clique.  
Notre formule $$\varphi_{G,k}$$ va garantir :
- qu'il y a exactement un sommet qui est le i-ème de la clique
- pour $$1\leq i,j\leq k$$, $$i\neq j$$ : i-ème sommet de la clique $$\neq$$ j-ème sommet de la clique.
- $$\{v:v_i=1\}$$ est une clique.
