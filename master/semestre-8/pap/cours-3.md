---
layout: cours
precedent: 2
suivant: 4
---

# Équilibrage des charges

Problème réguliers : Charge de calcul prévisible ne dépendant pas des résultats intermédiaires. Une distribution équitable a priori est possible.  
Problèmes irréguliers : Il faut rééquiliber dynamiquement la charge au fur et à mesure.

## Problèmes réguliers

Comment ordonnancer efficacement un ensemble de tâches sur une ensemble de processus ? Problème général NP-complet. Complexité polynomiale pour deux processeurs.

Avec OpemMP :
- `static` : distribution par bloc
- `static,1` : distribution cyclique
- `static,n` : distribution cyclique par n bloc
- `dynamic` : distribution dynamique
- `guided` : distribution par pourcentage des blocs restants

*Il est préférable de commencer par le chemin critique.*

