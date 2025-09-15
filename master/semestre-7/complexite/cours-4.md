---
layout: cours
usemathjax: true
precedent: 3
suivant: 5
---

## Théorème de Cook-Levin

Ce théorème montre que SAT est NP-Complet.

Si on réduit SAT à un problème A, alors A est NP-difficile.  
Si un problème A se réduit à SAT, alors A appartient à NP.

Preuve :  
On considère un problème $$A \in NP$$ quelconque et on montre $$A \leq_p SAT$$.  
- $$A\in NP$$ signifie qu'on a un vérificateur polynomial V pour le problème A.
- Le problème SAT.

Pour tout algorithme polynomial V on peut construire un circuit booléen $$C_v$$ de taille polynomiale dont les entrées $$z_1, ..., z_n \in \{0, 1\}$$ correspondent à l'entrée $$z = z_1, ..., z_n$$ de V (codée en binaire), et tel que V accepte z si et seulement si $$C_v$$ s'évalue à 1.  
Etant donné un circuit booléen C avec entrées $$x_1, ..., x_k, y_1, ..., y_m$$ et une valuation val : $$\{x_1, ..., x_k\} \to \{0, 1\}$$ on construit une formule booléen $$\varphi_{val}(y_1, ..., y_m)$$ de taille polynomiale en taille de C, telle que :  
$$\varphi_{val}$$ est satisfaisable si et seulement si il existe une valuation $$\tilde{val} : \{y_1, ..., y_m\} \to \{0, 1\}$$ tel que C s'évalue à 1 sous la valuation $$<val, \tilde{val}>$$.

## Problèmes de décision / calcul de solutions / optimisation

calcul de solution : si une formule est satisfaisable, alors calculer une valuation satisfaisant.

Considérons le problème SAT et supposons qu'il ait un algorithme polynomial P pour le résoudre. Alors on peut construire l'algorithme suivant, qui calcule pour une formule données $$\varphi$$ et valuation satisfaisante $$\sigma$$ :
- La formule donnée $$\varphi(x_1, ..., x_n)$$
- Si $$P(\varphi)$$ retourne "non", alors retourner "non-satisfaisable"
- Pour $$i=1$$ jusqu'à $$n$$ faire :
	- Si $$P(\varphi(b_1, ..., b_{i-1}, 1, x_{i-1}, ..., x_n))$$ retourne "oui", alors $$b_i := 1$$ sinon $$b_i := 0$$
- Retourner $$(b_1, ..., b_n)$$
- 