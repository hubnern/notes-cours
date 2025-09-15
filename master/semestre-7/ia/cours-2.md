---
layout: cours
usemathjax: true
precedent: 1
---


## Deep learning

### Neurone

Un neurone prend plusieurs données $$x_p$$, leurs poids $$w_p$$, et un biais $$b$$, et fait la somme $$z = \sum_{i=0}^{p} x_i \times x_i + b$$. Le neurone utilise ensuite sa fonction d'activation avec en paramètre $$z$$.

#### Perceptron

Un perceptron fait une classification binaire supervisée (seulement sur des données linéairement séparables).

Le biais permet de déplacer la ligne de séparation, c'est lui qui déterminer la valeur à $$y=$$.

#### Fonction d'activation

La fonction d'activation la plus utilisée est une sigmoïd, une fonction qui convertie une valeur quelconque en une valeur entre 0 et 1.

Il existe aussi, la fonction linéaire, une fonction seuil, ReLu (enlève les négatifs), radiale

### Réseau de neurones

Un réseau de neurone est un enchaînement de neurones.  
Chaque neuronnes ont leur propre poids et biais. On note $$\theta$$ les paramètres du réseau de neurones (les différents poids et les différents biais).

Le deep learning est un réseau de neurones avec énormément de couches de neurones.

