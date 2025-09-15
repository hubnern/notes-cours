---
layout: cours
precedent: 4
suivant: 6
---

# Hoare monitors

Construction de synchronisation proposé pour les languages orienté objet.  
Un monitor a deux méthodes f et g qui sont mutuellement exclusive.

Initialement utilisé dans Simula67, Concurrent Pascal, puis réutilisé dans Ada, Java, et les OS.

Principe :
- Verrou implicite associé à un moniteur
- Un objet Condition lui est associé qui permet la synchronisatoin
	- wait condition : bloquant inconditionnellement 
	- signal condition : reveille le processus ou ne fait rien

Les conditions sont différentes des sémaphores car il n'y a pas de tokens stockés. Lorsqu'il se met en veille, il relache son verrou et se met en attente sur la condition dans un temps indivisible. Lorsqu'il serra reveillé, il récupèrera le verrou et le bloquera.

Limitation : La condition ne peut pas attendre hors du moniteur, et le mutex a un propriétaire.

Comme les semaphores, les moniteurs de Hoare sont implémenté avec des instructions atomiques.  
Les implémentation dans l'espace utilisateur font souvent plusieurs tests (polling) avant de se bloquer. *On vérifie plusieurs fois si le verrous est ouvert avant de se bloqué s'il est fermé.*

Linux 2.6 a introduit les FUTEXes (Fast User-Level Mutex). Utilise des opérations atomiques sur des variables int 32 bits. Le noyau se sert des adresses mémoires comme verrou.
