---
layout: cours
precedent: 8
---

# Threads

Un thread est un fork qui partage toutes les données avec le "père".

```c
#include <pthread.h>

pthread_t pid;
pthread_create(&pid, NULL, thread_func, "Hello");//créer un thread qui va executer la fonction `thread_func` avec comme argument "Hello" (il doit être un pointeur)
pthread_join(&pid, NULL);//attend la fermeture du thread
```
