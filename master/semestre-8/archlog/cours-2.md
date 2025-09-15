---
layout: cours
precedent: 1
suivant: 3
usemermaid: true
---

# Problèmes de conception classiques

- Créer un objet en spécifiant explicitement une classe : Le programme est assujétit à une implémentation spécifique.
- Assujétissement à une opération particulière : Astreint à une forme unique de réponse à une méthode.
- Dépendance vis-à-vis de la plateforme matérielle et logicielle : Difficile à maintenir et à porter.
- Assujétissement de la représentation d'un objet ou à son code : Le programme est assujéttit à une implémentation spécifique.
- Assujétissement à un algorithme : Difficile de faire évoluer le programme.
- Couplage fort : Réutilisation difficile d'une sous partie de l'architecture.
- Extension des fonctionnalités par sous-classes : Multiplication des classes dans l'architecture.

# SOLID design

SOLID est un acronyme qui désigne cinq principes de conception destinés à produire des architectures logicielles plus compréhensible, flexibles et maintenables.

Ces concepts sont :
- Single responsability principle
- Open/closed principle
- Liskov substitution principle
- Interface segregation principle
- Dependency inversion principle

## Single responsability principle

Un classe ou une méthode doit avoir une et une seule responsabilité.

## Open/closed principle

Une entité applicative (classe, methode) doit être ouverte à l'extension, mais fermée à la modification.

## Liskov substitution principle

Une instance du type T doit pouvoir être remplacé par une instance de type G, tel que G sous-type de T, sans que cela ne modifie la cohérence du programme.

## Interface segregation principle

Il est préférable d'avoir plusieurs interfaces spécifiques pour chaque client plutôt qu'un seule interface générale.

## Dependency inversion principle

Il faut dépendre des abstractions, et non des implémentations.

# Patterns de conception

Un modèle décrit un problème que l'on rencontre régulièrement lors de l'élaboration d'un logiciel. Il apporte à ce problème une solution élégante, c'est-à-dire :
- flexible
- extensible
- réutilisable
- maintenable

Il existe trois groupes de pattern :
- Les modèles de création
- Les modèles de structure
- Les modèles de comportement

## Patterns de création

Les modèles de création permettent de modéliser le processus d'instanciation. Il sont particulèrement intéressants dans les architectures basées sur la composition d'objets.

- Abstract Factory
- Builder
- Method Factory
- Prototype
- SIngleton

### Abstract Factory

La fabrique abstraite fournit une interface pour la création de famille d'objets apparentés ou interdépendants, sans qu'il soit nécessaire de spécifier leur classes concrètes

Indication d'utilisation :
- Un système doit être indépendant de la façon dont ses produits on été créés, combinés et représentés.
- Un système doit être constitué à partir d'une famille de produits, parmi plusieurs.
- On souhaite renforcer le caractère de communauté d'un famille d'objet produit pour être utilisé ensemble.

Conséquence d'utilisation :
- Il isole les classes concrètes
- Il facilite la substitution de familles de produits
- Il favorise le maintien de la cohérence entre les objets
- Gérer de nouveaux produit est difficile (modification de toute l'architecture)

<div class="mermaid">
classDiagram
direction BT
class AbstractFactory {
	&lt;&lt;interface&gt;&gt;
	+createProductA()
	+createProductB()
}
class ConcreteFactory1 {
	+createProductA()
	+createProductB()
}
class ConcreteFactory2 {
	+createProductA()
	+createProductB()
}
ConcreteFactory1 --|> AbstractFactory
ConcreteFactory2 --|> AbstractFactory
ProductA1 --|> IProductA
ProductA2 --|> IProductA
ProductB1 --|> IProductB
ProductB2 --|> IProductB
ConcreteFactory2 ..> ProductA2
ConcreteFactory2 ..> ProductB2
ConcreteFactory1 ..> ProductA1
ConcreteFactory1 ..> ProductB1
</div>

### Builder

Dissocie la construction d'un objet complexe de sa représentation de sorte que le même processus de construction permette des représentations différentes.

Indication d'utilisation :
- L'algorithme de création d'un objet complexe doit être indépendant des parties qui composent l'objet et de la manière dont ces parties sont agencées
- Le processus de construction doit autoriser des représentations différentes de l'objet en construction.

Conséquence d'utilisation :
- Il permet de modifier le représentation interne d'un objet
- Il isole le code de construction et de représentation
- Il permet un meilleur contrôle du processus de construction

<div class="mermaid">
classDiagram
direction BT
class Builder {
	&lt;&lt;interface&gt;&gt;
	+addA()
	+addB()
	+build()
}
class ConcreteBuilder {
	+addA()
	+addB()
	+build()
}
ConcreteBuilder --|> Builder
ConcreteBuilder ..> Product
</div>

### Singleton

Garantit qu'une classe n'a qu'une seule instance et fournit un point d’accès de type global à cette classe.

Indication d'utilisation :
- S'il doit y avoir qu'une seule instance d'une classe, qui de plus doit être accessible aux clients en un point particulier.
- Si l'instance unique doit être extensible par dérivation en sous-classe et si l'utilisation d'une instance étendue doit être permise aux clients sans qu'ils aient besoin de modifier leur code

Conséquence d'utilisation :
- Accès contrôlé à une instance unique
- Réduction de l'espace de nom
- Raffinement des opérations et de la représentation
- Autorise un nombre variable d'instance
- Souplesse amélioré par rapport aux opérations de classes

<div class="mermaid">
classDiagram
class Singleton {
	+getInstance()$
	-instance$
	-Singleton()
}
</div>

### Prototype

Spécifie le type des objets à créer à partir d'une instance de prototype et crée de nouveaux objets en copiant ce prototype.

Indication d'utilisation :
- Le système doit être indépendant de la manière dont ses produits sont créés, composés, et représentés
- Si les classes à instancier sont spécifiées à l'exécution
- Pour éviter de créer une hiérarchie de classes de fabriques, qui réplique la hiérarchie de classes de produits.
- Si les instances d'une classe peuvent prendre un état parmi un petit nombre.

Conséquences d'utilisation :
- Addition et suppression de produit à l'exécution
- Spécification de nouveaux objets par valeur modifiables
- Spécification de nouveaux objets par structure modifiable
- Limitation du nombre de dérivation de classes
- Configuration dynamique d'unne application avec des classes

<div class="mermaid">
classDiagram
class Prototype {
	&lt;&lt;interface&gt;&gt;
	+clone()
}
</div>

### Method Factory

Définit une interface pour la création d'un objet, mais en laissant les sous-classes le choix des classes à instancier. La factory permet à une classe de déléguer l'instanciation à une sous-classe.

Indication d'utilisation :
- Une classe ne peut prévoir la classe des objets qu'elle aura à créer
- Une classe attend de ses sous-classes qu'elles spécifient les objets qu'elles créent
- Les classes délèguent des responsabilités à une des nombreuses sous-classes assistantes et l'on veut disposer localement de l'information permettant de connaître la sous-classe assistante qui a reçu cette délégation

Conséquence d'utilisation :
- Indépendance du code vis-à-vis des classes spécifiques de l'application
- Besoin d'hériter la classe factory à chaque fois que l'on veut faire un produit concret
- Il procure un gite aux sous-classes
- Il interconnecte des hiérarchies parallèles
