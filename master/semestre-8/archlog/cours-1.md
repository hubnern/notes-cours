---
layout: cours
usemermaid: true
suivant: 2
---

<!--
https://mermaid-js.github.io/mermaid/#/classDiagram?id=syntax
-->

> Sources bibliographiques :
> - Modélisation et conception orientées objets avec UML 2.0 - Michael Blaha & James Rumbaugh
> - Design patterns. Catalogue des modèles de conception réutilisables. - Eric Gamma, Richard Helm, Ralph Johnson, John Vissides

# Rappel et introduction UML

## Caractéristiques de l'approche objet

- **L'identité** : Les données sont organisées en entités discrètes et distinguables nommées objets.
- **La classification** : Deux objets possédant la même structure de données et le même comportement sont des représentants d'une même classe.
- **L'héritage** : Le partage des attributs et des opérations entre les classes sur la base d'une relation hiérarchique. Une super classe possède des informations générales que les sous classes spécialisent et décrivent en détail.
- **Le polymorphisme** : Les mêmes opérations peuvent se comporter différement dans des classes différentes.

## Modélisation vs Implémentation

L'objectif de la modélisation est de se détacher des langages de programmation afin d'élaborer le logiciel à un niveau plus abstrait.

## Modèles de la modélisation objet

**Modèles de classe** : décrit la structure statique des objets d'un système et de leur relations. Un diagramme de classe est un graphe dont les noeuds sont des classes et les arcs des relations entre ces classes.

**Modèle d'états** : décrit les états successifs d'un objet au cours du temps. Un diagramme d'état est un gaphe dont les sommets sont des états et les arcs des transitions entre les états.

**Modèles d'intéractions** : décrit la façon dont les objets d'un système coopèrent pour obtenir un résultat. Il commence par les cas d'utilisation qui sont détaillés par les diagrammes de séquence et des diagrammes d'activités. Un cas d'utilisation est axé sur une fonctionnalité.

## Modélisation des classes et objets en UML 2.0

Un modèle de classe capture la structure statique d'un système

<div class="mermaid">
classDiagram
class Name {
	+type publicField = defaultValue
	-type privateField
	#type protectedField
	~type packageField
	+type staticField$
	+method() type
	+method(arg1, arg2) type
	+staticMethod()$ type
	+abstractMethod()* type
}

A <|-- B : héritage
C <|.. D : implémentation

Card "0..32" -- "1" Deck
</div>

## Concept de liens et d'associations

Un lien est une connexion physique ou conceptuelle entre deux objets.  
Une association est une description d'un groupe de liens qui partagent une structure et une sémantique commune.

### Multiplicité des associations

- `1` : Exactement un
- `*` : Plusieurs (zéro ou plus)
- `0..1` : Optionnel (zéro ou un)
- `1..*` : Un ou plus

### Ordonnancement, bags, et séquences

Par défaut une association représente un ensemble de liens. Deux objets ne peuvent donc être relié deux fois via la même association. On peut indiquer trois autres types d'association.

- Rien : ensemble de lien
- Ordered : ensemble ordonné
- Bags : tableau/liste, plusieurs liens possibles
- Sequence : plusieurs liens ordonnés

### Classe d'association

Une classe d'association est une classe qui modélise une association entre deux classes.

### Agrégation

L'agrégation est une sorte d'association dans laquelle un objet agrégat est constitué de constituants. Elle permet de modéliser la relation "Tout ou partie", il y a une agrégation si un des points suivants est vrai :
- Peut-on utiliser l'expression fait partie de
- Les opérations appliquées à l'objet constitué s'appliquent-elles automatiquement à ses constituants
- Les valeurs des attributs constitué se propagent-elles à tous ses constituants ou à certains d'entre eux
- L'association présente-t-elle une asymétrie intrinsèque, dans laquelle une classe est subordonnée à une autre

### Composition

La composition est une forme restrictive de l'agrégation :
- Une partie constituante ne peut pas appartenir à plus d'un assemblage
- La durée de vie d'une partie constituante est celle du constituant

<div class="mermaid">
classDiagram
Agregat *-- Agregé
Composition o-- Composé
</div>

## Modélisation des interactions

### Cas d'utilisation

Au niveau le plus élevé, les cas d'utilisations décrivent comment un système interagit avec les acteurs extérieurs. Chaque cas d’utilisation représente une partie des fonctionnalités que le système fournit à ces utilisateurs.

Acteurs : est un utilisateur externe direct du système. Un objet ou un ensemble d’objet qui communique directement avec le système.

Cas d’utilisation : est une partie cohérente des fonctionnalités qu’un système peut fournir en interagissant avec les acteurs.

Comment le dessiner :
- Un rectangle contient les cas d’utilisation du système.
- Un nom dans une ellipse représente un cas d’utilisation. L’icône d’un bonhomme représente un acteur
- Des lignes connectent les cas d’utilisation aux acteurs qui y participent

### Diagramme de séquence

Fournissent plus de détail et représentent les messages que s’échange un ensemble d’objets au fil du temps. Les messages comprennent à la fois les signaux asynchrones et les appels de procédures.
- Objets actifs  
- Objets passifs  
- Objets temporaires

<div class="mermaid">
sequenceDiagram
	Alice->>+John: Hello John, how are you?
	Alice->>John: John, can you hear me?
	John-->>Alice: Hi Alice, I can hear you!
	John->>+Brain: How am I feeling?
	Brain-->>-John: You feel great.
	John-->>-Alice: I feel great!
</div>
