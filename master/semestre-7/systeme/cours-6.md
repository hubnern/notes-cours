---
layout: cours
precedent: 5
suivant: 7
---

# Gestion de la mémoire

## Historique

Au départ, un système avait un seul processeur, et un seul processus est lancé a la fois. Un processus avait toute la mémoire pour lui tout seul. L'adresse de départ du programme était connue à la compilation car elle était toujours lancé a partir de cette adresse.

**MS-DOS :**
- Un seul processus à la fois.  
- Max 1MB de RAM  
- 16 bits d'adressage "réel", aucune protection

**Vers le multi-tâche :**  
Raison vers la multi-tâche : exécuter plusieurs processus d'être en mémoire simultanément.  
L'objectif est de garder le processeurs utilisé le plus de temps possible. Lorsque la machine cherche un fichier dans une bande magnétique, le processeur ne fonctionne pas et le programme attend. Dans ce moment là, on lance un autre programme pendant que la machine cherche le fichier de l'autre programme.

**Nouvelles complications :**
- Comment compiler un processus alors qu'on ne sait pas à quel addresse mémoire ils seront lancé ?
- Comment gérer une mémoire fragmenté ?
- Comment laisser les processus grandir dans la mémoire ?
- Comment protéger la mémoire entre les processus ?

## Compiler un programme sans savoir son adresse mémoire initiale

*A quelles adresses sont nos variables ? Comment modifier ses valeurs si on ne sait pas les adresses ?*

Le code est relocalisé durant la compilation. Le compilateur met toutes les addresses à "zéro" et créé une table pour savoir les relocalisations.  
Le chargeur du programme localisera correctement les adresses des variables grâce à la table.

## Défragmenter la mémoire / agrandir la zone mémoire

À un moment, le système devra fusionner les morceaux de mémoire inutilisés pour qu'un programme puisse y être logé. Le système devra éventuellement déplacer des processus dans la mémoire pour fusionner deux blocs vides ensemble.

Il n'est pas possible de coder un déplacement de façon peu couteuse. On demande de l'aide aux architectes. Les constructeurs de processeurs ont ajoutés deux nouveaux registres.

Ces registres enregistrent la taille limite de mémoire des processus, et l'adresse initiale des processus. À chaque déplacement mémoire d'un programme, l'adresse de base de ce programme est modifié. Lors de l'augmentation de zone mémoire c'est la taille limite dans le registre qui est modifiée.

Les translations d'adresses sont très peu couteuse car il n'y a plus de traduction de chaque adresse à chaque déplacement. Il suffit de modifier l'adresse de base du processus. Chaque adresse utilisées dans le processus sont relative à cette adresse initiale.

Plus tard, chaque segment mémoire (heap, data, code, stack) d'un processus ont été modifiés pour utiliser les registres limite et base.  
En utilisant la fragmentation des segments mémoire, deux processus peuvent avoir le même segment mémoire, il suffit qu'ils aient la même adresse comme adresse de base de ce segment. On peut ainsi lancer plusieur même programme en chargeant une seule fois le code et en partageant le segment mémoire code entre ces processus.

## Segmentation

Séparer l'espace d'adressage en plus petits morceaux permet une allocation plus flexible.  
Mémoire partagée entre plusieurs processus est possible.  
Les accès à la mémoire sont controllées par segments.
