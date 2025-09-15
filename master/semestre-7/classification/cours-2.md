---
layout: cours
usemathjax: true
precedent: 1
suivant: 3
---

# Méthodes d'optimisations

## Descente de gradient

Intuition : On cherche x tel que f(x) est le minimum. La méthode trivial est de trouver f'(x) = 0. Malheureusement, lorsque f(x) devient complexe, trouver x tel que ça dérivée vaut 0 devient compliqué.

La méthode est d'utiliser une descente de gradient. Le but est de descendre la courbe sans la remonter. (Un peu comme une bille qui tombe le long de la courbe). Le problème est qu'il n'y a pas de méthode pour être sûr d'être dans un minimal global et non dans un minimal local.

On prend un x aléatoire, on calcule la dérivée de f. Si elle est négative, on incrémente x, si elle est positive, on décrémente. X est incrémenté de $$\eta$$, le taux d'apprentissage. On continue jusqu'à ce que la dérivée soit nulle ou très petite.

Méthode du momentum : La balle accumule de la vitesse en descendant la courbe et si elle a assez de vitesse, elle peut remonter la courbe pour essayer de sortir d'un minimum local.

# Régression linéaire

La régression linéaire est un outil statistique pour étudier la présence de liens entre une variable dépendante $$Y$$ et des variables indépendantes $$X_1, X_2, ..., X_p$$

Un modèle de régression peut servir à répondre à un des trois objectifs suivants ;
- décrire une réalité
- confronter des hypothèses
- prédire

## Intuition

On cherche a évaluer l'effet des caractéristiques socio-démographiques (ancienneté, sexe, année étude après le bac, type d'emploi occupé) sur le salaire des employés

## Régression linéaire simple

**Données** : un échantillon de n paires indépendantes $$(x_i, y_i)$$ et identiquement distribués  
On cherche un modèle permettant de prédire les valeurs de Y en fonction des valeurs de X.

Hypothèse 1 : X et Y sont des grandeurs numériques mesurées sans erreur. X est une donnée dans le modèle, Y est aléatoire par l'intermédiaire de $$\epsilon$$. (la seule erreur que l'on a sur Y provient des insuffisances de X à expliquer ses valeurs dans le modèle)

Hypothèses 2 : Hypothèses sur le terme aléatoire. Les $$\epsilon_i$$ sont identiquement distribués :
- En moyenne les erreurs s'annulent
- La variance de l'erreur est constance et ne dépend pas de l'observation : homoscédasticité $$\mathbb{V}ar(\epsilon_i) = \sigma^2_\epsilon$$
- En particulier, l'erreur est indépendante de la variable exogène $$COV(x_i, \epsilon_i)) = 0$$
- Indépendance des erreurs, les erreurs relatives à 2 observations sont indépendantes.
- \\(\epsilon_i\tilde N(0, \epsilon)\\)

### Formulation

Modèle de régression simple :  
$$y-i = \beta_1 \times x_i + \beta_O + \epsilon_i, \forall i$$  
Ce que l'on peut écrire $$ Y = X\times \beta + \epsilon$$

L'objectif est de trouver des valeurs de $$\beta_0$$ et $$\beta_1$$ qui minimisent les écarts entre les valeurs réelles et les valeurs prédites.  
Plus formellement, il s'agit de minimiser la fonction $$S(\beta_0, \beta_1) = \sum_{i=1}^n \epsilon_i^2$$.

### Analyse des sources de variabilités

La variabilité des données se décompose en une partie expliquée par le modèle, et d'une partie résiduelle (terme d'erreur).  
SCT = SCR + SCE

SCT : somme des carrés des totaux (c'est la variance) : $$\sum_{i=1}^n (y_i - \bar{y})^2$$  
SCE : somme des carrés expliqués par le modèle : $$\sum_{i=1}^n (\hat{y}_i - \bar{y})^2$$  
SCR : somme des carrés résiduels, non expliqué par le modèle : : $$\sum_{i=1}^n (y_i - \hat{y}_i)^2$$

### Coefficient de détermination

Il est défini par : $$R^2 = \frac{SCE}{SCT} = 1-\frac{SCR}{SCT}$$

Il mesure la variabilité expliquée par le modèle de régression linéaire. $$R^2$$ est toujours compris en 0 et 1.  
S'il vaut 1, le modèle est excellent, et au contraire, si il vaut 0 le modèle ne sert à rien.

### Test de significativité globale du modèle

Statistique de test : $$F = \frac{SCE/1}{SCR/(n-2)} = \frac{R^2}{\frac{1-R^2}{n-2}} \equiv F(1, n-2)$$

Région critique au risque $$\alpha$$ : $$F > F_{1-\alpha}(1, n-2)$$

### Prévision et intervalle de prévision

#### Prévision ponctuelle

Pour un individu $$i^*$$, la prédiction ponctuelle s'écrit : $$\hat{y_i^*} = \hat{y}(x_{i^*})$$.  
L'erreur de prévision est alors $$\hat{\epsilon_{i^*}} = \hat{y_{i^*}} - y_{i^*}$$, ($$\hat{y_{i^*}}$$ prédit et $$y_{i^*}$$ réel),  
avec $$y_{i^*} = \beta_1x_{i*} + \beta_0 + \epsilon_{i^*}$$ et $$\hat{y_{i^*}} = \beta_1x_{i*} + \beta_0 + \epsilon_{i^*}$$

On démontre alors que :
- $$\mathbb{E}(\hat{\epsilon_{i^*}}) = 0$$
- $$\mathbb{V}arr(\hat{\epsilon_{i^*}}) = \sigma^2_{\hat{\epsilon_{i^*}}}$$

#### Prévision par intervalle

On sait que $$\epsilon \tilde N(0, \sigma_{\epsilon})$$.  
On en déduit un intervalle de confiance au niveau $$1-\alpha$$ pour la prévision :
$$\hat{y_{i^*}} \pm t_{1-\alpha/2} \times \hat{\sigma}_{\hat{\epsilon_{i^*}}}$$
