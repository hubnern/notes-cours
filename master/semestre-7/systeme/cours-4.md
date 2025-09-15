---
layout: cours
precedent: 3
suivant: 5
---

## Processus et threads

Thread = Contexte d'exécution  
Processus = Thread + Espace d'adressage  
*plusieurs thread peuvent partager le même espace d'adressage.*

Quelques threads fonctionnent que dans le noyaux.

## Race conditions

Les threads peuvent accéder à la même mémoire simultanément.  
Cela peut provoquer des fonctionnent indéfinie, corruption de données...  
Par exemple : liste chaînée, structure de données

Écrire un nombre dans une mémoire ne pose pas de problème (opération atomique). Le matériel fera une instruction après l'autre si deux écritures arrive en même temp. Les deux valeurs ne seront pas mélangées.

Une incrémentation n'est pas atomique et peut ne pas fonctionner correctement lorsque plusieurs threads cherchent a incrémenter une même variable.

## Synchronisation

Pour protéger les structures de données de la corruption par l'exécution concurrente de code non-bloquant :
- exclusion mutuelle de sections critiques
- reader/write
- producer/consumer

Comment forcer l'exclusion mutuelle ? On voudrait envelopper notre code dans une zone critique.  
On peut implémenter cela facilement grace à une variable booléenne globale. Tant que celle-ci est à faux, on attend. Dès qu'elle passe à vrai, le programme continue.  
Problème : Si deux processus lisent la variable globale en même temps, ils rentrent tout les deux dans la zone critique.

L'algorithme de Peterson permet de régler ce problème mais a des limitations :
- fonctionne seulement avec deux processus
- un processus est en attente active. (while loop)

Le fournisseurs de processeurs on créé une nouvelle opération machine : test-and-set. Cette opération écrit 1 dans une adresse et retourne la valeur qu'il y avait avant l'écriture du 1.

Plus tard, les sémaphores on été créé. Un code de haut niveau pour faire de la synchronisation. Un semaphore a une boite à jetons. L'appel de sa méthode V ajoute un jeton dans la boite. L'appel de sa méthode P est bloquant jusqu'à ce qu'il y ai un jeton dans la boite ; au moment où il y a un jeton, P consomme le jeton et se débloque.

V() : lance un jeton dans la boite.  
P() : attend le jeton dans la boite, le prend, et continue  

`_thread int phase = 0;` : variable locale à un thread (normalement un thread n'a que des variables globales entre threads)

Parfois, des applications ont un signal a deux temps. D'abord, elle signale qu'elle est arrivée, elle fait des traitements, et ensuite, elle attend lorsqu'elle a terminée ses traitements. De cette manière, elle n'attend pas directement une fois qu'elle est arrivée.

### Problème producteur-consomateur

Pour une structure FIFO :
- put(elem) et get() sont asynchrone
- si get() est appelé et fifo vide : erreur levée

Pour que le get() n'ait pas d'erreur car file vide :
- On ajoute un sémaphore "cons" initialisé à 0 jetons.
- Le producteur doit lancer des jetons dans le sémaphore "cons" après avoir ajouté un élément dans la file.
- Le consommateur doit attendre un jeton dans le sémaphore "cons" avant de récupérer un élément dans la file.

Pour que le put(elem) n'ait pas d'erreur de file pleine :
- On ajoute un sémaphore "prod" initialisé a "taille max de la file" jetons.
- Le producteur récupère un jeteon du sémaphore "prod", puis ajoute son élément.
- Le consommateur lance un jeton dans le sémaphore "prod" après avoir récupéré un élément.

Lorsqu'il y a plusieurs producteurs et consommateurs, il suffit de mettre un semaphore mutex pour les producteurs et les consommateurs entre les appels P, V et les ajouts/récupérations dans la file.

### Problème lecteur-écrivain

Une structure de données accédée par de type de processus :
- lecteur : lis les données
- écrivains : modifie les données

Les lecteurs peuvent lire les données ensemble, les écrivains sont mutuellement exclusifs, et les écrivains et lecteurs sont mutuellement exclusifs.

On ajoute un sémaphore "writeToken" initialisé à 1.  
Pour écrire, on attend et récupère un jeton, puis on écrit nos données, puis on ajoute un jeton dans le sémaphore.  
Si les lecteurs veulent lire, ils doivent voler le jeton des écrivains pour qu'ils arretent d'écrire. Ainsi les lecteurs lisent les données et lorsqu'ils ont finit, ils rendent le jetons aux écrivains.
