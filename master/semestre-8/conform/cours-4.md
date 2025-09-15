---
layout: cours
usemathjax: true
precedent: 3
---

*On manipule le modèle, pas directement le programme.*

$$C_1$$ :
- `skip`
- `x=t | ret t |*t = t`
- $$P_1; P_2$$
- if test then $$P_1$$ else $$P_2$$

$$C_2$$ : `while(test) P;`

$$WP(P, \psi) \in FO$$ *weakest precondition*

Triplet de Hoare : $$\{\varphi\}P\{\psi\}$$
*phi,psi dans FO et P dans $$C_1$$*

$$\{\varphi\}P\{\psi\}$$ est valide si $$\forall M$$ si $$M \models \varphi$$ alors $$P(M) \models \psi$$ (s'il existe).  
$$\{\varphi\}P\{\psi\} \equiv P([\![\varphi]\!] \subseteq [\![\psi]\!]$$  
c'est une correction partielle

Règles de WP :  
$$WP(x=t, \psi) = \psi[x\leftarrow y]$$ (on remplace x par t dans la formule)  
$$WP(skip, \psi) = \psi$$ (on ne fait rien)  
$$WP(P_1, P_2, \psi) = WP(P_1, WP(P_2, \psi))$$  
$$WP(if(test) P_1 else P_2, \psi) = test \Rightarrow WP(P_1, \psi) \land \neg test \Rightarrow WP(P_2, \psi)$$

On peut déduire d'autres comme :  
$$\forall \varphi_1, \varphi_2, P | WP(P, \varphi_1 \land \varphi_2) = WP(P, \varphi_1) \land WP(P, \varphi_2)$$

Pour savoir si $$\{\varphi\}P\{\psi\}$$ est valide :
- on calcule $$WP(P,\psi)$$ *syntaxique*
- on décide si $$\varphi \Rightarrow WP(P,\psi)$$ est vrai *sémantique*

Exemple :
```c
int g(int a) {
	if (a > 2)
		res = a;
	else
		res = -a;
	return res;
}
```
*On s'autorise à dire `1-4` pour décrire le programme des lignes 1 à 4 (compris)*

$$\psi_1 \equiv \\result \geq 0$$  
$$WP(g, \psi_1)\\
\equiv WP(1-4, WP(5, \psi_1))\\
\equiv WP(1-4, \psi_1[\backslash result \leftarrow res])\\
\equiv a > 2 \Rightarrow WP(2, \psi_1') \land \neg (a > 2) \Rightarrow WP(4, \psi_1')\\
\equiv a > 2 \Rightarrow \psi_1[\backslash result \leftarrow res][res \leftarrow a] \land \neg (a > 2) \Rightarrow \psi_1[\backslash result \leftarrow res][res \leftarrow -a]\\
\equiv a > 2 \Rightarrow a \geq 0 \land \neg (a>2) \Rightarrow -a \geq 0\\
\equiv \top \land a \leq 2 \Rightarrow -a \geq 0\\
\equiv \neg (a\leq 2) \lor -a \geq 0\\
\equiv a > 2 \lor a \leq 0$$

`\old(a)` : la valeur de `a` au début du programme

