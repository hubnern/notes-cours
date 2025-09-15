---
layout: cours
precedent: 3
suivant: 5
---

# Paradigme des tâches

### Tâches OpenMP :

`#pragma omp task firstprivate(data1) shared(data2)`  
`foo(data1, data2)`

Mécanisme pour générer du parallélisme à grain fin. Facilite l'expression récursive du parallélisme. Solution OpenMP au problème du producteur/consommateur. Depuis OpenMP 4.0 tout est tâche.

Une tâche est une exécution différée d'une fonction.  
Le support d'exécution peut décider d'exécuter la tâche dès sa soumission ou de différer son exécution.

Le thread qui créé une tâche n'est pas forcément celui qui l'exécutera.

```c
#pragma omp parallel // créé un ensemble de thread
#pragma omp master   // seul le thread principal va exécuter le code
#pragma omp task     // création de la tâche
printf("hello world"); // résultat : il y a une seule tache de créé, mais elle sera réellement exécutée par un des threads
```

`taskwait` : attend la terminaison des tâches filles.

`taskgroup` : attend la terminaison des tâches de la descendance.

Attention, c'est une synchronisation des tâches, pas des threads.

`depend` : dépendance entre tâches. Construit un graphe de tâches. Calcul dynamique des dépendances entre tâches soeur suivant l'ordre de soumission des tâches.
- `depend(in:x)` : x est en lecture seule
- `depend(out:x)` : x est en écriture seule
- `depend(inout:x)` : x est en lecture et écriture
- `depend(mutexinoutset:x` : x en lecture/écriture, la tâche doit être réalisé en exclusion mutuelle, les tâches portant sur x sont commutative.

## Ordonnancement des tâches

Une tâche initiée peut être exécutée par tout thread de l'aquipe de thread initiateur.

Un thread ayant pris en charge l'exécution d'une tâche doit la terminer :
- à moins que la clause `untied` ait été spécifiée
- mais peut l'exécuter en plusieurs fois

Un thread peut prendre/reprendre en charge une tâche :
- à la création d'une tâche
- à la terminaison d'une tâche
- à la rencontre d'un taskwait, d'une barrière, fin d'un taskgroup

Comment choisir la tâche : Parcours en profondeur d'abors pour un thread donné. De la même manière que cilk.


### Cilk

[...]

#### Ordonnanceur de vol de travail

Chaque processeur possèdent une file à double entrée (deque) des threads prêts et manipule le bas de la deque comme une pile.  
Quand un processeur arrive à court de travail, il vole un thread du haut de la file d'un autre processeur aléatoire.

### Clause `untied`

Elle autorise une tâche à être suspendue à tout instant et à être reprise par tout thread de l'équipe. Une tâche liée ne peut être exécutée que sur le thread qui a débuté son exécution. Une tâche non liée est préemptible en dehors des points d'ordonnancement.

Il faut éviter d'utiliser `untied` en même temps que des variables threadprivate, l'identité du thread (`omp_get_thread_num()`)

La clause `taskyield` demande à l'ordonnanceur de changer de tâche.

`priority()` : décide la priorité d'une tâche.

`final(bool)` : après cette clause, plus de tâches sont crées, tout se fait en séquentiel.

## Tâches et granularité

Gestion de la granularité dynamique : décision dépendante de l'état courant de la machine, recherches actuelles dans un contexte hétérogène.