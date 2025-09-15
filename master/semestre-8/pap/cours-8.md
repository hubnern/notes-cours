---
layout: cours
precedent: 7
---

# Calculer sur des cartes graphiques avec OpenCL

Un GPU a 5000 ou plus coeurs. Il y a énormément de parallélisation. On parle de calculs sur 20000 threads.

En 2008, création de Khronos pour créer OpenCL pour concurencer nvidia et CUDA. Groupe de Apple, AMD, IBM, Qualcomm, Intel, Nvidia et autres.  
OpendCL à beaucoup de ressemblance à CUDA. C'est portable sur plusieurs architectures de GPU (et même non-GPU).

## Environnement de programmation OpenCL

OpenCL c'est un ensemble d'une bibliothèque d'instructions et un langage de programmation

Le langage est C avec des keywords en plus.

## Programmer avec OpenCL

On transfere les données de la ram vers la ram de la GPU, en placant des ordres "copies de données". C'est possible d'attendre que ça soit terminé de manière synchrone ou asynchrone.  
On transfère du code sur les processeurs du GPU puis on place l'ordre "run" pour executer le code sur les données.  
On transfère ensuite les données de la ram du GPU vers la ram une fois que le cacul est terminé avec l'ordre "copie les données de ramGPU vers ram"

Une unité de calcul est construite par un controlleur, 8 coeurs, mémoire local, et une unité de précision pour les doubles.  
Le controlleur (dispatch unit) envoie un ordre à tout ses processeurs pour faire l'instruction sur ses registres.  
Il n'existe pas de "threads". Tout est registre. Un processeur à 64Kb de registres.  
Le dispatch unit met 4 cycles pour récupérer et décoder la prochaine instruction. Il faut donc mettre 32 threads (8 coeurs x 4 cycles) sur l'unité de calcul pour calculer le temps que la prochaine instruction soit calculé. Il y a toujours 32 threads qui font la même instruction.

Les threads sont regroupés en warps. Nvidia a des warps de 32 threads. AMD a des warps (appelé wavefront) de 64 threads.  
Tous les threads du warp exécutent la même instruction sur le même cycle logique.


## Créer un noyau OpenCL

```c
__kernel void ScalVec(__global float *vec, float k) {
	int index = get_global_id(0); // thread id
	vec[index] *= k;
}
```
Il n'y a pas besoin de faire de boucles. Chaque thread calcules un élément du vecteur. (la boucle est remplacée par le parallélisme.)

```c
// on donne un argument au kernel
err |= clSetKernelArg(compute_kernel, 0, sizeof(cl_mem), &cur_buffer);
// pour exécuter le kernel
err = clEnqueuNDRangeKernel(queue, compute_kernel, 2, NULL, global, local, 0, NULL, NULL);
//2 : l'image est en deux dimensions, on "numérote" les threads en x et y
```

Structurellement c'est impossible de mettre une barrière entre tous les threads.

# Memory concern

Il est mieux d'avoir plusieurs threads qui font des accès mémoires proche et un thread fait des accès mémoires écartés, plutôt que plusieurs threads font des accès mémoires éccarté et un thread des accès proches. (avec une carte graphique).  
C'est l'aggrégation des accès mémoires. Si plusieurs threads demandes presque au même endroit, la carte fait tout les accès en un coup.

# Conditions sur les threads

Pour des threads ne doivent pas exécuter la même chose selon par exemple une condition. La carte gèle les threads qui ont la condition à faux, et exécute les threads qui ont la condition à vrai. Elle gèle ensuite les threads à condition vrai, et exécute les threads à condition fausse. De cette manière, à chaque fois, les threads qui travaillent font tous les mêmes instructions.
