---
layout: cours
precedent: 3
---

# Design patterns

Un solution précise à un problème donné, dans un contexte précis.

Ils servent à décrire les relations entre les objets du modèles et les conséquences liées à leur utilisation.

Les design patterns sont des architextures de classes permettant d'apporter une solution à des problème fréquemment rencontrés lors de l'analyse et de la conception d'applications.  
Ces solutions sont facilement adaptables et réutilisables, et ont une architecture facilement compréhensible et identifiable.

Il existe 23 design patterns, regroupé en 3 groupes :

**Création** :
- Abstract Factory
- Factory
- Builder
- Prototype
- Singleton

**Structuraction** :
- Adapter, Bridge
- Composite
- Facade, Proxy
- Flyweight
- Decorator

**Comportement** :
- Interpreter, Command
- Chain of responsability
- Iterator, Mediator, Template
- Memento, Observer
- State, Strategy, Visitor

## Design patterns de création

### Singleton

Un singleton est un objet d'une classe dont on a la garantie qu'il n'existe qu'une seule instance en mémoire.  

Exemple :
```java
public final class Singleton {
	private static final Singleton INSTANCE = new Singleton();
	// private constructor to disable instanciation
	private Singleton() {}
	public static Singleton getInstance() { return INSTANCE; }
}
```

### Prototype

Un prototype est une classe qui propose elle-même une façon de se copier. Un utilisateur n'a pas à faire la copie lui-même de l'objet, celui-ci nous offre une méthode pour le faire à notre place. Un prototype aura une methode du style `clone()` ou `copy()`.

### Builder

Un builder sert à séparer la construction d'un objet complexe en plusiers éléments qui le compose. L'interface déclare les étapes communes pour la construction de l'objet. Le code du constructeur de l'objet est délégué au builder.

Exemple : `java.lang.StringBuilder`

## Design patterns structurels

### Decorator

Un décorateur sert a affecter dynamiquement de nouveaux attributs ou comportements à des objets en les placant dans des wrapper qui implémenttent ces caractéristiques.

Exemple :  
Les types primitifs et leur wrapper classes. Le type `int` à une classe qui permet de le décorer : `java.lang.Integer`. Cette classe ajoute de nouveaux traitements au type `int` et le décore.

### Proxy

Un proxy sert à substituer un objet par un autre pour le controler. Il redirige ses appels vers les appels de la classe originale.  
Ainsi la classe originale peut changer sans que l'utilisateur du proxy ne s'en rende compte, et sans modification de code.

Exemple :
```java
public interface Image { void display(); }

public class RealImage implement Image {
	@Override public void display() { /* do something */ }
}

public class ImageProxy implement Image {
	private Image delegate;
	public ImageProxy() { this.delegate = new RealImage(); }
	@Override public void display() { this.delegate.display(); }
}

public class User {
	public static void main(String[] args) {
		Image image = new ImageProxy();
		image.display();
	}
}
```

### Composite

Un composite permet la composition d'objets de façon transparente. Une composition est vue comme un objet simple par le client.

Exemple :
```java
public interface Node {
	void print();
}

public class SimpleNode implement Node {
	@Override void print() { System.out.println("SimpleNode"); }
}

public class CompositeNode implement Node {
	private List<Node> nodes = new ArrayList<>();
	@Override void print() { nodes.forEach(System.out::println); }
	// add/remove elements to the composition
	public void addNode(Node node) { nodes.add(node); }
	public void removeNode(int index) { nodes.remove(index); }
}

public class User {
	public static void main(String[] args) {
		CompositeNode node = new CompositeNode();
		node.addNode(new SimpleNode());
		CompositeNode n = new CompositeNode();
		n.addNode(new SimpleNode());
		node.addNode(n);
		node.print(); // will print all the composition
	}
}
```

### Observer

Ce design pattern permet de créer des dépendances entre objets (qui ne sont pas dans la même hiérarchie) afin que lorsque l'un d'eux change d'état, l'autre soit informé. Un observé notifiera tous ses observateur lorsqu'il c'est produit un changement.

Exemple :
```java
public interface WeatherObserver { void notify(WeatherType weather); }
public class ObserverA implement WeatherObserver {
	@Override public void notify(WeatherType weather) { /* do something */ }
}
public class ObserverB implement WeatherObserver {
	@Override public void notify(WeatherType weather) { /* do something else */ }
}

public enum WeatherType{ SUNNY, RAINY, WINDY, COLD; }

public class Weather {
	private WeatherType currentWeather = WeatherType.SUNNY;
	private List<WeatherObserver> observers = new ArrayList<>();

	public void addObserver(WeatherObserver observer) { observesr.add(observer); }
	public void notifyObservers() { observers.forEach(observer -> observer.notify(currentWeather)); }
	public void changeWeather(WeatherType weather) {
		this.currentWeather = weather;
		// observers will be notified when the weather is changed
		this.notifyObservers();
	}
}

public class User {
	public static void main(String[] args) {
		WeatherObserver obsA = new ObserverA();
		WeatherObserver obsB = new ObserverB();
		Weather weather = new Weather();
		weather.addObserver(obsA);
		weather.addObserver(obsB);
		weather.changeWeather(WeatherType.WINDY);
		weather.changeWeather(WeatherType.RAINY);
		weather.changeWeather(WeatherType.COLD);
	}
}
```

### Abstract Factory

Ce design pattern sert à encapsuler un groupe de factory. Cela permet de créer une famille d'objets apparentés sans préciser la classe concrètre.

Un utilisateur n'aura qu'une interface pour communiquer avec les factory (par le biais de l'abstract factory) sans connaître leur réelle implémentation.

### Adapter

Un adapter sert à faire communiquer des objets ayant des interfaces incompatibles. L'adapteur se chargera de traduire les communications

Exemple : Un objet qui lit des données xml et un autre qui produit des données json. Un adapter sera nécessaire pour que les deux objets puissent communiquer sans être modifiés.

### Facade

Une facade est une interface simple d'un sous-système complexe. L'utilisateur à une liste de choix simple au lieu de plein de choix plus complexe.

Exemple : Plusieurs classes pour convertir un format de vidéo vers un autre. Une facade pourrait avoir comme interface une méthode qui demande la vidéo et le format de sortie. L'utilisateur a ainsi un choix simple à faire.

## Design pattern comportemental

### Visitor

Cela permet de séparer un algorithme de la structure de données. On peut ajouter des traitements sans modifier la structure.  
Cela permet de mettre en oeuvre le principe open/closed (ouvert aux extensions, fermé aux modifications)

Le comportement concerné est placé dans une classe séparée, le visiteur. Celui-ci a un comportement différent selon la classe visitée.

Exemple :
```java
public class CarVisitor {
	public void visit(Wheel wheel) { /* do something */ }
	public void visit(Engine engine) { /* do something */ }
	public void visit(Body body) { /* do something */ }
	public void visit(Car car) {
		for(CarElement element : car.getElements()) { element.accept(this); }
	}
}

public abstract class CarElement {
	void accept(CarVisitor visitor) { visitor.accept(this); }
}
public class Wheel implement CarElement {}
public class Engine implement CarElement {}
public class Body implement CarElement {}
public class Car {
	private CarElement[] elements;
	public Car() {
		this.elements = new CarElement[] { new Wheel(), new Engine(), new Body() }
	}
}

public class User {
	public static void main(String[] args) {
		Car car = new Car();
		CarVisitor visitor = new CarVisitor();
		
		visitor.visit(car);
	}
}
```


### Strategy

Ce design pattern propose une interface qui a différentes stratégies pour un problème données. Ainsi l'utilisateur qui veut régler son problème n'a qu'à choisir une stratégie pour le résoudre. Il n'a pas besoin de savoir les algorithmes utilisés.  
Ce pattern permet de changer les algorithmes très facilement.

Exemple : Un gps permet d'aller d'un point a à un point b en choisissant la stratégie : à pied, en vélo, en voiture, etc. L'utilisateur change d'algorithme sans les connaîtres.
