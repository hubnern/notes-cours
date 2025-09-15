---
layout: default
---

# Decorator

Le but de ce TD est de comprendre le fonctionnement du design pattern Decorator, et savoir comment l'utiliser.

**Problème** : Nous avons une armée de soldat, mais ceux-ci ne sont pas assez puissant pour gagner la guerre. Nous souhaitons améliorer ces soldats pour gagner.  
**Contrainte** : Il est interdit de modifier ces soldats.

Concrètement, nous avons une interface `Soldier` qui définie ce que doit faire un soldat, et un classe d'un soldat de base `NoWeaponSoldier` que nous souhaitons améliorer.

```java
public interface Soldier {
	int getStrength();

	/**
	 * Hit a soldier with a strength.
	 * @param incomingStrength the strength to hit the soldier
	 * @return the health points of the soldier after being hit.
	 */
	int hit(int incomingStrength);
}

public class NoWeaponSoldier implements Soldier {
	private int health;

	public NoWeapondSoldier(int initialHealth) { this.health = initialHealth; }

	@Override public int getStrength() { return 0; }

	@Override public int hit(int incomingStrength) {
		this.health -= incomingStrength;
		if (this.health < 0) {
			this.health = 0;
		}
		return this.health;
	}
}
```

Pour améliorer nos soldats, nous décidons de leur donner des armes.

Comme il est interdit de modifier la classe `NoWeaponSoldier` pour modifier leur force ou leur ajouter un attribut, nous allons décorer un soldat sans arme avec une arme.

- Ajouter un décorateur qui viendra ajouter une épée de force 3 à un soldat.

Maintenant que nos soldats sont plus puissant, nous voulons en rendre certains plus résistant aux dégâts.

Encore une fois, il est interdit de toucher aux soldats sans arme, mais aussi aux soldats avec épée.

- Ajouter un décorateur qui viendra ajouter un bouclier de défense 2 à un soldat.

Nous avons maintenant une plutôt bonne armée. Cependant, nous pensons qu'il est possible de l'améliorer encore. Nous aimerions que les soldats se servent de leur deux mains et aient une épée et un bouclier en même temps.

- Est-ce que le pattern Decorator permet à nos soldats d'avoir deux armes en même temps ?
