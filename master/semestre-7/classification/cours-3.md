---
layout: cours
usemathjax: true
precedent: 2
suivant: 4
---

# Méthode d'apprentissage

## Apprentissage supervisé

Les données d'apprentissage sont accompagnés par labels indiquant leurs classes.
Les nouvelles données sont classifiés en se basant sur le set d'entraînement.

## Apprentissage non supervisé

Le label de classe des elements observés (entraînement) n'est pas connu.
Le but est de déceler l'existence de classes ou groupes dans les données.

## Classifieur (évaluation d'un modèle d'apprentissage)

A partir des données de test, on le sépare en deux parties. L'une servira a faire du machine learning et apprendre. L'autre servira de test pour savoir si notre apprentissage est correct.

Si l'ensemble de données est suffisament grand, le dataset est découpé en deux ensemble : Training Set (80%) et Test Set (20%).  
On fait l'apprentissage sur le training set. Une fois l'apprentissage terminé, on le valide avec le test set. Mais une fois que le modèle appris a été validé avec le test set il ne faut pas re-apprendre avec ce set.

##### Cross-validation

On découpe le training set en 4 quarts. On en prend trois, on fait l'apprentissage dessus, et on évalue sur le dernier quart. On fait ainsi pour tout ensemble de quart. La moyenne de tout ces entraînement fera le modèle du training set.

## Matrice de confusion (cas binaire) :  

| |Observation|Observation|
|:---:|:---:|:---:|
|Expectation|true positive|false positive|
|Expectation|false negative|true negative|

Exemple :

| |Chat (positif)|Chien (négatif)|
|:--:|:--:|:--:|
|Chat|50|15|
|Chien|15|20|

## Métrique d'évaluation (cas binaire)

#### Accuracy

Il s'agit de la fraction des exemples bien classés par rapport à toutes les prédications.  
$$Acc = \frac{TP + TN}{TP + TN + FP + FN}$$

*Si les deux classes sont fortement déséquilibrées, alors cette mesure n'apporte pas d'information pertinente.*

#### Precision

C'est la fraction des vrais positifs parmi les exemples prédits comme positif. 

#### Recall (sensibilité)

Il s'agit du rapport entre le nombre d'exemples de la classe c correctement prédit ...

#### F-measure

## Métrique d'évaluation (cas multi-classe)

Il s'agit d'une généralisation de la classification binaire.  
La diagonale contient les TP de chaque classes.  

# Apprentissage supervisé

## k-Nearest-Neighbor Classifieur

> Si ça marche comme un canard, crie comme un canard, c'est que c'est probablement un canard.

On a besoin de trois choses :
- Un ensemble d'entraînement
- Une mesure de distance
- La valeur de k, le nombre de voisins à interroger)

Pour classifier un nouvel enregistrement :
- calculer la distance vers les autres enregistrements
- identifier k plus proches voisions
- utiliser la classe des k voisions les plus proches pour déterminer la classe du nouvel enregistrement.

Il faut aussi bien choisir ses métriques pour mesurer la distance (comment on mesure la distance entre deux données). Il est parfois nécessaire de standardiser les valeurs avant de calculer les distances.  
Ex : distance de Monkowski, euclidienne, de Manhattan

Il faut ensuite déterminer la classe à partir de la liste des voisins. Choisir la classe majoritaire dans le k-voisinage.

Algorithme :  
Soit k le nombre de voisins le plus proches et D l'ensemble d'entraînement.
1. Pour chaque exemple z=(x', ?) de l'ensemble de test
	1. Calculer d(x, x'), la distance de z et chaque (x,y) de D
	2. Choisir $$D_z \subseteq D$$, l'ensemble des k exemples les plus proches de z
	3. $$y' =$$ arg $$max_v \sum_{(x_i, y_i)\in D_z}I(v=y-i)$$ //TODO
2. Fin pour

Choix de la valeur de k : Si k trop petit alors la classification sera sensible au bruit. Si k trop grand, le voisinage peut contenir des éléments d'autres classes.
