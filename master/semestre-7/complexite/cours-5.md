---
layout: cours
usemathjax: true
precedent: 4
---

# Calculabilité

Un calcul est un programme, algorithme.  
Une fonction $$f:\mathbb{N} \to \mathbb{N}$$ est calculable s'il existe un programme qui calcule f.

Cette pseudo-définition est floue : est-ce qu'il existe des fonctions "c-calculable", mais pas "java-calculable" ? Est-ce qu'on doit faire une preuve de calculabilité à chaque fois qu'on change de langage ? ou d'ordinateur ?

Heureusement non : on admettra la thèse de Church-Turing, qui dit que tous les modèles "raisonnables" de calcul sont équivalents.

Il existe des fonctions non-calculables :
- Combien de mots binaire y a-t-il ?
- Combien d'algorithme existe-t-il ?
- Combien de fonction  $$f:\mathbb{N} \to \mathbb{N}$$ existe-t-il ?

Ces ensembles sont non-dénombrables donc les problèmes sont non-calculables.

Un ensemble D est dit dénombrable s'il est en bijection avec $$\mathbb{N}$$.  
Un ensemble D fini ou dénombrable est dit au plus dénombrable.

Quelques ensembles non dénombrables :
- L'ensemble des nombres réels
- L'ensemble des suites infinies d'entiers
- L'ensemble des parties de $$\mathbb{N}$$
- L'ensemble des fonctions de $$\mathbb{N}$$ dans $$\{0, 1\}$$

### LOOP, WHILE, GOTO

On commence par quelques "langages de programmation" basiques : LOOP, WHILE, GOTO  
A chacun de ces langages on associe une classe de fonctions calculables par des programmes de ce langage.  
On va montrer que WHILE-calculable est équivalent à GOTO-calculable. Par contre, LOOP-calculable est plus restrictif.  
On va présenter un autre modèle de calcul, la machine de Turing, et montrer qu'elle définit la même classe de fonctions - qu'on va appeler la classe de fonctions calculables : WHILE-calculable = Turing-calculables

LOOP : on sait combien de tour de boucle on va faire.  
WHILE : on ne sait pas combien de tour de boucle on va faire.  

### Programmes LOOP

Exemple :
```code
x := y;
LOOP (z) DO x := x+1 OD;
```
calcule x = y + z

L'effet de `LOOP (x) DO P OD;` est d'itérer x fois le programme P.

#### LOOP-calculable

Soit P un programme LOOP utilisant les variables $$x_0 = res, x_1, ..., x_l$$.
L'entrée de P est un k-uplet stocké dans les variables x.  
L'effet de P sur x est noté P(x).  
La sortie de P est la valeur de la variables res à la fin du calcul de P. La fonction calculée par P est notée $$f_P$$.

Une fonction $$f : \mathbb{N}^k \to \mathbb{N}$$ est LOOP-calculable s'il existe un progamme LOOP-calculable qui la calcule. $$f(n) = f_p(0, n, 0, ..., 0)$$.

Un progamme LOOP termine toujours, donc les fonctions LOOP-calculables sont totales.

### Programmes WHILE

Les programmes WHILE sont définis à partir des programmes LOOP, en rajoutatn l'instruction "while" : `WHILE (x != 0) DO P OD`

Par définition, toute fonction LOOP-calculable est aussi WHILE-calculable.  
Les fonction WHILE-calculables ne sont pas totales. (une fonction while peut ne jamais terminer).

### Programmes GOTO

Syntaxe : Un pogramme $$P = l_1; ...; l_m$$ est une séquence d'instructions.  
Les intructions possibles sont :
- x := y +/- c
- x := c
- IF (x=0) THEN GOTO $$l$$ FI (saut conditionnel)
- GOTO $$l$$ (saut inconditionnel)
- HALT

Il est possible de réécrire un programme WHILE en un programme utilisant une seule boucle WHILE.

### Machine de Turing

Une machine de Turing comporte :
- Une bande **infinie** à droite et à gauche faite de cases consécutives.
- Dans chaques cases se trouve un symbole, éventuellement blanc $$\square$$
- une tête de lecture-écriture
- Un contrôle à nombre **fini** d'états.

Le nombre d'états d'une machine de Turing est fini.  
La bande représente la mémoire infini de la machine.  
L'accès à la mémoire est séquentiel : la machine peut bouger sa tête à droite ou à gauche d'une case à chaque étape.

#### Formalisation

Une machine de Turing (MT) à une bande $$M = (Q, q_0, F, \Sigma, \Gamma, \delta)$$ est données par :
- $$Q$$, ensemble fini d'états
- $$q_0$$, état initial
- $$F \subseteq Q$$, ensemble d'états finaux
- $$\Gamma$$, alphabet fini de la bande avec $$\square \in \Gamma$$
- $$\Sigma$$, alphabet d'entrée, avec $$\Sigma \subseteq \Gamma \setminus \{\square\}$$
- $$\delta$$, ensemble de transition. Une transition est de la forme $$(p, a, q, b, d)$$, notée $$p \overset{a, b, d}{\to} q$$ avec
	- $$p, q \in Q$$,
	- $$a, b \in \Gamma$$,
	- $$d \in \{\leftarrow, -, \rightarrow\}$$.

*On supposera qu'aucune transition ne part d'unn état de $$F$$.*

On représente souvent une MT comme un automate, en changeant les étiquettes des transitions.

#### Fonctionnement d'une machine de Turing

Initialement, un mot $$w$$ est écrit sur la bande entouré de $$\square$$.  
Un calcul d'une MT sur $$w$$ est une suite de pas de calcul. Cette suite peut être finie ou infinie. Le calcul commence avec la tête de lecture-écriture sur la première lettre de $$w$$ et dans l'état initial $$q_0$$.  
Chaque pas de calcul consiste à appliquer une transition, si possible. Le calcul ne s'arrête que si aucun transition n'est applicable.

Chaque pas consiste à appliquer une transition. Une transition de la forme $$p \overset{a, b, d}{\to} q$$ est possible seulement si :
- la machine se trouve dans l'état $$p$$
- le symbole se trouvant sous la tête de lecture-écriture est $$a$$

Dans ce cas l'application de la transition consiste à :
- changer l'état de contrôle qui devient $$q$$
- remplacer le symbole sous la tête de lecture-écriture par $$b$$
- bouger la tête d'une case à droite si $$d = \leftarrow$$, à gauche si $$d = \rightarrow$$, ne pas bouger si $$d = -$$

#### Configuration et calcul

Une **configuration** (état généralisé) représente un instantanné du calcul.  
La configuration $$uqv$$ signifie que l'état de contrôle est $$q$$, le mot écrit sur la bande est $$uv$$ entouré par des $$\square$$, et la tête de lecture est sur la première lettre de $$v$$.  
La configuration initiale sur $$w$$ est donc $$q_0w$$.  
Pour deux configurations $$C, C'$$, on écrit $$C \vdash C'$$ lorsqu'on obtient $$C'$$ par application d'une transition à partir de $$C$$. Le calcul d'une machine de Turing est une suite de configurations.

Il y a trois cas de calculs :
- Le calcul est infini
- Le calcul s'arrête sur un état final
- Le calcul s'arrête sur un état non final

#### Langague acceptés

On peut utiliser une machine pour accepter des mots.

Le langage  $$\mathfrak{L}(M) \subseteq \Sigma*$$ des mots acceptés par une MT $$M$$ est l'ensemble des mots $$w$$ sur lesquels il existe une calcul **fini**.

On dit qu'une machine est **déterministe** si, pour tout $$(p, a) \in Q \times \Gamma$$, il existe **au plus** une transition de la forme $$p \overset{a, b, d}{\to} q$$.  
SI $$M$$ est déterministe, elle n'admet qu'un calcul par entrée.

