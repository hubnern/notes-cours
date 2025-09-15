---
layout: cours
precedent: 1
suivant: 3
---

# OpenMP

C'est une API pour la programmation parallèle en mémoire partagée :
- C, C++, Fortran
- Portable
- Porté par l'Architecture Review Borad (Intel, IBM, AMD, Microsoft, Cray, Oracke, NEC, ...)

Elle est basé sur des annotations `#pragma omp <directive>` et des fonctions `omp_fonction()`.  
Elle permet de paralléliser un code de façon plus ou moins intrusive. Plus on en dit, plus on a de performance. Facile à mettre en oeuvre par un non spécialiste.  
Ne permet pas de créer ses propres outils de synchronisation.

## Les bases

### `#pragma omp parallel`

Permet de paralléliser une instruction. Elle sera exécutée sur tous les threads disponibles.

```c
int main() {
	#pragma omp parallel
	printf("bonjour\n");
	printf("au revoir\n");
	return EXIT_SUCCES;
}
```

Possible d'utiliser `OMP_NUM_THREAD` en variable d'environnement pour choisir le nombre de thread à utiliser. Ou dans le code avec `omp_set_num_threads(3)`.

### Parallélisme de type fork-join

Un unique thread exécute séquentiellement le code de main.  
Lors de la rencontre d'un bloc parallèle, un ensemble de thread est créé, ils exécutent l'instruction et main attend que tous les threads soient terminés.

### `#pragma omp for`

permet de paraléliser une boucle for. Elle sera répartie sur les threads disponibles.

```c
int main() {
	#pragma omp parallel
	#pragma omp for
	for(int i=0; i < 100; i++) {
		//do things
	}
}
```

Il est possible d'utiliser `#pragma omp parallel for` pour faire les deux annotations à la fois.

### `#pragma omp barrier`

Permet de préciser une barrière d'attente des threads de façon explicite.  
Il y a des barrières implicites à la fin des `parallel` et `for`.

### `#pragma omp master`

Permet de dire qu'une instruction doit être exécutée seulement sur le thread principal. Il n'y a pas de barrière implicite.

Instruction remplacée par `#pragma omp masked` dans les nouvelles version d'OpenMP.

### `#pragma omp single`

Permet de dire qu'une instruction doit être exécutée par un seul thread (usuellement le premier qui arrive à cette instruction). Il y a une barrière implicite.

### `firstprivate(pnt_var, ...)`

Spécifie que chaque thread doit avoir sa propre instance d'une variable, et que sa valeur doit être initialisé à la valeur de la variable, car elle existe avant la construction parallèle.

```c
int main() {
	double* in = input;
	double* out = output;
	#pragma omp parallel firstprivate(in, out)
	//do things
}
```

### `nowait`

Permet d'enlever une barrière.

### `atomic`

Permet d'exécuter une instruction simple de façon atomique.

### `critical`

Met un ensemble d'instruction en zone critique.
