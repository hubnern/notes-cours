---
layout: cours
usemathjax: true
precedent: 2
suivant: 5
---

### Contradiction ($$\bot$$)
La contradiction est une formule à elle même.  
Ex :  
$$\bot \to P$$  
$$P \to \bot \to Q$$

La valuation de la contradiction est tout le temps fausse.

Un séquent valide si aucune valuation ne satisfait toutes les hypothèses.  
Le séquent est contradictoire.

Exemple :  
$$P\to Q, P, Q\to \bot \vdash R$$  
Ce séquent est valide car aucune valuation ne peut rendre vraie simultanément les trois hypothèses.

#### Élimination de la contradiction ($$\bot _e $$)
"Ex falso quodlibet"  
principe d'explosion  

$$\frac{\Gamma \vdash \bot}{\Gamma \vdash A}\bot _e$$

Nouvelle façon de prouver un séquent : montrer que ses hypothèses entraînent une contradiction.

### Négation ($$\sim$$)

$$\frac{\Gamma , A\vdash \bot}{\Gamma \vdash \sim A}$$

La notation de la négation est usuellement $$\neg$$, mais en coq, la notation est $$\sim$$

$$\sim$$ n'est pas un vrai connecteur.  
C'est en fait une macro : $$\sim A$$ est une abréviation de $$A \to \bot$$  
*attention aux parenthèses*

Ainsi, pas de nouvelles règles de raisonnement et les méta-théorèmes restent valables dans ce nouveau cadre.

### Règles dérivées

##### Expansion de la macro $$\sim$$

$$\frac{\Gamma \vdash A \to \bot}{\Gamma \vdash \sim A}\sim _exp$$

##### Introduction de la négation

$$\frac{\Gamma , A \vdash \bot}{\Gamma \vdash \sim A}\sim _i$$

##### Contraposée

$$\frac{\Gamma \vdash A \to B}{\Gamma \vdash \sim B \to \sim A}contraposée$$

##### Preuve par l'absurde

$$\frac{\Gamma \vdash A \ \ \ \ \Gamma \vdash \sim A}{\Gamma \vdash B}abs$$
