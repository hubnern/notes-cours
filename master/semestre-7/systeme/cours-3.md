---
layout: cours
precedent: 2
suivant: 4
---

## Ordonnancement dans un ordinateur interactif

Le propriété la plus importante : la réactivité et interactivité des processus

Processus interactif : processus réagissant aux input et output

### L'ordonnancement FIFO

Les processus sont exécuté chacun les un après les autres, selon une file d'attente FIFO.

Pro : algorithme simple  
Cons : famine (un programme boucle à l'infini et bloque la file d'attente)


### L'ordonnancement tourniquet (round-robin)

C'est un ordonnancement FIFO avec de la préemption. A chaque interruption du timer, le processus actif est remplacé par le suivant.

Pro : Pas de famine  
Cons : Pas de priorité

### L'ordonnancement a priorité

Il comporte une liste FIFO par niveau de priorité. Le processus actif est celui en tête de file du niveau de priorité le plus élevé.

Pro : priorité des processus  
Cons : comment choisir les priorités

### Assigner la priorité dynamiquement

On observe les processus pour savoir ce qu'ils font, ce qu'ils utilisent, pour pouvoir changer leur priorité.  
On essaye de prédire le futur en regardant ce qu'il s'est passé dans le passé.

### Stratégie utilisée dans le noyau Linux 4.2

Chaque processus a une quantité de crédit. Ils peuvent dépenser leur crédit pour être actif. Leur cota est remis à zéro à chaque époques.
Une nouvelle époque commence lorsque chaque processus a utilisé tout ses crédits.

### Ordonnancement dans une machine multicœurs

Chaque cœurs à son propre ordonnanceur.
