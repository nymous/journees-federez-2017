Attaque de protocoles
=====================

>Georges Bossert @inthreat.com  
>@Lapeluche

Rappels sur qu'est-ce qu'un protocole de communication

On parle du HTTP, et de la nouvelle version HTTP/2.  
Beaucoup de changements :

  - Protocole binaire et presque tout le temps chiffré
  - Entêtes compressées
  - Le serveur peut pusher des donées aux clients

Beaucoup de développements en cours autour du h2, pour rendre les serveurs et les clients compatibles

Problème : h2 c'est nouveau, c'est très complexe, et tout le monde veut l'implémenter le plus vite possible -> toutes les implémentations sont-elles fiables ?

Méthodes pour rechercher des vulnérabilités :

  - Étude des spécifications pour trouver des failles de conception
  - Analyse manuelle ou semi-auto du code (s'il est disponible)
  - Fuzzing : émettre pleeeeeeein de données potentiellement inattendues, faites pour ne pas être normales, et on observe les réactions de la cible. Si ça crashe -> bug ! Outils pour faire ça : fusil, AFL, Sulley, Peach

Comment on fuzze ?

  1. Démarrer la cible
  2. Ouvrir un canal
  3. Écrire des trucs aléatoires sur le canal
  4. Observer la cible, a-t-elle crashé ?
    - Le canal est-il ouvert ?
    - Le processus est-il encore vivant ?
    - Répond-il toujours correctement ?
    - On utilise GDB
    - On utilise Valgrind (analyse de la mémoire)

Problème : on n'a pas eu de crash avec notre fuzzer. On fait quoi maintenant ?

On devient plus intelligent que juste faire du fuzzing aléatoire, on va modifier des données valides.

Exemple avec le h2 : on allume un navigateur, on fait quelques échanges h2, on les capture, et ensuite on les modifie. -> outil `pyzzuf` (little bit-flip atomic bomb)

:warning: On doit faire des mutations intelligentes pour que ça marche mieux ! Par exemple, on garde un champ `length` correct, et un champ `type` valide aussi (pour le h2).

Bibliothèque qui le fait pour nous : `Netzob` Reverse engineering communication protocols  
Framework en python qui permet de faire du reverse engineering et de la modélisation de protocoles

On peut définir une structure de message, dans lequel on met plusieurs champs qui contiennent parfois de l'ASCII, parfois des octets, parfois une taille qui dépend de d'un autre champ...  
Ensuite on demande à Netzob de générer un message du type défini, et ça crée un message correct.

Si on veut être encore plus intelligent, on va modifier des *séquences* de données valides.

On regarde les chemins de messages valides, et on génère des mutations sur les messages valides, ou alors on génère des chemins non valides.

Lib pour faire ça : pyLSTAR, qui permet d'inférer la machine à états d'une cible.  
Comment ça marche : on met la cible dans un process wrapper, qui peut démarrer/arrêter la cible, communiquer avec elle, et *surtout* la réinitialiser **correctement** (comme ça quand on croit avoir compris un truc, on peut reset et recommencer pour vérifier ; si le reset est pas correct c'est pas possible).  
On obtient en sortie une machine de Mealy

Pour protéger un serveur web (pour détecter un tentative d'intrusion), on peut utiliser un IDS : il analyse le trafic entrant en parallèle du serveur, et décide en fonction du contexte actuel du serveur si le trafic est malveillant.  
Comment l'attaquer ? -> Contournement de l'IDS : si on arrive à désynchroniser l'état de la cible et de l'IDS (ex : on envoie un RESET, l'IDS se dit "c'est bon, la connexion est fermée j'arrête de surveiller", alors que le serveur ne se reset pas et continue à écouter).

Des résultats ?

  - Plusieurs vulnérabilités déjà remontées
  - Ça nécessite **beaucoup** de temps CPU
