Algorithme des moteurs de recherche sur Internet - Triage et descripteur
========================================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	fr: algorithme-des-moteurs-de-recherche-sur-internet---triage-et-descripteur
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

Dans cette leçon sur les principes d'un moteur de recherche Internet, nous allons comprendre comment un moteur de recherche trie, décrit et évalue les résultats.

Tri des résultats
----------------

Imaginons un baril fini qui est actuellement prêt sur le serveur de recherche. Notre première requête de recherche nous parvient de l'utilisateur et nous devons maintenant effectuer un premier tri "grossier", qui sera affiné par la suite.

Prenons l'exemple suivant de requête d'entrée :

```txt
[O (and) pejskovi (and) a (and) kočičce]
```

Oui, il s'agit de la forme sous laquelle le serveur de recherche reçoit la requête traitée de l'utilisateur et attend maintenant que le résultat soit renvoyé. Au total, nous avons moins de 300 ms pour faire cela, c'est rapide :)

Dans la première étape, nous obtenons des barils remplis d'ID de résultats futurs (également appelés candidats) et d'opérations à effectuer. Ce baril récupéré peut ne pas être complet dans certains cas, mais contient uniquement les valeurs que le serveur a réussi à lire sur le disque dans un intervalle de temps prédéterminé (également appelé **timeout**). Par exemple, nous obtenons les séries de chiffres suivantes (qui sont partiellement triées à l'avance) :

| Barrels | Documents |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| chien | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| chatte | 9,19,42,57,58,62,68,83 |

Nous effectuons maintenant une intersection grossière de tous les ensembles et obtenons une liste d'identifiants de documents qui sont les mêmes dans tous les barils. En pratique, on passe par la liste la plus courte et on cherche les autres.

Les identifiants communs à tous les barils sont "19, 42, 58", de sorte que la future page de résultats contient 3 éléments dans un ordre encore inconnu. Nous considérons toujours les documents comme des identifiants, car cela permet d'économiser une énorme quantité de données. Dans les résultats de recherche, cependant, nous ne voulons généralement pas présenter à l'utilisateur les numéros des documents, mais plutôt le titre (l'en-tête), la description, l'URL et d'autres informations. Ceci est fourni par le composant "descripteur".

Descripteur
---------

Le chapitre précédent nous a laissé une liste de candidats pour les résultats futurs. Nous les avons récupérés sous une forme triée en fonction du système, c'est-à-dire comme le moteur de recherche les a classés séquentiellement dans l'index. À ce stade, nous ne pouvons plus effectuer une lecture séquentielle des big data, mais devons parcourir séquentiellement un certain type de base de données relationnelle afin de trouver des informations supplémentaires pour un autre type de tri plus compréhensible pour l'utilisateur.

Par exemple, une tranche de base de données peut ressembler à ceci :


| ID | Titre | Étiquette | URL | Rang | Classement
|----|---------|---------|-----|------|
| 19 | Josef Čapek : L'histoire d'un chien et d'un chat | C'était l'époque où le chien et le chat étaient encore fermiers ensemble ; ils avaient leur petite maison au bord de la forêt et y vivaient ensemble et voulaient tout faire comme les grandes personnes. | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| 42 | Parler du chien et du chat | "Très bien", dit le chien, et le chat prit du savon et un pot d'eau, et s'agenouilla sur le plancher, et prit le chien comme brosse, et frotta tout le plancher avec le chien. Le sol était tout mouillé | www.capek.narod.ru/povidani.htm | 86 |
| 58 | Comment le chien et le chat ont fait un gâteau pour la fête| Demain, c'était la fête du chien et l'anniversaire du chat. Les enfants le savaient et ont voulu surprendre le chien et le chat pour leur anniversaire. Ils pensaient à ce qu'ils pouvaient faire pour le chien et | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Calcul de la pertinence
-----------------

La dernière étape consiste à calculer la pertinence de la réponse. Pour la plupart des résultats, il suffit de représenter la pertinence sous la forme d'un nombre unique avec un rayon fixe selon lequel les résultats seront triés. Par rang, les résultats sont à nouveau triés "grossièrement" en plusieurs groupes (le nombre de groupes dépend de la variance des rangs) et ces groupes sont ensuite traités.

Les groupes les mieux classés sont pris en premier, ce qui suffit souvent à trier la première page de résultats et nous n'avons pas à nous occuper d'autre chose. Cependant, si plusieurs documents différents ont le même rang, nous devons effectuer une analyse plus détaillée et calculer des rangs auxiliaires supplémentaires pour aider à classer la page. Le rang supplémentaire peut être par exemple la qualité des liens, l'ancienneté du document, la longueur du titre, le "look and feel" de l'URL et bien d'autres facteurs.

Renvoyer les résultats à l'utilisateur
---------------------------

Hourra ! Nous avons les résultats et maintenant il ne reste plus qu'à rendre la page pour l'utilisateur. Le paquet de résultats est renvoyé au serveur, qui adapte les résultats à un modèle préétabli, insère des bannières publicitaires autour de la page et renvoie la page à l'utilisateur. Dans le même temps, il peut également effectuer d'autres opérations auxiliaires, comme la mise en cache des résultats. Si quelqu'un d'autre effectue une recherche pour la même requête dans un avenir proche (par exemple, au cours de la dernière heure), les résultats ne seront pas recherchés à nouveau, mais seulement lus à partir de la mémoire temporaire - ce qui contribue souvent à améliorer les performances globales du moteur de recherche.

Conclusion et aperçu de la sémantique
---------------------------

Cet article avait pour but de décrire les principes de base du fonctionnement interne des moteurs de recherche et la manière dont ils parviennent à retrouver un nombre théoriquement illimité de documents en un temps encore raisonnable. Il s'agit d'énormes optimisations mathématiques qui ont été développées pendant de nombreuses années par les meilleures équipes de programmation.

Une description complète des détails dépasse le cadre de cet article. Le tout est également compliqué par le fait que la plupart des autres étapes ne sont décrites en détail par aucun des moteurs de recherche, car il s'agit de leurs secrets commerciaux.

Il est également important de noter que les moteurs de recherche ne comprennent toujours pas le contenu des pages et la signification des résultats, et qu'il ne s'agit toujours que d'un calcul statistique basé sur la puissance de calcul brute et la capacité des gens à créer des liens vers des textes de qualité, et que ce système est également très vulnérable (prenez l'exemple de la "bombe Google", où un webmaster sournois impose un contenu non pertinent pour une requête de recherche).

Ce problème devrait être en partie résolu par l'intelligence artificielle, que tous les moteurs de recherche tentent progressivement d'intégrer. Toutefois, il s'agit encore d'une musique d'avenir lointain, car ce n'est pas une tâche facile et il s'agit surtout d'améliorer les méthodes de calcul statistique. Prenons l'exemple de la recherche graphique de Google, qui tente de répondre directement à la requête (même si, dans sa structure interne, elle se contente de rechercher dans une base de connaissances prédéfinie).

La prochaine fois que vous poserez une question à votre moteur de recherche préféré, rappelez-vous le travail que celui-ci a dû accomplir et le nombre de To de données qu'il a dû lire sur le disque pour trouver les dix premiers documents correspondant à votre requête.
