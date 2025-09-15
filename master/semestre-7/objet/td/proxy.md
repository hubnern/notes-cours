---
layout: default
---

# Proxy

Ce td se base sur celui des décorateurs.  
Nous avons un armée de plusieurs soldats, ces soldats sont soit des soldats simple, soit des soldats décorés par un arme.

### Comment faire pour utiliser un proxy dans cet exemple ? Que peut-on bien faire avec ?

1. Utiliser un proxy pour lazy-load un soldat (charger en mémoire le soldat seulement lorsqu'on en a besoin, et pas au démarrage de l'application)
2. Faire des statistiques sur le nombre de fois qu'un soldat a été attaqué
3. Afficher des informations à chaques fois qu'une méthode est appelée

#### Lazy-load
```java
public class LazySoldier implement Soldier {
	private Soldier soldier = null;
	
	@Override public int getStrength() {
		if (this.soldier == null) {
			this.soldier = new NoWeaponSoldier();
		}
		return this.soldier.getStrength();
	}
	
	@Override public int hit(int incomingStrength) {
		if (this.soldier == null) {
			this.soldier = new NoWeaponSoldier();
		}
		return this.soldier.hit(incomingStrength);
	}	
}

```
Une des limitations est que notre LazySoldier ne permet pas de changer le type de soldat qu'il sera.  
Il est possible d'enlever cette limitation, tout en gardant le lazy-load, de deux manière possible, à ma connaissance :
- avec de la reflection : on passe au contructeur du LazySoldier la classe du soldat à instancier. Le constructeur se chargera d'instancier un nouvel objet de cette classe.
- avec un supplier : on passe au constructeur un suplier d'une instance de soldat. On utilisera le supplier pour faire le null-check et le lazy-loading.

