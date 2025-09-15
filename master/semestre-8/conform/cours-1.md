---
layout: cours
usemathjax: true
suivant: 2
---

Sureté de fonctionnement : assurer que le fonctionnement est sécurisé (notamment pour l'humain). Lorqu'il y a une erreur il faut faire en sorte qu'il y ai le moins de dégats possible. Il faut mieux perdre du fonctionnement que de la sécurité. (exemple : les barrières des passages à niveau se ferment toutes seules en cas de panne électrique.)

Limites des méthodes de conception traditionnelles :
- coût des tests
- différentes interprétations de mots usuels (UML a résolu ce problème)
- les design patterns de POO sont bien mais les events ou le reactive programming sont plus complexe et il n'y a pas de "pattern
- ajouter des fonctionnalités sans régression est une tache difficile.

# Méthodes formelles

L'objectif est raisonner sur un logiciel ou système pour déterminer leur fonctionnement et controle. On se sert d'objets mathématiques pour cela.  
Le processus est de trouver un modèle formel pour le système, analyser le modèle avec les techniques formelles adéquates, puis transposer les résultats du modèle sur le vrai système.  
Mais est-ce certain que le modèle choisi correspond bien ? Et comment transposer les résultats ?

Pour qui : Pour des raisons économiques, le travail suplémentaire causé par les techniques formelles implique que seulement les concepteurs et/ou développeur de systèmes complexes ou critiques se servent des techniques formelles.

Un système est critiques si :
- La vie de personnes est lié à son fonctionnement. Comme pour les systèmes embarqué, les controlleurs, etc...
- Le coût économic est important en cas de faille.

Prouveur de théorèmes : Les Automated Theorem Prover (ATP) font des preuves mathématiques. Ils sont interactifs et aide l'utilisateur à construire une preuve. La vérification du matériel est l'utilisation principale des ATP. Malheureusement, ce n'est pas très adapté pour les systèmes réactifs.

L'interprétation abtraite est le transformation d'un problème concret en un modèle abstrait simple sur lequel la preuve serait plus facile (et toujours correcte).

Langages spécifiques au domaines : Un langage avec des primitives spécifiques au type d'application. Il est compilé dans un langage classique.

Vérification du modèle : Outils qui peuvent décider automatiquement si un modèle satisfait une propriété logique.

Rafinement et Implémentation : En un certain nombre d'étapes, transformer un une spécification en une implémentation en préservant à chaques étapes les propriétés essentielles du modèle. On prouve alors que les étapes conservent les modèle (et plus que le modèles est correct).

Génération de tests : Générer une séquence de test à partir des spécifications d'un modèle. Les tests sont relatif aux buts spécifiques. Leurs conformités sont utilisées pour valider un système réel. L'inter-opérabilité des tests est utilisé pour valider les systèmes réels dans un contexte ouvert.

Systèmes stochastiques : Les chaînes de Markov et les processus stochastiques ajoutent la probabilité d'évènements, pour calculer la probabilité qu'un évènemnts apparaisse, et quelles erreurs élémentaires sont impliquées dans un tel évènement. C'est un analyse de la sécurité et fiabilité du domaine.

Cycle de vie dans méthodes formelles : différentes méthodes formelles sont appliqué à chaque étapes de la conception et du développement d'un logiciel.

# AltaRica

Le langage a été défini en 97/99 par le LaBRI, LADS, Dassault, Renault, Total, Schneider et IPSN.  
Il y a plusieurs outils comme OCAS AltaRica, SIMFIA V2, arc, et mec 5.  
*Plus d'info sur [https://altarica.labri.fr](https://altarica.labri.fr).*

## Modèle de calcul

### Automate à contrainte :

- Un ensemble fini de variables d'état $$\overrightarrow{s}$$ (Variable qui décrit un état.)
- Un ensemble fini de variables de flux $$\overrightarrow{f}$$ (Variable qui découle d'un état. L'état produit la valeur de la variable.)
- Un ensemble fini d'évenements $$E$$
- Un ensemble de transitions $$G(\overrightarrow{s}, \overrightarrow{f})\ \ \overset{e}{\to}\ \ \overrightarrow{s} := \sigma(\overrightarrow{s}, \overrightarrow{f})$$.  
G est un garde (formule booléenne) et $$e \in E$$.  
*Si la garde est valide, alors on met à jours les variables d'état.*
- Une assertion (ou invariant) $$A(\overrightarrow{s}, \overrightarrow{f})$$. *L'invariant doit toujours être valide, cela met à jour les variables de flux.*
- Un ordre partiel sur $$E$$ pour définir les priorités

### Modularité et composition

- Une hiérarchie (arbre)
- La visibilité des composants
- Les feuilles :
	- les différences entre les variables d'état et de flux
	- les transitions gardées avec des post-condition
	- les assertions qui lient les variables d'état et de flux
- Interactions entre les composants :
	- Une assertion liant les variables d'état et de flux visible
	- Des vecteurs de synchronisation généralisés
	- Des priorités entre les évènements
- Bisimulation

## Les outils

ARC : The AltaRica Checher, ligne de commande  
altarica-studio : front-end graphique de ARC

Par défault, ces outils ajoutent automatiquement l'état "ne rien faire" :  
$$E := E \cup \{\epsilon\}$$  
$$G := G \cup \{\epsilon, true, \}$$  
Epsilon permet de visualiser le temps qui coule.

La commande `quot()` dans Graph permet d'avoir un graphe très simplifié (produit un gaphe quotient).  
La commande `modes()` dans Graph permet d'avoir un graphe avec seulement les variables d'état (les variables de flux sont enlevées).

# Définitions

### Validation

Un modèle est valide si ça ressemble à un système. Le modèle est un bon candidat pour être le vrai système.

Comment décider si un modèle est valide :
- Le modèle satisfait toutes les propriétés qui définissent un bon candidat
- Une liste minimale de propriétés dépendantes du type d'application
- La simulation est considérée come une bonne façon de validaer un systeme.

La validation assure que "la bonne chose soit construite".

### Vérification

Un modèle valide est vérifié si il satisfait toutes les exigences.

Comment décider si un modèle est vérifié :
- Le modèle doit satisfaire toutes les exigences décrites dans le document de spécifications.
- La simulation n'est pas considérée comme une bonne façon de vérifier un système critique
- Le model checking est considéré comme une bonne façon de vérifier un système critique

La vérification assure que "la chose soit construite correctement".

### Model checking

Principe :  
Le système est représenté par le modèle $$M$$. Les exigences sont décrites comme un propriété logique $$\phi$$.  
Un model-checker est un outil qui prend en entrée $$M$$ et $$\phi$$, et donne automatiquement un sortie $$M \models \phi$$ ou $$M \not \models \phi$$.

Propriété de sureté :
- quelque chose est impossible (la configuration est inateignable)
- après un évènement, un autre évènement est possible
- si une propriété de de sureté n'est pas satisfaite, le contre-exemple est un mot fini (une exécution finie du système)

Ces propriétés décrivent habituellement que quelque chose de grave ne se produit jamais.

Propriété de vivacité :
- quelque chose est toujours possible
- un évènement est toujours suivit par un autre dans un temps finis.
- si une propriété de vivactité n'est pas satisfaite, le contre-exemple est un mot infini (une exécution infinie du système)

Ces propriétés décrivent habituellement que quelque chose de bon continue de se produire.

Limites théoriques :  
$$M \models \phi$$ doit être décidable donc ($$M$$, $$\phi$$) doivent être dans des certaines calsses de modeles et formules.

Systèmes finis : Toutes les formues en logique comportementale sont décidables. Il y a de nombreux outils académiques

Systèmes infinis : Très peu de formules en logique comportementable sont décidables. Il y a beaucoup de résultats sur des systèmes infinis spécifiques (automates avec conteur, horloge, fifo, ...). Il y a beaucoup d'article scientifique, mais peu d'outils.

Limites pratiques :  
- On dépent de la mémoire de l'ordinateur quand on utilise une représentation explicite de l'accessibilité d'un graphe.
- On dépent de la vélocité de l'ordinateur quand on utilise quelques représentation implicites sur l'accessibilité d'un graphe.
- La difficulté de la modélisation.
- La difficulté d'être sûr que le modèle est valide.
- La difficulté d'écrire une propriété logique.

Formalisations pour décrire un modèle :
- réseaux de Petri
- graphes d'états
- graphes de séquence de message
- diagrammes de flots de données
- langages spécifiques

Logiques comportementales
- CTL (Computational Tree Logics) et sont extension CTL*
- LTL (Linear Time Logics). *La plus populaire.*
- HML (Hennessy-Milner Logics)
- $$\mu-$$calculus : une logique très chere
- logique de Dicky. Une logique basée sur une ensemble d'états et de transitions.

# Logique de Dicky

## Concept

Une propriété logique d'un automate peut être vue comme un ensemble d'entités satisfaisant la formule.  
La propriété peut être vérifiée en mettant dans marques lors d'un algorithme de parcours en profondeur sur l'accessibilité du graphe.

Les conjonction (resp. disjonctions) des propriétés logiques correspondent aux intersection (resp. unions) des ensembles.

Il y a deux types de propriétés sur un graphe $$G(V, E)$$ : les propriétés d'état ($$S \subseteq V$$) et les propriétés de transitions ($$T \subseteq V \times E \times V$$).

## Constantes pour les modèles AltaRica

Pour chaque automates :
- L'ensemble vide $$\emptyset$$ : `empty_s` et `empty_t`
- L'ensemble de tous les états : `any_s`
- L'ensemble de toutes les transitions : `any_t`
- L'ensemble de tous les états initiaux : `initial`
- L'ensemble de transitions : `self`, `epsilon`, `self_epsilon`

## Proposition élémentaires pour les modèles AltaRica

Pour une expression booléenne `Exp` sur des variables et un évènement `a` :
-  L'ensemble des états : `[Exp]`
-  L'ensemble des transitions : `label a`

## Opérations sur les ensembles

Si $$F_1, F_2$$ sont deux formules de même type :
- $$[[F_1 and F_2]] = [[F_1 \& F_2]] = [[F_1]] \cap [[F_2]]$$
- $$[[F_1 or F_2]] = [[F_1 \mid F_2]] = [[F_1]] \cup [[F_2]]$$
- $$[[F_1 - F_2]] = [[F_1]] \setminus [[F_2]]$$

Si $$F_1$$ une formule et $$T$$ une transition :
- $$[[not S]] = [[any\_s]] \setminus [[S]]$$
- $$[[not T]] = [[any\_t]] \setminus [[T]]$$

## src, tgt, rsrc, rtgt

*(source, target, reverse source, reverse target)*

$$S$$ une formule d'état, $$T$$ un formule de transition.

$$src(T)$$ est une formule d'état et $$[[src(T)]] = \{s \mid \exists (s, e, t) \in T\}$$. *états sources de la transition T.*

$$tgt(T)$$ est une formule d'état et $$[[tgt(T)]] = \{t \mid \exists (s, e, t) \in T\}$$. *états arrivée de la transition T.*

$$rsrc(S)$$ est une formule de transition et $$[[rsrc(S)]] = \{(s, e, t) \mid s \in S\}$$. *transitions ayant leurs sources en S.*

$$rtgt(S)$$ est une formule de transition et $$[[rsrc(S)]] = \{(s, e, t) \mid t \in S\}$$. *transitions ayant leurs arrivées en S.*

## reach, coreach

$$S$$ une formule d'état, $$T, T_1, T_2$$ des formules de transition.

$$reach(S, T)$$ est une formule d'état qui calcule les états accessibles depuis $$S$$ et utilisant seulement la transition $$T$$.

$$coreach(S, T)$$ est une formule d'état qui calcule les états depuis lesquels on prend seulement $$T$$ pour arriver à $$S$$.

## trace, project

$$trace(S_1, T, S_2)$$ permet de calculer un ensemble de transition représentant un des plus courts chemins de $$S_1$$ à $$S_2$$ passant seulement par les transitions $$T$$.

# Processus de création

## Le premier modèle

1. Identifier les composants élémentaires
2. Choisir entre un conception architecturalle ou fonctionnelle
3. Choisir entre un système ouvert ou fermé
4. Construire l'hiérarchie du système

## Validation du premier modèle

1. Topologie du la connexité du graphe (deadlock, SCC, ...)
2. Propriété dépendantes du système (pas de réactions infinies, ...)
3. Tous les évènements sont utiles
4. Visualisation des petits composants
5. Simulation
