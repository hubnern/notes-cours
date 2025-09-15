---
layout: cours
suivant: 2
---

> Modern Operating Systems, Andrew S. Tanenbaum

# Introduction

Pourquoi utiliser un OS ?
- Pilotes de périphériques et factorisation du code
- Abstraction de haut niveau (fichiers, fenêtres graphiques)
- Virtualisation des ressources (partage de mémoire et processeur), et arbitrage des ressources

Un OS est une sorte de machine abstraite composée de :
- un ensemble de drivers
- un ensemble de programmes
- un ensemble de bibliothèques
- une autorité qui règle tout

Lancement d'un OS :  
Ordinateur allumé -> BIOS se lance -> BIOS lance le kernel -> kernel se lance -> kernel lance le premier processus -> etc

## Interruption
Une interruption est un signal envoyé au processeur.  
Elle est envoyé par les périphériques ou le processeur lui même.

Une table est créé qui renseigne l'endroit a sauter selon le numéro de l'interruption. Il doit y avoir tout les numéros d'interruption. Cette table est créé par le kernel.

Quelques interruption peuvent être parfois ignorées (plus retardées que ignorées).

## Partage de temps (temporisateur)

Le kernel créé un temporisateur qui envois une interruption périodiquement. Ce temporisateur permet d'exécuter du code noyau tout les x millisecondes. 

## Restriction des privilèges

Seulement le kernel devrait être tout puissant.  
Une liste d'instruction interdite aux processus est renseignée et seulement le kernel aura droit de les executer.  

Le processeur a deux modes. Un mode noyau (mode réel) et un mode utilisateur (mode protégé).  
Si le processeur est en mode utilisateur il n'acceptera pas les instruction interdites. Si il reçoit une instruction interdite en mode utilisateur, il lancera une interruption qui rendra la main au kernel. Le kernel a pour habitude de terminer le processus qui a essayé d'exécuter l'instruction interdite.  
Un processus quelconque devra demander au kernel de faire l'instruction à sa place.
