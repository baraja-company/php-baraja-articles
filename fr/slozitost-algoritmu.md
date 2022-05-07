Complexité des algorithmes
==========================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	fr: complexite-des-algorithmes
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '6269d38b9a2a8d75ec01b569af8b371c'

Chaque algorithme a sa propre complexité, qui peut être exprimée en notation mathématique. Cette vue d'ensemble montre la complexité typique des algorithmes en fonction de la taille des données d'entrée (c'est-à-dire le nombre d'éléments avec lesquels ils travaillent) et quels types d'algorithmes sont adaptés à quel type de tâche.

En général, il existe un meilleur algorithme spécialisé pour chaque type de problème. Aucun algorithme n'est universellement le meilleur, et vous devez toujours connaître votre contexte.

Notation du grand O
--------------

La *notation Big O* est utilisée pour classer les algorithmes en fonction de l'augmentation de leur temps d'exécution ou de leurs besoins en mémoire lorsque la taille de l'entrée augmente.

Le tableau suivant montre les ordres de croissance les plus courants des algorithmes spécifiés en notation Big O.

Vous trouverez ci-dessous une liste de quelques-unes des notations Big O les plus couramment utilisées ainsi qu'une comparaison de leurs performances pour différentes tailles de données d'entrée.

| Notation du Grand O | Complexité pour 10 éléments | Complexité pour 100 éléments | Complexité pour 1000 éléments |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
| **O(log N)** | 3 | 6 | 9 |
| **O(N)** | 10 | 100 | 1000 |
| **O(N log N)** | 30 | 600 | 9000 |
| **O(N^2)** | 100 | 10000 | 1000000 |
| **O(2^N)** | 1024 | 1.26e+29 | 1.07e+301 |
| **O(N !)** | 3628800 | 9.3e+157 | 4.02e+2567 |

Complexité des opérations sur les structures de données
----------------------------------

| Structure de données | Accès | Recherche | Insertion | Suppression | Commentaire | ...
| ----------------------- | :------- : | :------- : | :------- : | :------- : | :-------- |
| **Array** | 1 | n | n | n | n | n | n | | 1
| **Stack** | n | n | n | n | 1 | 1 | | |
| **Queueue** | n | n | n | 1 | | | | |
| Liste de liens** | n | n | n | n | 1 | n | n
| **Table de hachage** | - | n | n | n | n | n | Dans le cas d'une fonction de hachage parfaite, la complexité sera O(1) |.
| Dans le cas d'un arbre équilibré, la complexité sera O(log(n)).
| **B-Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | |
| **Arbre rouge-noir** | log(n) | log(n) | log(n) | log(n) | log(n) | | | |
| **Arbre AVL** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | |
| **Filtre Bloom** | - | 1 | 1 | - | Lors de la recherche de `faux positifs` |

Complexité des algorithmes de tri
----------------------------

| Nom de l'algorithme | Meilleur | Moyen | Pire | Mémoire | Stable ? | Commentaire |
| --------------------- | :------------- : | :----------------- : | :----------------- : | :------- : | :------- : | :-------- |
| **Tri à bulles** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Oui | | |
| **Tri par insertion** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | | | | | |
| **Tri sélectif** | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | | | | |
| **Tri de tas** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | | Non | |
| **Tri de fusion** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Oui | | |
| **Tri rapide** | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | Non | Le tri rapide est généralement exécuté avec une complexité de pile O(log(n)). |
| **Tri de coquille** | n&nbsp;log(n) | dépend de la séquence | n&nbsp ;(log(n))<sup>2</sup> | 1 | | Non | |
| **Tri par comptage** | n + r | n + r | n + r | n + r | Oui | r - le plus grand nombre dans le tableau |
| **Tri de Radix** | n * k | n * k | n * k | n + k | Oui | k - longueur de la clé la plus longue |
