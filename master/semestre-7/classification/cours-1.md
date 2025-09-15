---
layout: cours
usemathjax: true
suivant: 2
---

# Introduction

> "The goal of machine learning is to build computer systems that can adapt and learn from their experience" - Tom Dietterich

Un programme informatique apprend par expérience E avec le respect de la classe de tâche T et une mesure de performance P, si la performance de la tâche T, mesurée par P, s'améliore sur l'expérience E.

Il faut bien différencier :
- la tâche adressée au système
- la performance mesurée pour évaluer le système 
- l’entraînement réalisé par le système

# Rappels d'algèbre linéaire

Les vecteurs et matrices sont nécessaires pour convertir des images/données en nombres sur lesquels ont pourra facilement appliquer des algorithmes.

## Notions de base

### Définition

**Vecteur** : $$(V_i)_{1<=i<=n}$$

**Matrice** : $$A = (a_{i,j})_{1<=i<=m; 1<=j<=n}$$

**Addition** : $$C = (c_{i,j})_{1<=i<=m; 1<=j<=n} = A + B = (a_{i,j} + b_{i,j})$$

**Multiplication** :
- deux vecteurs : $$x\times y = \sum^{n}_{i=1}x_i\times y_i$$
- matrice par vecteur : $$A\times x = \sum^{n}_{i=1} a_{j, i}\times x_i $$
- deux matrices : $$P = A\times B\ avec\ p_{i,j} = \sum^n_{k=1} a_{i,k} \times b_{k,j}$$

**Transposée** de $$A = (a_{i,j})$$ : $$A^T = (a_{j,i})$$

**Inverse** : Unique matrice carrée $$A^{-1}$$ telle que $$A \times A^{-1} = I$$ (la matrice identité)

**Trace** : La trace d'une matrice carrée est la somme de ses entrées en diagonale. $$tr(A) = \sum^n_{i=1} a_{i,i}$$

**Déterminant** : $$det(A) = \sum^n_{i=1} (-1)^{i+j} \times a_{i,j}$$

## Propriétés

### Norme

Une norme est une fonction : $$ N : V \to [0, +\infty [$$ où V est un espace vectoriel tel que :
- \\(\forall x,y \in V\\)
- \\(N(x+y) <= N(x) + N(y)\\)
- \\(N(ax) = \mid a\mid N(x)\\) pour tout scalaire a
- \\(N(x) = 0 => x = 0\\)

Exemple de normes :

$$L^1$$, Manhattan : $$\mid\mid x\mid\mid _1$$ $$\sum^{n}_{i=1} \mid x_i\mid$$

### Rang

Dépendance linéaire :  
Les vecteurs $$v_1, v_2, ..., v_n$$ sont dit dépendants s'il existe des scalaires $$\alpha_1v_1, \alpha_2v_2, ..., \alpha_nv_n$$ tels que :
- \\(\alpha_1v_1, \alpha_2v_2, ..., \alpha_nv_n = 0\\)
- \\(\exists i \in \{1,2,...,n\}\\) tel que \\(\alpha_I \neq 0\\)

Sinon, les vecteurs $$v_1, v_2, ..., v_n$$ sont dit indépendants.

Rang d'une matrice :  
Le rang d'une matrice A, noté $$rang(A)$$ est la dimension de l'espace vectoriel généré par ses colonnes. Ceci est équivalent au nombre maximum de colonnes indépendantes de 1.

## Valeurs propres, Vecteurs propres

Définition :  
$$A\in R^{n\times n}, \lambda$$ est une valeur propre de A s'il existe un vecteur $$v \in R^*$$ appelé propre, tel que : $$Av = \lambda v$$

## Opérations avancées

### Gradient
$$f : \mathbb{R}^{m\times n} \to \mathbb{R}$$ une fonction  
$$A \in \mathbb{R}^{m\times n}$$ une matrice

Le gradient de f par rapport à A est une matrice de $$R^{m\times n}$$, notée $$\nabla_A f(A)$$ définie par :  
$$(\nabla_A f(A))_{i,j} = \frac{\partial f(A)}{\partial a_{i,j}} \forall(i,j) \in \{1,2,...,m\} \times \{1,2,..., n\}$$

### Hessienne
$$f : \mathbb{R}^{m\times n} \to \mathbb{R}$$ une fonction  
$$x \in \mathbb{R}^{n}$$ une matrice

La hésienne de f par rapport à x est une matrice symétrique de $$\mathbb{R}^{n\times m}$$ notée $$\partial^2_x f(x)$$, définie par :  
$$(\partial^2_x f(x))_{i,j} = \frac{\partial^2 f(x)}{\partial x_i \partial x_j}, \forall i,j \in \{1,2,...,n\}$$

# Rappels de probabilités

## Distribution uniforme

Tous les éléments de $$\Omega$$ sont de même probabilité.

Un *espace probabilisé discret* est un couple $$(\Omega, \mathbb{P} r)$$, où $$\Omega$$ est un ensemble non vide au plus dénombrable et $$\mathbb{P} r$$ une application de $$\Omega$$ dans [0,1] telle que :  
$$\sum_{\omega\in\Omega} \mathbb{P} r(\omega) = 1$$

## Probabilité conditionnelles

Soit $$(\Omega,\mathbb{P} r)$$ unn espace probabilisé discret et soit A un événement de probabilité non nulle. On appelle $$\mathbb{P} r(B/A)$$ probabilité conditionnelle de B sachant A telle que :  
$$\mathbb{P} r(B/A) = \frac{\mathbb{P} r(A\cap B)}{\mathbb{P} r(A)}, \forall B \in P(\Omega)$$

Conditionnement par A : $$Pr(A\cap B) = Pr(B/A)Pr(a)$$

Loi des probabilités totales :  
Soit $$A_1, ..., A_n$$ une partition de $$\Omega$$. Si chacun des ensemble de probabilité est non nul alors : $$ \mathbb{P} r(B) = \sum^{n}_{i=1} \mathbb{P} r(A_i)\mathbb{P} r(B/A_i), \forall B \in P(\Omega)$$

## Indépendance

deux événements sont indépendant si : $$\mathbb{P} r(A\cap B) = \mathbb{P} r(A)\mathbb{P} r(B)$$

## Théorème de Bayes

Soit l'évènement B causé par l'un des évènements $$A_1, A_2, ..., A_n$$, tous de probabilité non nulle.  
On connaît les probabilités $$\mathbb{P} r(A_i)$$ ainsi que $$\mathbb{P} r(B/A_i).

On peut alors calculer la probabilité $$\mathbb{P} r(A_i/B)$$ :  
$$\mathbb{P} r(A_i/B) = \frac{\mathbb{P} r(A_i)\mathbb{P} r(B/A_i)}{\sum^{n}_{j=1} \mathbb{P} r(A_j)\mathbb{P} r(B/A_j)}$$

En particulier, pour deux évènements A et B :  
$$\mathbb{P} r(A/B) = \frac{\mathbb{P} r(A)\mathbb{P} r(B/A)}{\mathbb{P} r(B)}$$

## Variables aléatoires

Soit $$(\Omega,\mathbb{P} r)$$ un espace probabilisé discret et soit $$\Omega'$$ un ensemble non vide dénombrable.  
Une variable aléatoire X à valeur dans $$\Omega'$$ est une application de $$\Omega$$ dans $$\Omega'$$  
On pourra munir $$\Omega'$$ d'une loi de probabilité $$\mathbb{P} r_X$$ en posant : $$\mathbb{P} r_X(\omega') = \mathbb{P} r(X^-1(\{\omega'\})) \forall \omega' \in \Omega'$$

### Fonction de répartition

Soit $$\mathbb{P}r$$ une mesure de probabilité. Sa fonction de répartition F est définie par : $$F(t) = \mathbb{P}r(]-\infty, t]), \forall t \in \mathbb{R}$$

S'il existe une fonction f telle que :  $$F(t) = \int_{-\infty}^{t} f(x)dx, \forall t \in \mathbb{R}$$
Alors cette fonction s'appelle densité de $$\mathbb{P}r$$.

La fonction de répartition étant croissante, si la densité f existe, son intégrale de $$-\infty$$ à $$t$$ doit valoir $$F(t)$$. Donc on peut définir la densité f comme la dérivée de la fonction de répartiton, si cette dernière en admet une.

|Cas|CDF|PDF|Propriétés|
|:---:|:---:|:---:|:---:|
|Discret|$$F(t) = \sum_{x_i\leq t}\mathbb{P}r(X=x_i)$$|$$f(x_i) = \mathbb{P}r(X=x_i)$$|$$0\leq f(x_i)\leq 1$$ et $$\sum_i f(x_i)=1$$|
|Continu|$$F(t) = \int_{-\infty}^{t}f(x)dx$$|$$f(x)=\frac{dF}{dx}$$|$$f(x)\geq 0$$ et $$\int_{-\infty}^{+\infty} f(x)dx = 1$$|

### Expérance

L'espérance d'une variable aléatoire est définie comme la somme des valeurs prises pondérées par les probabilités respectives.

|Cas|$$\mathbb{E}(X)$$|$$\mathbb{E}(g(X))$$|
|:---:|:---:|:---:|
|Discret|$$\sum_{i=1}^{n}x_i\mathbb{P}r(X=x_i)$$|$$\sum_{i=1}^{n}g(x_i)\mathbb{P}r(X=x_i)$$|
|Continu|$$\int_{-\infty}^{+\infty} xf(x)dx$$|$$\int_{-\infty}^{+\infty} g(x)f(x)dx$$|

**Linéarité de l'espérance** :  
Soit $$X_1$$ et $$X_2$$ deux variables aléatoires définies sur le même espace probabilisé discret ($$\Omega, \mathbb{P}r$$) et admettant toutes deux une espérance. Soit $$\alpha \in \mathbb{R}$$. Alors :  
$$\mathbb{E}(\alpha X_1) = \alpha\mathbb{E}(X_1)$$  
$$\mathbb{E}(X_1 + X_2) = \mathbb{E}(X_1) + \mathbb{E}(X_2)$$

**Distributivité conjointe** :  
Soient $$X$$ et $$Y$$ deux variables aléatoires définies sur ($$\Omega, \mathbb{P}r$$).  
$$\mathbb{P}r_{XY}(X=x,Y=y)=\mathbb{P}r(\{\omega\in\Omega\mid X(\omega)=x,Y(\omega)=y\})$$

$$X$$ et $$Y$$ sont indépendantes si : $$\mathbb{P}r_{XY}(X=x,Y=y)=\mathbb{P}r_X(X=x) \mathbb{P}r_Y(Y=y)$$

### Variance

Un paramètre important qui vient tout de suite après l'espérance est la variance. Elle mesure la dispersion d'une variable aléatoire. Si X est une v.a. définie sur $$(\Omega, \mathbb{P}r)$$, sa variance est définie par : $$\mathbb{V}ar(X) = \mathbb{E}((X - \mathbb{E}(X))^2)$$

La racine carré de la variance est appelé **écart-type** : $$\sigma(X)=\sqrt{\mathbb{V}ar(X)}$$

### Distributions importantes

|Type|Distribution|PDF|$$\mathbb{E}(X)$$|$$\mathbb{V}ar(X)$$|
|:---:|:---:|:---:|:---:|:---:|
|Discret|$$X\sim B(n,p)$$|$$\mathbb{P}r(X=k)=\binom{n}{k}p^kq^{n-k}$$|$$np$$|$$npq$$|
|Discret|$$X\sim P(\lambda)$$|$$\mathbb{P}r(X=k)=\frac{\lambda^k}{k!}e^{-\lambda}$$|$$\lambda$$|$$\lambda$$|
|Continu|$$X\sim U(a,b)$$|$$f(x)=\frac{1}{b-a}$$|$$\frac{a+b}{2}$$|$$\frac{(b-a)^2}{12}$$|
|Continu|$$X\sim E(\lambda)$$|$$f(x)=\lambda e^{-\lambda x}$$|$$\frac{1}{\lambda}$$|$$\frac{1}{\lambda^2}$$|
|Continu|$$X\sim N(\mu, \sigma)$$|$$f(x)=\frac{1}{\sqrt{2\pi\sigma}}e^{-\frac{1}{2}(\frac{x-\mu}{\sigma})^2}$$|$$\mu$$|$$\sigma$$|
