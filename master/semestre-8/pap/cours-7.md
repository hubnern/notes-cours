---
layout: cours
precedent: 6
suivant: 8
---

# Parallélisme de données

Dans beaucoup d'application, le parallélisme vient d'appliquer une même opération simultanément sur tout un ensemble de données.

Lors des processus scalaires, il y a plusieurs sources d'inéficacité : Il faut plein d'instructions pour modifier le compteur, la latence de la mémoire impacte sur chaque donnée. Pour un tableau très grand, le cache n'aide plus.

## Processeur vectoriel (SIMD)

Le but est d'envoyer un flux de données à travers la pipeline de l'ALU qui sortira un flux de données. Durant tout le flux, l'opération de l'ALU ne change pas.

L'implémentation est faite par des instructions spécifiques (vload, vstore, vadd, ...)

Des registres de vecteurs ont été ajouté, ils sont chargés par blocs.

Le chargement des données est pipeliné, le stream commence dès que des valeurs sont chargés dans les registre. On n'attend pas que toutes les données soient chargés dans le registre. Un registre sait si une valeur est chargée ou pas.

Dans le cas ou le stream se fait par conditions, il y a un registre spécial qui contient le résultat des conditions pour tout le stream (calculé au préalable), c'est le Vector Mask Register. L'ALU executera l'opération seulement si le VMR l'autorise.

## Intel Intrinsics

Pour faire une opération entre un scalaire et un vecteur, on transforme le scalaire en vecteur (contenant plusieurs fois la même donnée).
