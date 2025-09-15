---
layout: cours
usemathjax: true
precedent: 4
suivant: 6
---

## Plus court chemin

### Théorème

Il existe un plus court chemin de u à v dans (G, w) si et seulement si il existe un chemin de u à v et aucun chemin de u à v ne contient de circuit de longueur totale strictement négative.

L'ensemble des plus courts chemins à partir d'un sommet s forme une arborescence de racine s.  
On notera par $$d(v)$$ la distance trouvée de s à v et par $$\pi (v)$$ le prédécesseur de v sur le chemin de s à v de longueur $$d(v)$$.

### Principe du relâchement

Soient d la valeur de plus court chemin trouvé à l'instant t entre un sommet s et les sommets de G,  
$$\pi$$ les prédécesseurs de chaque sommet sur les plus courts chemins trouvés à l'instant t depuis le sommet s,  
uv un arc de G.  

Relacher(G, $$\omega$$, u, v) :  
si d(u) + $$\omega$$(uv) < d(v) alors  
	d(v) <- d(u) + $$\omega$$(uv)  
	$$\pi$$(v) <- u  
fin si

## Algorithme de Dijkstra

On peut utiliser l'algorithme de Dijkstra à condition que la fonction $$\omega$$ de pondération des arcs soit positive.

## Algorithme de Bellman
On peut utiliser l'algorithme de Bellman à condition que le graphe G soit sans circuit.  
*On peut obtenir un tri topologique sur les sommets de G.*
