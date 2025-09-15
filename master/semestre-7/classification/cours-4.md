---
layout: cours
usemathjax: true
precedent: 3
suivant: 5
---

### Classifieur ML (Maximum likelihood)

On voudrait choisir un classe si Pr(type=valeur \ classe) > Pr(type=valeur \ autre classe)

Pour une observation l, on choisi la classe qui a le plus de ressemblance. 

### Classifieur a priori

On ne dispose pas des données mais d'une connaissance a priori. ON exploite une connaissance pour en déduire une distribution.

> Exemple : "dans la mer, il y a deux fois plus de saumon que de bars"  : P(saumon)=2/3 et P(bar) = 1/3

### Classifieur MAP (Maximum a posteriori)

Règle de décision de Bayes :
- On dispose de la fonction de vraisemblance
- On a les probabilité a priori
- On choisi la classe qui a la plus grande probabilité

Pour avoir les probabilité a posteriori on se sert de la formule de Bayes
