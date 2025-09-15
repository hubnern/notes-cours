---
layout: cours
usemathjax: true
suivant: 2
---

Le parallélisme est important sinon on n'utiliserait pas l'ordinateur complètement.  
Les processeurs sont de plus en plus hétérogènes.  
La puce Apple M1 contient un gpu à l'intérieur.

## Pourquoi calculer en parallèle ?

Traiter des gros problèmes (simulation scientifique, big data).

Gagner du temps. Gagner en précision (simulation, jeu vidéo, apprentissage). Faire plus de tests (cryptanalyse, minage cryptomonnaie). Prendre des décisions plus vite que les autres (finance).

La simulation haute performance est devenue le troisième pivot de la science (expérimentation, modélisation mathématique, simulation), incontournable dans les grands groupes industriels (simulation phénomènes physiques/chimiques, maquette numérique pour conception et fabrication).

Meilleur plateforme de calcul parallèle (2020) : Fugaku (Japon), contient 7 million de CPU fujitsu, 537 PetaFlops (flop = float operation / second).

## Simulation

### Modélisation météorologique

L'objectif est de calculer l'humidité, pression, température et vitesse du vent on fonction de x, y, z, et t.  
La résolution d'un système dynamique est impossible à résoudre formellement.

On fait un simulation numérique. On discrétise l'espace (représente une portion du monde par un cube, tout le cube aura la même valeur), on initialise le modèle, puis on le fait évoluer (discrétiser le temps (ex : par pas de 60s), résoudre pour chaque maille un système d'équations).

Malheureusement, une prévision n'est pas une masse informe d'opréation flottantes. Il faut aussi tenir compte du temps d'accès aux données (récupérer une donnée sur disque est beaucoup plus long que faire un calcul).

## Loi d'Amdahl

accélération = temps du meilleur prog séquentiel / temps du prog parallèle

On note :
- $$s$$ la part séquentielle d'un programm
- $$1-s$$ la part parallélisable de l'exécution
- $$p$$ le nombre de processus

$$durée\ \ parallèle \geq (s + \frac{1-s}{p}) \times  durée\ \ séquentielle$$

L'accélération de l'exécution sur une machine à $$p$$ processeurs est ainsi bornée par :
$$accélération \leq \frac{1}{s + \frac{1-s}{p}} \leq \frac{1}{s}$$

### Amdahl et chemin critique

Pour le tri fusion.  
Dans le pire des cas, on recopie tous les éléments à chaque niveau de l'arbre : $$2^k$$ écritures par niveau.

Cout séquentiel : coût d'un tri de $$2^k$$ éléments : $$k*2^k$$ écritures  
Cout parallèle : coût d'une branche d'un tri de $$2^k$$ éléments : $$2 +  4 + ... + 2^k = 2^{k+1} - 2$$ écritures.

## Comment obtenir des performances optimales ?

Il faut extraire sufisament de parallélisme pour occuper toutes les unités de calcul. Nécessite souvent du calcul supplémentaire.

Quelle finesse de grain de parallélisation ?
- Gros gain
	- plus : efficacité des unités de calcul
	- parallélisme ?
- Grain fin
	- plus : parallélisme
	- efficacité des unités de calcul ?
	- surcoût ?

Il faut utiliser correctement la mémoire, et ne pas être limité par la bande passante de la mémoire.
