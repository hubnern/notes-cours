---
layout: cours
usemathjax: true
precedent: 6
---

## Arbres de décision et classification

Méthode supervisée : Données d'entrainement : $$(x_i, y_i)$$

Le but est, à partir d'un tableau de données, de créer un arbre de décision.

### Création de l'arbre de décision

L'arbre est construit de haut en bas récursivement :
- au début tous les tuples sont sur la racine
- les attributs sont qualitatifs (discrétisation s'il le faut)
- les tuples sont ensuite partitionnés en fonction de l'attribut sélectionné
- l'attribut de test est selectionné en utilisant des heuristiques.

Condition d'arrêt du partitionnement : tous les tuples d'un noeud se trouvent dans la même classe.

### Choix de l'attribut de partitionnement

Soit le training set suivant :

|A|B|Classe|
|:-:|:-:|:-:|
|0|1|$$C_1$$|
|0|0|$$C_1$$|
|1|1|$$C_2$$|
|1|0|$$C_2$$|

Un arbre de décision représente la suite de question à poser pour pouvoir classifier un nouvel exemple.  
Le but consiste à obtenir une classification en posant le moins de question possible.  
Dans l'exemple précédent, on dira que l'attribut A apporte plus d'information, respectivement à la classification des exemples, que B.  
Nous avons donc besoin de quantifier l'information apportée par chaque attribut.

### Notions sur la théorie de l'information

Intuitivement, plus un évènement est probable, moins il apporte d'information.  
La quantité d'information associée à un évènement x sera considérée comme une fonction croissante sur son improbabilité : $$h(x) = f(\frac{1}{\mathbb{P}r(x)})$$  
Un évènement certain apporte une quantité d'information nulle, ainsi $$f(1)$$ doit être nulle.

La réalisation de deux évènements indépendants apporte une quantité d'information égale à la somme de leur informations respectives : $$h(x, y) = f(\frac{1}{\mathbb{P}r(x, y)}) = f(\frac{1}{\mathbb{P}r(x) \times \mathbb{P}r(y)})$$  
La fonction choisie pour représenter cela est la fonction log en base de 2 : $$h(x) = -log_2(\mathbb{P}r(x))$$. Elle satisfait les deux conditions : croissante et l'information apportée par deux évènements indépendants est la somme des informations apportées par chacun.

Supposons maintenant qu'il y a deux classes $$P$$, et $$N$$.  
Soit $$S$$ un ensemble qui contient $$p$$ éléments de $$P$$ et $$n$$ éléments de $$N$$. La probabilité qu'un élément soit dans $$P$$ est $$frac{p}{p+n}$$. La quantité d'information nécessaire pour décider si un élément quelconque de $$S$$ se trouve dans $$P$$ ou dans $$N$$ est défini par $$I(p, n) = -p log_2(\frac{p}{p+n} - n log_2(\frac{n}{n+p}$$.

Cas particulier :  
Supposons que $$p$$ soit nul, cela veut dire que $$l(n, p) = 0$$, ce qui est conforme à l'intuition, on n'a pas besoin d'information supplémentaire, on est sur que l'élément est dans $$N$$.

### Gain d'information et arbre de décision

Supposons qu'un utilisatint l'attribut $$A$$, $$S$$ est partitionné en $$\{S_1, S_2, ..., S_v\}$$ (ce qui veut dire que $$A$$ prend $$v$$ valeurs).  
SI $$S_i$$ contient $$p_i$$ tuples de $$P$$ et $$n_i$$ tuples de $$N$$, l'**entropie**, ou la quantité d'information nécessaire pour classifier les objets de tous les arbres $$S_i$$ est $$E(A) = \sum_{i=1}^{v} I(p_i, n_i)$$.  
L'entropie mesure la quantité de désordre qui reste après le choix de $$A$$.  
L'information de codage gagnée en utilisant $$A$$ sera donc $$G(A) = I(p, n) - E(A)$$.

Intuition derrière le gain :  
Pour classer les éléments dans $$S_i$$, nous avons besoin d'une quantité d'information égale à $$I...$$

### Retour à l'exemple

En choisissant A : $$p_1 = 2, n_1 = 0, p_2 = 0, n_2 = 2$$  
L'entropie est $$E(A) =...$$  
Le gain de A est $$G(A) = ...$$

Pour B les gain est de $$G(B) = ...$$

Il vaut donc mieux choisir A.

### Extraction de règles de classification

De la former SI-ALORS, chaque chemin partant de la racine et atteignant une fauille donne lieu à un règles. Chaque paire attribut-value le long d'un chemin forme ...

### Autres critères pour le choix de l'attribut

Indice Gini : $$1 - \sum_{i=1}^{v} f_i^2$$, où $$f_i$$ désigne la fraction des éléments de l'ensemble avec l'étiquette $$i$$ dans l'ensemble.

### Problème de l'overfitting

Par la construction ci-dessus, on obtient des arbres qui classent correctement les exemples du training set. Mais rien ne dit qu'ils seront efficaces pour les exemples de l'ensemble de test. Lorsque l'arbre "colle trop au training set", on parle d'overfitting (sur-entraînement).  
Pour résoudre le problème, on va autoriser des erreurs sur le training set pour obtenir des arbres qui généralisent mieux.

**Pré-élagage** :  
Ne pas découper un noeud si le partage fait basculer la mesure de pertinence en dessous d'un certain seuil. Arrêter quand il y a une classe majoritaire dans le noeud : utiliser un seuil pour détecter un classe dominante. Incovénien ...

**Post-élagage** :  
On construit d'abord l'arbre de décision. Ensuite, on coupe des sous-arbres entiers et on les remplace par des feuilles représentant la classe la plus fréquente dans l'ensemble des données de cet arbre. On commence de la racine et on descend, pour chaque noeud interne (non-feuille), on mesure sa complexité avant et après sa coupure (son remplacement par une feuille), si la différence est peu importante, on coupe le sous-arbre et on le remplace par une feuille.

### Agrégation de modèles

Bagging : utiliser le hasard pour améliorer les performances d'algorithmes de "faibles" ...  
Boosting : ...


