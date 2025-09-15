---
layout: cours
usemathjax: true
precedent: 6
---

## Pagination de la mémoire

Pour éviter les petits morceaux de mémoire inutilisé, on découpe la mémoire en morceaux fixes (pages). Lorsqu'un processus veut de la mémoire, il peut la demander seulement en nombre de pages.  
La division est virtuelle.  
Une page a seulement deux états : alloué, libre.

L'espace d'adressage des processus est aussi divisé en pages. (car une page est la seule unité d'allocation).  
Il n'y a pas de garantie que des pages virtuelles contigue le soient aussi dans la mémoire physique.  

### Page virtuelle vers page physique

Lorsque le CPU execute du code utilisateur il n'a que des adresses mémoires virtuelles.

En admettant que les pages font 4k et les adresses sont sur 32bits. Il sufiit de prendre les 20 premiers bits pour savoir le numéro de la page, les 12 autres sont le déplacement dans la page. Il suffit donc de changer les 20 bits pour retrouver la bonne adresse physique.

Si on voudrait stocker le déplacement de chaque page pour chaque processus, il faudrait stocker $$2^{20}$$ données  (environ 1 million) par processus, avec des entrées à 32bits (4 octets). Cela donne 4 Mo par processus.  
Ces tables sont stockés en RAM.
 
Maintenant, le processeur doit savoir quelle table utiliser. Pour cela on ajoute une adresse de la table à utiliser par processus, elle sera mise à jour par le système. On y stocke la page physique, un bit qui dit sit elle est aloué ou pas, et les droits rwx.
 
Le Memory Management Unit (un circuit physique) permet de transformer l'adresse virtuelle en adresse physique en modifiant les bits de page grêce à la table des pages réelles.
 
A cause de cette pagination, on double les accès mémoire : un pour la page a utiliser, un pour le vrai accès.
 
### Réduire l'empreinte mémoire
 
Les tables de pages contiennent plein de pages invalides (libres). Elles sont souvent en grandes régions.

On peut séparer la tables des pages en plusieurs blocs, et utiliser une deuxième table qui choisi le bout de table des pages à utiliser. (les deux premiers bits servent à l'index de la table de choix des bouts).  
L'utilité de faire un troisième accès comme ça est de pouvoir ne pas allouer un morceau de table de pages qui n'a que des pages non allouées.

Il serait possible de continuer à faire ces tables, et par exemple avoir une table de troisième niveau pour désallouer des morceaux de la table de deuxième niveau.

Malheureusement, plus il y aura de niveau de table, plus le temps est long pour avoir un accès mémoire à notre variable.  
Pour régler ce problème, on introduit un cache dans le MMU, nommé Translation Lookaside Buffer (TLB). Le but est d'utiliser le plus souvent le cache car les accès mémoires cache sont "gratuits". Il garde des tuples <adresse virtuelle, adresse physique, mode>. Lorsque le TLB est plein et qu'il faut y ajouter un tuple, on remplace la plus ancienne données par la nouvelle. Cette stratégie est la LRU (Last Recently Used). 

*Un cache stocke les variables recherchés les plus récentes et intercepte les demandes du cpu en répondant à la place de la ram. Le cache est beaucoup plus rapide que la RAM.*  
*Un cache est plus rapide grace au hardware. Toutes les comparaison sont faitent en même temps. Les comparateurs sont mis en parallèle. Cela créé un multiplexeur. Mettre les comparateurs de telle sorte est cher à fabriquer.*

Lorsque le scheduler change de processus, que faire des entrées dans le TLB ? Ces valeurs sont plus les bonnes, elles changent entre chaque processus.  
Est-ce nécessaire de sauvegarder les valeurs du TLB lors du changement de contexte ? Ou est-ce mieux de simplement vider le TLB ?  
Cela dépend de la profondeur des tables de pages. Plus la profondeur est importante, moins il faut vider le TLB.  
Une autre possibilité est d'ajouter une colonne dans le TLB, un tag, qui indique à quel processus appartient la valeur du cache. On identifie un processus grâce aux pointeur de la table des pages.

### Page des tables user/kernel

Le noyau est aussi paginé. Pour cela, on utilise la même table des pages et on y rajoute un colonne qui détermine si l'adresse appartient à l'utilisateur ou au noyau. Une partie de la table virtuelle est cachée en mode utilisateur.  
La partie noyaux de la table est partagée entre toutes les tables des processus

### Optimisation de la pagination

Pour utiliser moins de mémoire et augmenter la vitesse des processus, le noyau utilise de nombreuses optimisations aggressives.

#### First-touch optimization

Lors de la création d'un processus, seulement une partie de l'espace d'adrdressage est alloué, seulement quelques pages sont allouées dès le début. L'allocation des autres est retardée. Les pages retardées seront allouées seulement lorsque le processus en aura besoin.

Certains morceaux de code ne seront jamais utilisé, comme ceux des blibliothèques, ou des tableaux statiques.
Lorsqu'un programme veut accéder à une adresse dans une page qui n'est pas encore alloué, le kernel va allouer la page avant de retourner la valeur.  
Problème, on ne peut pas allouer à chaque fois une page si elle ne l'est pas encore car certaines pages sont invalides et le processus n'a pas le droit d'accéder à cette page.
