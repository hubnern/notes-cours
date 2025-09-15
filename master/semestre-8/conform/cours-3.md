---
layout: cours
usemathjax: true
precedent: 2
suivant: 4
---

# Frama-c

C'est un analyseur statique de code C. Il permet la déclaration et la vérification formelle de contrats de fonction exprimé en ACSL (Ansi-C Specification Language) qu permet d'exprimer des phrases en logique de premier ordre.

RTE : permet d'ajouter automatiquement des assertions pour empecher les comportements indéterminés (erreur à l'exécution).  
WP : applique l'algorithme Weakest Precondition à un contrat de fonction puis détermine sa validité avec un solveur SMT externe (Alt-ergo, Z3, Coq).

# Contrat de fonctions
Un contrat de fonction contient :
- Un domaine de définition $$D$$. Il décrit l'ensemble des états de mémoires dans lequel la fonction peut être appelée (=paramètres).
- Une valeur de retour $$V$$. Elle décrit ce que voudra la valeur retournée par la fonction
- Une explication des effets de bords sur l'environnement mémoire auquel a accès la fonction.

La précondition est $$D$$. Les postconditions sont $$V$$ et les effets de bords.

*Un contrat de fonction ne dit rien sur son comportement si elle est appelée hors de son domaine de définition.*

Utilités pricipales :
- Décrire le comportement de la fonction pour qu'un utilisateur sache comment l'appeler correctement et ce qu'il obtiendra après l'appel.
- Permettre au développeur de la fonction de certifier que le code qu'il a produit correspond bien à la spécification.

*pour ce cours on se concentrera essentiellement sur le deuxième point*

# Modèle de programmation

## Etat de mémoire

On note $$Var$$ la mémoire locale d'une fonction.  
On note $$\mathbb{Z}$$ la mémoire globale d'un programme.  
On note $$M(x)$$ la valeur de la variable $$x$$.  
`\result` contient la valeur de retour de la fonction.
