---
layout: cours
usemathjax: true
precedent: 5
suivant: 7
---

## Analyse en composantes indépendantes

Motivations :
- L'ACP est concue pour représenter des données et non pour les classifier
	- elle préserve la variance autant que possible
	- si les directions à plus gande variances coïcident avec les bons choix pour la classification...
- En général, la direction conservant la plus grand variance peut ne pas être avantageuse pour la classification

Idée de base :  
Préférer une projection sur une ligne concervant la séparation entre les classes

Formalisation :  
Supposons que l'on dispose d'un corpus avec n enregistrements, chacun de dimension d. Chaque élément $$x_i$$ appartient à une classe. Le nombre de classes est 2 (classification binaire). $$n_1$$ éléments appartiennent à la première classe, $$n_2$$ éléments appartiennent à la seconde classe.  
Supposons que l'on projette chaque élément $$x_i$$ du corpus sur une droite passant par l'origine et ayant le vecteur unitaire v comme vecteur directeur.
- Le réel $$v^tx_i$$ représente la distance entre la projection de $$x_i$$ et l'origine
- $$v^tx_i$$ est la projection de $$x_i$$ sur un sous-ensemble de dimension 1

Mesurer la qualité de la projection en terme de séparation des classes :
$$\mu_1, \mu_2$$ les moyennes des éléments classés 1 et 2 respectivement
$$\tilde{\mu}_1, \tilde{\mu}_1$$ leurs projections sur le nouvel axe

$$\tilde{\mu}_i = \frac{1}{n_i} \sum_{x_j\in C_i} v^tx_j = v^t(\frac{1}{n_i} \sum_{x_j\in C_i} x_j) = v^t\mu_1$$

Intuitivement, on a envie de chosir un axe tel que $$|\tilde{\mu}_2 - \tilde{\mu}_2|$$ soit la plus grande possible.  

On doit normaliser par la matrice de dispersion des deux classes. La solution de Fisher est de projeter les éléments du corpus sur la ligne dans la direction v qui maximise : 
$$J(v) = \frac{(\tilde{\mu}_2 - \tilde{\mu}_2)^2}{\tilde{s}_1^2 + \tilde{s}_2^2$$  

On définit à présent la matrice de dispersion inter-classes : $$S_B = (\mu_1 - \mu_2)(\mu_1 - \mu_2)^t$$  
Ainsi $$J(v) = \frac{v_tS_Bv}{v_tS_wv}$$

Il faut donc résoudre l'équation $$\frac{d}{dv} J(v) = 0$$.

Comme pour l'ACP, on résout l'équation et on trouve que la meilleur direction est celle donnée par le vecteur $$V = S_w^{-1} (\mu_1 - \mu_2)$$
