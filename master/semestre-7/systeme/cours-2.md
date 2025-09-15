---
layout: cours
precedent: 1
suivant: 3
---

## Processus

Un processus est une instance d'un programme.  
Un processus est un ensemble d'espace d'adressage et un d'un contexte d'exécution.

## Espace d'adressage

Il est typiquement composé de regions mémoires distinctes. (Une région étant un séquence continue d'adresse).  
L'espace contient habituellement les régions suivantes :
- Code (*text segment*). Contient les instruction exécutables. Souvent en lecture seule.
- Données. Contient les variables statiques dans deux segments, les variables initialisées (*data segment*) et les variables non initialisées (*bss segment*)
- Tas (*Heap*). Contient les allocations dynamiques (malloc/free). Peut être agrandi si elle est complète.
- Pile (*Stack*). Contient les paramètres des fonctions, les variables locales et les appels aux méthodes. Croissance automatique. Sur linux la limite de la pile est de 8Mb.
- Bibliothèques partagées. libc, libm, libGL, etc. Lien fait dans la mémoire au premier appel d'un méthode contenu.

Un accès dans une zone non contenu dans l'espace d'adressage provoquera une segmentation fault.

## Attributs d'un processus (contexte d'execution)

Le kernel garde des informations supplémentaire pour chaque processus :
- Process ID (pid)
- Priorité
- Id d'utilisateur (réel et effectif). Celui qui lance le programme (réel). Parfois le programme peut demander a être un autre utilisateur, pour avoir de droits supplémentaire (effectif).
- File descriptor table
- Table des signaux
- Espace pour les sauvegardes des registres du processeur.

## Création de processus

Le noyau créé le premier processus.  
Le processus P0 veux créer un nouveau processus. Il doit utiliser un appel système.  
Le noyau enregistre un nouveau contexte d'exécution pour P1.  
Le noyau démarre le processus P1.  
*Que faire maintenant ? redonner la main à P0 pour lui dire que P1 a démarré, ou donner directement la main à P1 ?*
Le noyau redonne la main à P0 pour lui dire le résultat du syscall.  
Jusqu'au prochain systimer P0 continue son execution.

## Ordonnancement des processus

Après qu'une interruption est apparue :
1. L'interruption handler sauvegarde les registres sur la pile.
2. Le noyau décide si il continue le processus ou si il change de processus :
	- Reste sur le même processus
		1. Le noyau restore les registres
		2. Retourne sur le processus
	- Change de processus
		1. Changement de contexte (change les registres utilisé)
		2. Restore les registres
		3. Retourne sur le processus

## État d'un processus

Juste créé -> Prêt  
Prêt ->[élis] En cours d'exécution (noyau)  
En cours d'exécution (noyau) ->[retour interruption] En cours d'exécution (utilisateur)  
En cours d'exécution (utilisateur) ->[interruption, syscall, exception] En cours d'exécution (noyau)
En cours d'exécution (noyau) ->[préemption] Prêt
En cours d'exécution (noyau) ->[forcé à attendre] Bloqué
Bloqué ->[réveillé] Prêt
En cours d'exécution ->[fin] Zombie

## Ordonnancement

Le but général d'un ordonnancement de processus est de :
- Optimise le processeur et maximiser la joie de l'utilisateur
	- chaque processus à peu accès au processeur
	- le processus est toujours en cours d'exécution à 100%
	- la réactivité des interactions des processus est optimal
	- le temps de calcul de long processus est minimal
	- etc

Malheureusement, satisfaire toutes les règles en même temps est impossible.
