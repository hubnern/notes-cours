---
layout: cours
precedent: 1
suivant: 3
---

## Static

Le keyword `static` fait d'un attribut un attribut de classe et plus d'instance. L'accès à cet attribut ce fait par la classe et non pas par une instance de la classe.

Il est aussi possible d'avoir des méthodes statiques. Elles ont les même comportements qu'un attribut statique. De plus, les méthodes statiques ne peuvent utiliser que des méthodes et attributs statiques (il n'est pas possible d'utiliser une méthode d'instance si on a pas d'instance de cet objet).

Il est aussi possible d'avoir des classes statiques, mais seulement les inner-classes peuvent être statiques. Une inner class statique agira comme si elle est dans son propre fichier, le fait qu'elle soit dans une classe est "annulé".

## Final

Le keyword `final` permet de spécifier la non-modification de valeurs.

Un attribut final ne peux pas être re-instancié.

Une méthode finale ne peux pas être redéfinie.

Une classe finale ne peux pas être héritée.

```java
public final class Car {
	public final String name;

	public Car(String name) {
		this.name = name; // final attribute can be instanced only in constructor or at definition.
	}

	public final void stop() {
		System.out.println("Stopping the car");
	}
	
	public void methodeA() {
		this.name = "abcd"; // error at compile-time. Can't redefine final attribute.
	}
}
public class Plane extends Car { // error at compile-time. Can't extends final classes.

	@Override
	public void stop() { // error at compile-time. Can't override final methods.
		System.out.println("Stopping the plane");
	}
	
}
```

## Super

Permet d'appeler les méthodes d'une classe parente.  
`super()` dans un constructeur d'une classe fille appelle le constructeur parent. Si cet appel est utilisé, il doit forcément être à la première ligne dans le constructeur fils.  
`super.method()` appelle la méthode `method()` de la classe parente.

## Abstract

Ce keyword permet de définir une classe comme abstraite. Une classe abstraite ne peut pas être instancié. IL est impossible de faire `new AbstractClass();`. Cette classe devra être héritée pour pouvoir être instancié.

Il est aussi possible d'avoir des méthodes abstraite. Ces méthodes doivent être redéfinies pour pouvoir être utilisé. Le compilateur lancera une erreur si une méthode abstraite non redéfinie est appelée.

# Interfaces

Une interface permet de fournir un contrat à une classe. Permet de partager le comportement entre plusieurs classes.

Une interface est sensé définir seulement les signatures des méthodes. Une interface et ses méthodes sont forcement publiques (elles sont déclaré implicitement publiques).  
Une interface ne peut pas avoir d'attributs.

Il est possible de définir le corps d'une méthode dans une interface avec le keyword `default`. Cela permet de définir un comportement par défault.

# Exception

En Java les erreurs sont produite sous formes d'exceptions. Les exception sont lancées par de méthodes avec `throw new ExceptionClass()`.  
Les exceptions peuvent être rattrapées dans un bloc `try{}catch{}`.  

**Error** : Ces erreurs sont considéré comme graves et ne sont pas sensé être rattrapées. Elles empêchent le bon fonctionnement de la JVM. Ce sont les erreurs qui héritent de `java.lang.Error`. Exemple : `java.lang.OutOfMemoryError`.

**Checked Exception** : Ces exceptions doivent être déclarées dans la signature de la méthode ou doivent être rattrapées. Ce sont les exceptions qui héritent de la classe `java.lang.Exception`. Exemple : `java.io.IOException`.

**Unchecked Exception** (runtime exception) : Ces exceptions sont implicites. Il n'est pas nécessaires de la rattraper ou de la déclarer dans la signature. Ce sont les exceptions qui héritent de la classe `java.lang.RuntimeException`. Exemple : `java.lang.ArrayIndexOutOfBoundsException`.
