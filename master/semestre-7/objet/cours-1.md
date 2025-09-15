---
layout: cours
suivant: 2
---

## Paradigme de programmation

**Programmation fonctionnelle** : Déclarative, traitant des opérations successivement en évitant les mutations de données et les changements d'état.  
**Programmation Orientée Objet** : Les données sont la première approche. Les objets qui modélisent ces données ont des propriétés et des méthodes. Le programme n'est plus une séquence d'instructions mais un ensemble d'interactions entre ces objets.

*Les données sont plus permanentes que les traitements.*

# La programmation Orientée Objet

Toute chose est un objet (données+fonctionnalités).  
Chaque objet est d'un type précis (instance d'une classe).  
Une classe décrit un ensemble d'objets partageant des caractéristiques communes et des comportements communs.

## Caractéristiques d'un modèle orienté objet

### Modularité

- Scinder un programme en composant individuels afin d'en réduire la complexité.
- Partitionner le programme pour créer des frontières bien définies pour réduire la complexité.

*Choisir un bon ensemble de modules pour un problème données est presque aussi difficile que choisir un bon ensemble d'abstractions.*

## Les concepts de base

**Objets** : Unités de base organisées en classes et partageant des traits communs

**Classes** : Le type d'un objet.

**Encapsulation** : Un objet protège son état : 
- Les structures de données et les détails de l'implémentation sont cachés aux autres objets.
- La seule façon d'accéder à l'état d'un objet est d'utiliser une de ses méthodes.
- Un objet doit être le seul a modifier ses données.
- Un objet ne devrait pas laisser lire ses données.

**Abstraction** et encapsulation sont complémentaire.

**Responsabilité** : Un objet est responsable de ses traitements. Il peut délégué des traitements à d'autres objets.

**Communication** : Les objets communiquent par envoi de messages. L'envoyeur connaît le destinateur, mais le receveur ne connaît pas l'envoyeur.

**Héritage** : Chaque classes hérite d'un classe parente pour pouvoir factoriser le code. La classe fille aura les méthodes et attributs de sa classe parente.

**Polymorphisme (surcharge)** : Il est possible d'avoir plusieurs méthodes ayant le même non mais de signature différents.

**Signature de méthode** : La signature d'une méthode est composée du type de retour et des types des paramètres.

**Types primitifs** : Les seuls types qui ne sont pas des objets (`int`, `char`, `float`, etc).
