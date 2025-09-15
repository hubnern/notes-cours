---
layout: cours
usemathjax: true
precedent: 4
suivant: 6
---

## Malédiction de la dimensionnalité

Plus il y a de dimension, plus la classification est compliquée.

**Complexité :**

La complexité augmente en fonction de la dimension d.

> Exemple : On veut estimer la matrice de covariance, la complexité en temps est alors de O(nd²)

Si on veut la même densité que pour une dimension 1 dans une dimension 3, il faut agmenter la quantité de données. Avec $$x$$ la quantité dans $$d=1$$, il faudrait $$x^3$$ données pour $$d=3$$, $$x^k$$ pour $$d=k$$.

## Analyse en composantes principales

Transformer des variables très corrélées en nouvelles variables décorrélées.  
L'ACP préserve la plus grande variance des données. Il s'agit de résumer l'information qui est contenue dans un dataset en un certain nombre de variables synthétiques : les **composantes principales**.

L'ACP est utilisé pour la compression de données, visualisation des données, ou accélération de l'apprentissage.

**Objectif :**  
Données d'origine $$X=(X_1, ...X_n)$$.  
Les variables $$X_i$$ sont fortement corrélées.

Ce que l'on cherche : $$Y = (Y_1, ...Y_q)$$ tel que :
- $$Y_j = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + ... + \beta_p X_p$$,
- les variables $$Y_j$$ sont indépendantes,
- avec une perte d'information la plus petite possible

**Rappel :**

V un espace de dimension d, W un sous espace de V de dimension k.

On peut toujours trouver k vecteurs de dimension , $$\{e_1, e_2, ...e_k\}$$ formant une base orthonormée de W, i.e., $$<e_i, e_j> = 0$$ pour tout $$i \neq j$$ et $$<e_i, e_i> = 1$$.

Tout vecteur u de W peut s'écricre $$u = \sum_{i=1}^{k} \alpha_i e_i$$.

**Théorème de l'ACP :**

Soit $$\{e_1, e_2, ..., e_k\}$$ la base orthonormée de W. Chaque vecteur u dans W peut être écrit en fonction de la base : $$\sum_{i=1}^{k} \alpha_i e_i$$.  
$$z_j$$ sera représenté par un vecteur dans W : $$z_j = \sum_{i=1}^{k} \alpha_{ji} e_i$$.  
L'erreur, pour $$z_j$$ peut alors être représentée par : $$erreur_j = \left \| z_j - \sum_{i=1}^{k} \alpha_{ji} e_i \right \|^2$$.  
L'erreur totale pour le dataset S : $$J(e_1, e_2, ..., e_k, \alpha_{11}, \alpha_{12}, ..., \alpha_{nk}) = \sum_{j=1}^{n} \left \| z_j - \sum_{i=1}^{k} \alpha_{ji} e_i \right \|^2$$

L'objectif de l'ACP est de trouver les bon paramètres $$e_1, e_2, ..., e_k$$ et $$\alpha_{11}, \alpha_{12}, ..., \alpha_{nk}$$ tels que $$J$$ soit minimale.

$$J$$ est minimale si on prend comme base, pour W, les k vecteurs propres associés aux k plus grandes valeurs propre de la "scatter matrix" $$S = (n - 1) \hat{\sum}$$. $$\hat{\sum}$$ étant la matrice de covariance

### Résumé

Données : $$S = \{x_1, x_2, ..., x_n\}$$.  
Chaque $$x_i$$ est un vecteur de dimension $$d$$.  
Objectif : réduire la dimension de $$d$$ à $$k$$.
1. Calculer la moyenne de l'échatillon $$\overline{x} = \frac{1}{n} \sum_{i=1}^n x_i$$
2. Centrer les données : $$z_j = x_j - \overline{x}$$
3. Calculer la "scatter matrix" : $$S = \sum_{j=1}^n z_j z_j^t$$
4. Calculer les vecteurs propres $$e_1, e_2, ..., e_k$$ associés aux $$k$$ plus grandes valeurs propres de $$S$$
5. Soit $$E = (e_1, e_2, ..., e_k)$$
6. Le vecteur recherché est $$y = E^t z$$

### Limites

L'ACP est conçue pour représenter des données et non pour les classifier
- elle préserve la variance autant que possible
- si les directions à plus grandes variances coincident avec les bons choix pour la classification alors elle peut servir

En général la directioin conservant la plus grande variance peut ne pas être avantageuse