---
layout: cours
precedent: 5
suivant: 7
---

# Mémoire cache

La bande passante mémoire limite les performances.

Comment obtenir les performances promises par les processeurs :  
- Exploiter une hiérarchie mémoire : Plus une mémoire est petite plus son temps de réponse peut être court. Placer les données les plus importantes dans les mémoires les plus rapides.
- Exploiter la localité temporelle : Règle 90/10 "90% de l'exécution se déroule dans 10% du code". [...]
- Exploiter la localité spaciale : [...]

Vitesse des mémoires : Registres(0.3ns), Cache L1 (1ns), Cache L2 (3-10ns), Cache L3 (10-20ns), RAM (50-100ns), disque dur (50-100micro-secondes).

La vitesse de récupération de données dans un registre du CPU est de 0.3 nano-secondes.

## Localité spaciale

Forte probabilité qu'une donnée soit accédée prochainement si elle est à proximité dune donnée récement accédée.

[...]

La mémoire est une séquence de ligne. La RAM ne travaille qu'avec des lignes. Lorsque le CPU réclame un mot absent du cache, c'est toute la ligne qui est chargée.

L'agencement des données en mémoire a de l'importance. En c, les tableaux sont triés en mémoire de façon à occuper une ligne.

## Réalisation matérielle des caches

Le cache est une table de hachage réalisé matériellement. La clef est l'adresse. Le nombre de conflits possibles est limité car gravé en silicium. La fonction de hachage doit optimiser la fréquence des conflit (et tenir en compte de la règle de localité spatiale et temporelle).

### Fully-associative cache

Pour être très rapide, les comparaisons se font toutes en parallèles. Il y a un comparateur par ligne et beaucoup de multiplexeurs. Malheureusement, c'est trèx couteux en silicium et énergie. Il est utilisé habituellement dans les TLB. C'est trop complexe pour des caches de grande taille.

### Direct-mapped cache

Les bits de poids faible de l'adresse de ligne prédétermine l'emplacement dans le cache. Les bits de poids fort servent de tags (table de hachage). L'adresse se décompose en tag-index-offset. On regarde la ligne de cache numéro index si le tag est le même alors on peux chercher la valeur à offset, sinon c'est pas bon.

### N-way associative cache

C'est plusieurs direct-mapped cache mis ensemble. on cherche la valeur dans tout les sous-caches et on envoie les résultats dans un multiplexeur pour avoir la vrai valeur.

## Optimisation de l'utilisation du cache

Types de défauts de cache :
- cold miss : premier accès à une variable. Remède : regrouper des variables (structures, tableaux)
- capacity miss : le cache est complet. Remède : revoir l'ordre des boucles, algorithmes, travailler plus intensément sur une plus petite zone de données.
- conflict miss : le cache n'est pas complet mais son associativité est trop faible. Remède : regrouper des variables, revoir l'alignement des données

## Multi-coeur

Protocole MESI : Chaque lige de cache peut être dans l'un de ces quatres états :
- Modified. Le ligne n'est présente que dans le cache actuel, et est dirty.
- Exclusive. Le ligne n'est présente que dans le cache actuel, et est clean.
- Shared. Le ligne est peut-être dans d'autres caches, et et dirty.
- Invalid. La ligne de contient pas de données.

*clean : valeur dans cache = valeur dans mémoire.*  
*dirty : valeur dans cache pas forcément égal à valeur dans mémoire.*

Il est possible qu'une ligne de cache fasse du ping-pong entre deux caches. C'est un faux-partage. Cela fait perdre beaucoup de temps.

## Instruction atomique

Protéger une opération sur une donnée unique sans prendre de mutex, prolonger MESI jusque dans les registres.  
Enchaînement atomique "lecture L1, opération, ecriture L1" réalisé via la mémoire transactionnelle :
- acquérir la ligne de cache et positionner son état à modified
- exécuter l'opération
- écrire la valeur si la ligne est toujours là, recommencer sinon

## Non Uniform Memory Access

Réseau de communication inter noeuds. Un noeud est des processeurs + des mémoires. La latence mémoire dépend de la distance entre le processeur et la mémoire.

Outils pour le placement mémoire :
- OpenMP, clause `affinity()`
- HWLOC
- Libnuma, placement des threads
- Allocation first touch, stratégie d'allocation paresseuse des pages
