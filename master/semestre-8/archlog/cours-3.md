---
layout: cours
precedent: 2
---

## Patterns structurels

Les patrons structurels servent à organiser les classes d'un programme.

- Decorator
- Composite

### Decorator

Permet de décorer un objet pour lui ajouter des fonctionnalités, sans le modifier.  
On attache dynamiquement des responsabilités à un objet. Les décorateurs fournissent une alternative à l'héritage, pour étendre les fonctionnalités.

Indication d'utilisation :
- Ajouter dynamiquement des responsabilités à des objets individuels, de façon transparente, sans affecter les autres objets
- Des responsabilités qui doivent être retirées
- Quand l'extension par héritage est impraticable (trop de combinaisons)

Conséquences d'utilisation :
- Plus de souplesse que l'héritage
- Evite la surcharge des fonctionnalités des classes parentes
- Un composant n'est pas identique
- Plein de petits objet, difficile à comprendre

### Composite

Cela permet d'avoir un objet de type A qui est un ensemble d'objets de type A. Le client peut ainsi traiter une composition d'objet de la même manière qu'un seul objet.

Indication d'utilisation :
- On souhaite représenter des hiérarchies de l'individe à l'ensemble
- On souhaite que le client n'ait pas à se préoccuper de la différence entre la combinaison d'objets et objets individuels

Conséquences d'utilisation :
- Définit des hiérarchies de classes consistant en des objets primitives et des objets composites
- Simplifie le niveau client
- Ajout de nouveau composant simplifié
- Difficile de contrôler ce qu'il y a dedans

### Proxy

Fournit au client un remplaçant d'un objet pour contrôler l'accès à cet objet.

Indication d'utilisation :
- Procuration à distance : représentation local d'un objet distant
- Procuration virtuelle : crée des objets lourds à la demande
- Procuration de protection : contrôle l'accès à l'objet original
- Référence intelligente : remplacement d'un pointeur brut

Conséquences d'utilisation :
- Peut cacher le fait qu'un objet réside dans un autre espace d'addresse
- optimisation
- sécurité
