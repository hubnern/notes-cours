---
layout: cours
precedent: 4
suivant: 6
---

# Architecture des ordinateurs à mémoire commune

On trouve du parallélisme au niveau des circuits, des instructions, des threads (multicoeur/multiprocesseur), et au niveau des données (unité vectorielle, accélérateur)

## Parallélisme des instructions

### Pipeline

Chaque étape est traitée par une unité fonctionnelle dédiée. Idéalement chaque instruction progresse déune étape à chaque top.

Architecture CICS vs RISC (Complex Instruction Set Computer vs Reduced Intruction Set Computer) : Pour n instructions, CISC a k cycles fois n, alors que RISC à (nb étage - 1) + n cycles. Attention, une instruction CISC est traduite par plusieurs instructions RISC.

Actuellement les pipelines sont RISC avec des instructions CISC.

Plus il y a d'étages dans le pipeline, plus il y a de parallélisme. La durée de l'étage le plus lent détermine la fréquence maximale du pipeline.

Il peut y avoir des bulles dans le pipeline à cause de la dépendance des données (attend une valeur en cours de calcul), la dépendance de contrôle (attend l'adresse de la prochaine instruction), et des défaut de cache.

Optimisation du pipeline : exécution Out of Order. Cela permet déviter les bulles en modifiant l'ordre d'exécution des instructions. Les micors instructions se doublent dans le pipeline et nécessite une analyse à la volée par le processeur. Des barrières mémoire empêchent le réordonnancement.

Transformation du code :  
Le compilateur optimise le code sur une fenêtre large. Il peut transformer le code en profondeur[...]

Nids de boucles : les boucles consomment l'essentiel du temps d'exécution. Leur régularité rend leur optimisation possible. Il est possible de transformer un boucle grace à la fusion, permutation, ou déroulement.

Optimisation des branchements :  
Tirer partie de l'évalutation spéculative. Stratégies de prédiction simple : toujours sauter, ne jamais sauter. Stratégies de prédiction avec historique : comme la dernière fois, comme d'habitude.

## Parallélisme des threads

On ne gagne plus en fréquence de pipeline, mais en finesse de gravure.

On multithread le pipeline. Il a ainsi un meilleur rendement au niveau du débit.