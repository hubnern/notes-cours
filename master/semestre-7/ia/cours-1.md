---
layout: cours
usemathjax: true
suivant: 2
---

# Intelligence Artificielle

Limites intrinsectes à l'apprentissage :
- être résistant aux attaques
- comment garantir la qualité des décisions
- ne pas avoir de biais, nottament a partir des données
- prendre (vite) la meilleure décision

L'IA aime les jeux :
- Preuve d'intelligence :
	- tous les hommes jouent
	- les résultats sont faciles à vulgariser
- Machines adaptées aux jeux :
	- Loins des problèmes physiques du monde réel
	- Tout est souvent formalisable

On va étudier :
- les règles de jeux formalisables
- les jeux à deux joueurs
- les jeux à somme zéro (ce que l'un gagne, l'autre le perd)
- les jeux ouvert (l'état du jeu est connu dans sa globalité)

## Recherche totale

La recherche totale est la simulation de tous les coups possible depuis la position initiale. Cela permet de construire l'arbre de jeu.

Malheureusement, l'explorer tous les coups possibles d'un graphe de jeu est rapidement exponentiel.

## Une première recherche intelligente

On ne veut qu'une stratégie gagnante, on n'a pas besoin de toutes les stratégies.  

Si une branche amie mène à la victoire, ne pas développer les branches voisines.  
Si une branche ennemie mène à la défaite, ne pas développer les branches voisines.  
De cette manière, on peut éviter de dérouler une branche sans perdre des informations.

Malheureusement :
- On est obligé d'aller jusqu'aux feuilles de l'arbre
- Peu de jeux en pratique permettent d'utiliser ces techniques.

## Recherche avec horizon

On ne développe l'arbre de recherche que jusqu'à une certaine profondeur maximale. Les feuilles de l'arbre ne sont plus forcément des positions finales du jeu.  
On ne peut donc plus se contenter de l'information "gagné" ou "perdu". Il faut évaluer les positions.

Il faut mettre en place des heuristiques. Des fonctions qui associent une valeur à un plateau. Plus cette valeur sera positivement grande, plus on est proche de la victoire.  
C'est une fonction statique, c'est-à-dire que on ne calcule qu'en fonction du plateau tel qu'il est, pas en fonction des coups possibles.

On développe l'arbre jusqu'à une certaine profondeur données.  
On calcule l'heuristique.  
On fait remonter l'évaluation pour trouver les meilleur chemins.

On cherche la branche permettant de maximiser la valeur heuristique obtenue sur le plateau après les p prochains coups.

L'algorithme MinMax permet de faire une recherche avec horizon.

```c
Fonction MinMax(etat): // évaluation niveau ennemi
	etat: Plateau de jeu courant
	Si EstFeuille(etat) alors:
		Retourner evaluer(ENNEMI, etat) // heuristique
	Fin Si
	pire <- infini
	Pour Tout successeur s de etat Faire:
		pire <- min(pire, MaxMin(s))
	Fin Pour
	Retourner pire

Fonction MaxMin(etat): // évaluation niveau Ami
	etat: Plateau de jeu courant
	Si EstFeuille(etat) alors:
		Retourner evaluer(AMI, etat) // heuristique
	Fin Si
	meilleur <- -infini
	Pour Tout successeur s de etat Faire:
		meilleur <- max(meilleur, MinMax(s))
	Fin Pour
	Retourner meilleur

```

$$\alpha\beta$$ :  
Meilleure version de minmax. Ami cherche a maximiser $$\alpha$$, et ennemi à minimiser $$\beta$$. Si on parcours une branche, il est possible de trouver une valeur telle que les branches voisines n'ai pas de meilleure valeure pour $$\alpha / \beta$$. On peut ainsi élaguer des branches rapidement.

### Négamax

Idée : Au lieu d'alterner deux fonctions Min/Max, on se restreint à une seule fonction en utilisant l'opposé du résultat à chaque niveau.

```c
Fonction NegaMax(etat)
	etat: Plateau de jeu courant
	meilleur: évaluation
	[...]
```


### Performances de $$\alpha\beta$$

On appelle arbre de jeu uniforme de largeur $$l$$ un arbre de jeu où tous les noeuds non terminaux ont exactements $$l$$ fils.

MinMax explore donc exactement $$l^p$$ noeuds, où $$p$$ est la profondeur de rechercher.

**Types de noeuds visités lors de l'exploration** :  
Tous les noeuds élagiés lors de la recherche Neg$$\alpha\beta$$ le sont dès $$\alpha\geq\beta$$. L'appel récursif Neg$$\alpha\beta$$ se ferait sur la fenêtre $$[\alpha\beta]$$ avec $$\alpha\leq\beta$$.  
Trois types de noeuds jamais élagués :  
A chaque niveau de l'arbre, $$\alpha\beta$$ est appelé avec un certain \[...\]
1. \[...\]
2. \[...\]
3. \[...\]

Visite de l'arbre :  
L'algorithme $$\alpha\beta$$ utilisé avec la fenêtre $$[-\infty, +\infty]$$ regarde au moins l'arbre critique, c'est-à-dire l'ensemble des noeuds de type 1, 2 et 3 et uniquement celui-ci dans le cas où l'arbre est parfaitement ordonné.  
Si l'arbre n'est pas parfaitement ordonnée, on visitera plus de noeuds, jusqu'à l'arbre complet ($$l^p$$ noeuds).  
La borne inférieure est de $$l^{p/2}$$.

Résultat des performances :  
\[\]
L'ordre de développement des fils est promordial pour ...

Problématiques du temps réel :  
La profondeur de recherche doit petre fixée **avant** de lancer la recherche. *L'horizon doit être équilibré sur tout l'arbre de recherche.*
- Comment garantir au joueur que l'ordinateur ne va pas passer trop de temps à réfléchir ?
- Comment garantir avec des machines différentes ?
- Comment lier le temps passé à réfléchir avec la profondeur maximale de l'horizon ?
- Que se passe-t-il si l'arbre n'est pas homogène ? (hypothèse très réaliste)

### Iterative Deepening

Idée : étendre peu à peu l'horizon de $$\alpha\beta$$.

Algorithme :  
Soit $$p$$ initialisé à un horizon immédiat. Faire une recherche à l'horizon $$p$$. S'il reste du temps, tout oublier (sauf le coup à jouer) et incrémenter $$p$$.

Performances : ID semble refaire beaucoup de fois la même chose.

La croissance exponentielle des arbres montre intuitivement que la majorité des noeuds se situent sur les feuilles. Développer le dernier niveau coûte le plus cher, d'autant plus si on a un grand facteur de branchement.

Avantages :
- Grande souplesse dans la gestion du temps. Le programme peut donner la meilleur valeur trouvée à n'importe quel moment.
- Couplé à $$\alpha\beta$$, ID peut même devenir plus efficace qu'un seul $$\alpha\beta$$.

Comment ? $$\alpha\beta$$ élague d'autant plus que les meilleurs coups sont développés en premier. On profite donc de l'ancien $$\alpha\beta$$ à profondeur $$p$$ pour ordonner les fils de profondeur $$p + 1$$.

Idée : Plutôt que de tout oublier après chaque relance de ID, on va essayer de mémoriser pour réordonner l'arbre ensuite.

Première méthode : Garder tout l'arbre de recherche en mémoire, l'ordonner à chaque niveau puis relancer $$\alpha\beta$$ directement sur le nouvel arbre optimal.

Seconde méthode : Réserver une zone mémoire pour associer à chaque plateau de jeu déjà vu le meilleur des fils et la valeur heuristique associée (ainsi que sa profondeur).

Tous les programmes doivent fixet un horizon. Pourtant, cela entraîne deux problème importants :
- Manque de clairvoyance : On ne peut prévoir un coup (même fatal) au-delà de l'horizon.
- Aveuglement volontaire : L'approche MinMax va pousser le programme à tout faire pour repousser un coup mauvais (mais inévitable) au delà de l'horizon.

#### Atténuation d'horizon

Zoomer où l'on va aller :  
Une fois que l'on a fait le choix du coup à jouer, passer un peu de temps pour voir si la fonction heuristique ne nous a pas trompée. On va déséquilibrer l'arbre de recherche.

Reconsidérer la notion d'horizon :  
Définir l'horizon en fonction de l'intérêt des coups joués et non en fonction du nombre de coups.