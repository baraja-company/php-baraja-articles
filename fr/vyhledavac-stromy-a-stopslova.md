Algorithme des moteurs de recherche sur Internet - Arbres et StopLead
=====================================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	fr: algorithme-des-moteurs-de-recherche-sur-internet---arbres-et-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

Chaque seconde, 5 millions de nouvelles pages sont ajoutées à l'internet, et ce taux ne cesse d'augmenter. Pour mettre de l'ordre dans cette immense mer d'informations et y trouver quelque chose, il existe des moteurs de recherche. Le travail suivant vise à introduire la question de la recherche et à expliquer l'ensemble du processus, de la création d'une nouvelle page à sa recherche dans un moteur de recherche.

La tâche de trouver et de trier un ensemble de milliards de documents n'est pas facile. À lui seul, Google a besoin de 300 000 serveurs web pour accomplir cette tâche en quelques heures. En fait, la recherche de votre question a lieu bien avant que vous ne la posiez. Google a déjà en mémoire les résultats de recherche que vous allez demander dans les prochains jours.

Architecture des moteurs de recherche
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Architecture commune de moteur de recherche pour un maximum de 50 millions de documents" class="w-100 mb-3">

Un moteur de recherche correctement conçu contient de nombreux composants, dont le nombre est de l'ordre de plusieurs centaines. Ce diagramme décrit les éléments les plus fondamentaux qu'un moteur de recherche doit contenir au minimum pour fonctionner. Il s'agit d'une architecture ancienne qui n'est plus utilisée et qui a fonctionné jusqu'en 2005 environ, alors que le web ne comptait que quelques millions de documents. Cette architecture ne permet pas de disposer d'une quantité "infinie" de contenu, ni d'une éventuelle indisponibilité des serveurs de recherche (recherche de base). En cas de panne d'un seul composant, l'ensemble du système cesserait de fonctionner et le moteur de recherche serait indisponible.

Une description complète des tendances actuelles en matière de conception de nouvelles architectures dépasse le cadre de cet article, car il s'agit généralement de secrets commerciaux de grandes entreprises. L'objectif de ce document sera d'expliquer le fonctionnement de cette architecture de base. Les différents composants sont expliqués en détail dans les chapitres suivants.

Entrée de la requête
------------

L'élément le plus visible, même pour les utilisateurs ordinaires, est la saisie de la requête, qui est actuellement le plus souvent représentée par une zone de texte. Le champ de saisie est situé sur une page spéciale qui est généralement réservée à la saisie.

Dans l'étape suivante, l'utilisateur saisit la chaîne de recherche et soumet le formulaire aux serveurs du moteur de recherche, initiant ainsi la communication avec le composant de distribution, parfois aussi appelé recherche principale. Les serveurs de distribution sont construits pour gérer d'énormes charges d'utilisateurs et doivent être capables de traiter tous les chercheurs en même temps.

L'architecture d'un serveur de répartition est généralement un simple serveur Apache avec une installation de base et une excellente bande passante réseau. Lorsqu'une requête entre dans le serveur, elle est traitée par des arbres de décision de régression et une "carte de requête" est créée qui capture la sémantique complète autour des autres significations pour rendre clair ce que l'utilisateur recherche réellement. Les méthodes de réécriture de requêtes dépassent largement le cadre de cet article. Nous nous contenterons donc de présenter des descriptions générales de ce à quoi la réécriture peut et ne peut pas ressembler.

Considérons la requête suivante :

```txt
"O pejskovi a kočičce"
```

Cette requête sera convertie en un arbre binaire qui en capture l'essence sémantique :

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Diagramme symbolique de l'arbre d'analyse" class="w-100 mb-3">

L'objectif d'un arbre d'analyse est de capturer la manière dont un moteur de recherche examinera les résultats de recherche. Cet arbre, ainsi que la requête, est envoyé aux serveurs de recherche auxiliaires (appelés recherche de base dans le présent document), où les mots-clés individuels sont recherchés (lecture de l'index), puis triés selon des règles (par opérations).

| Opérateur | Opérations de tri |
|----------|------------------
| ET | Intersection |
| OR | Somme |
| NOT | Complément |

Il n'y a pas de tri réel des résultats de la recherche, ce composant se contente d'effectuer des opérations binaires rapides sur d'énormes quantités de données (souvent des millions d'enregistrements) et se contente essentiellement de comparer les ID des documents individuels.

Le fonctionnement du tri des résultats de recherche pour la page de résultats sera abordé dans les sections suivantes. Le serveur de sortie est simplement utilisé pour combiner un grand nombre de données provenant de différents serveurs de recherche afin d'augmenter les performances globales et de répartir la charge, puis il introduit les valeurs détectées dans la page de résultats (c'est ce qu'on appelle une "page web"), qui contient des informations composites provenant de dizaines de serveurs.

Réécriture en arbre binaire
-----------------------

Comme nous l'avons mentionné dans le chapitre précédent, la recherche entière est construite sur des arbres binaires qui vous indiquent à quoi ressembleront les résultats et ce qu'il faut rechercher. Toute la "logique" et l'"intelligence" d'un moteur de recherche dépendent directement de la qualité du programme qui crée l'arbre, à savoir le descripteur.

Un arbre correctement construit doit contenir toutes les méta-informations nécessaires sur la requête sous une forme telle qu'il peut être facilement traité, même à l'aide d'une lecture séquentielle du disque, et doit éliminer les accès aléatoires à la mémoire. Il contient donc généralement les mots individuels recherchés (par l'utilisateur), leurs transcriptions (et leurs formes épelées) et, enfin et surtout, les liens entre les mots, c'est-à-dire leurs distances relatives dans le texte.

Méthodes de transcription des requêtes
---------------------

La façon la plus élémentaire de transcrire une requête est de l'analyser en mots individuels (en fonction des espaces), qui seront travaillés par la suite. La deuxième étape consiste à rechercher les mots d'arrêt et à les remplacer par un pointeur de variable (car, comme nous le montrerons plus tard, il est inutile de rechercher les mots d'arrêt).

Prenons la requête suivante : "A propos d'un chien et d'un chat", sa transcription dans son état le plus élémentaire ressemble à ceci :

```txt
"O (and) pejskovi (and) a (and) kočičce"
```

La deuxième étape consiste à éliminer les mots qui n'ont aucun rapport avec la recherche. Bien sûr, par exemple, le mot "about" ou "and" est significatif pour nous, les humains, mais pas pour une recherche dans une base de données, car il peut être remplacé par un autre mot et les résultats listés. L'objectif ne doit pas être de trouver une correspondance littérale de la requête par rapport à l'index, car cela conduirait à de mauvais résultats, car souvent les mots dans le texte sur Internet ont des orthographes différentes, et cette méthode essaie de trouver des résultats sous d'autres formes que celles que nous avons obtenues de l'utilisateur. Il s'agit donc de la première indication d'un moteur de recherche "intelligent".

Ces mots sans signification sont souvent appelés "stopwords", chaque moteur de recherche devrait conserver un index de ces mots et cet index devrait être mis à jour fréquemment (manuellement).

```txt
["pejskovi (and) * (and) kočičce"]
```

À ce stade, nous recherchons 2 mots différents ("chien" et "chat") avec un mot supplémentaire entre les deux (en interne, nous le marquons d'un astérisque).

Le but de cette modification n'est pas d'augmenter la qualité de la recherche, mais d'accroître les performances. En effet, lorsque nous recherchons un mot trop fréquent, il y a une charge rapide, et cette modification aide le processus - notamment en ne recherchant pas un mot qui sera de toute façon disponible à cette position dans la plupart des documents.

Il est également important de noter qu'il n'est pas toujours possible d'effectuer cet ajustement (omettre un mot). Chaque moteur de recherche devrait donc conserver un index distinct des phrases trouvées sur Internet afin de vérifier quel ajustement permet d'obtenir de bons résultats et lequel ne le permet pas. Cependant, il arrive qu'un moteur de recherche procède à cet ajustement même s'il n'a pas l'expression en question dans son index - notamment lorsqu'un utilisateur recherche un mot important qui devrait avoir des pages importantes dans les premières positions, lesquelles annulent cette "erreur" parce que les utilisateurs les demandent de toute façon.

La dernière étape consiste à travailler avec la langue anglaise et à "nettoyer" la requête des caractères inutiles. Par exemple, pour la requête "machine à laver", l'utilisateur s'attend généralement à obtenir des résultats uniquement sur le mot "machine à laver" et nous pouvons ignorer le caractère virgule.

Les méthodes exactes de fonctionnement de ce système ne sont pas connues et chaque moteur de recherche a le sien. On pense qu'il s'agit surtout d'un modèle statistique qui effectue ces ajustements en se basant sur la connaissance des milliards de textes de la base de données.

La transcription anglaise est aussi une science en soi, aussi seule une description de base sera donnée ici également. Dans la plupart des cas, il suffit d'ajouter les formes citées (selon le dictionnaire) à chaque mot et de les rechercher avec le mot, ce qui a pour effet que le moteur de recherche peut trouver des documents où les mots n'apparaissent pas seulement dans leur forme de base (telle que saisie par l'utilisateur), mais il peut également trouver les versions infléchies - une fonctionnalité très utile. Le problème de cette approche réside dans l'inflexion de mots obscurs et problématiques, ainsi que dans l'inflexion de requêtes courtes (et donc il n'est pas possible de déterminer à partir du contexte comment infléchir le mot). Le mot "jeux" est un exemple d'inflexion problématique.

Étant donné une requête en anglais, "I love her", un inflecteur mal conçu infléchira le mot "games" comme, par exemple, "games, gaming, gaming room, ...", il est donc également nécessaire d'indexer les mots environnants comme des phrases et de n'effectuer l'inflexion que dans des situations familières, ou d'utiliser une procédure différente (c'est là que les algorithmes phonétiques interviennent souvent).

La dernière étape consiste à envoyer l'arbre terminé aux serveurs de recherche de la base, où la recherche effective de barils aura lieu - c'est le sujet du prochain chapitre.
