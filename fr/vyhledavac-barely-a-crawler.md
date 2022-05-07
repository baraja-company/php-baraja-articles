Algorithme des moteurs de recherche Internet - A peine un crawler
=================================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	fr: algorithme-des-moteurs-de-recherche-internet---a-peine-un-crawler
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

Dans la leçon d'aujourd'hui, nous aborderons les barils de données, leur structure, les StopSlovas et enfin nous décrirons les crawlers.

Barils de données
-------------

Il s'agit d'un type de données spécial qui réside sur plusieurs serveurs simultanément en plusieurs copies. Il s'agit généralement de fichiers à forte intensité de données, de plusieurs centaines de Go, qui sont lents à lire (c'est pourquoi ils sont divisés en plusieurs parties) et pratiquement impossibles à modifier. Si nous voulons apporter un changement, même minime, nous devons recalculer l'ensemble du baril. Par exemple, le moteur de recherche Seznam peut recalculer les barils de données au maximum une fois tous les quelques jours ou semaines, alors que Google recalcule une fois toutes les quelques heures (et seulement certaines parties, jamais la totalité en une seule fois).

Les barils contiennent des mots et leurs occurrences sur Internet. On peut dire qu'il s'agit de données brutes pour les résultats de recherche sous une forme non encore triée (parfois ils sont partiellement triés par importance relative) et préparée à l'avance. La recherche proprement dite a lieu bien avant que l'utilisateur ne pose la requête de recherche et c'est ici que tous les résultats sont préparés. La recherche elle-même serait impossible en temps réel, de sorte que le moteur de recherche lit essentiellement les résultats déjà préparés ici (la recherche est effectuée par le composant indexeur, qui sera abordé dans un chapitre distinct).

Les barils contiennent toutes les informations de base nécessaires à l'élaboration de résultats de recherche pour chaque mot présent sur l'Internet. Les problèmes de performance doivent donc être pris en compte lors de la lecture séquentielle. En pratique, les barils ont tendance à être stockés par lots sur plusieurs serveurs et également en mémoire vive pour un accès plus rapide. Par exemple, le moteur de recherche Google ne lit pas du tout les barils sur le disque, mais les conserve entièrement en RAM et n'en garde qu'une copie sur le disque en cas de perte de données en RAM.

Les grands moteurs de recherche disposant de ressources suffisantes conservent également ce que l'on appelle des "tonneaux d'or", qui contiennent les pages les plus importantes de l'internet et sont recherchées en cas de forte charge de recherche. Par exemple, si plusieurs serveurs de recherche tombent en panne, les barils d'or assurent temporairement les recherches. Le moteur de recherche ne trouvera peut-être pas tous les résultats, mais au moins la recherche ne sera pas complètement interrompue, seul le nombre de documents recherchés sera temporairement limité. Il est également nécessaire de mettre à jour les barils régulièrement, car le nombre de pages sur Internet ne cesse d'augmenter. Comme les barils ont généralement une taille de l'ordre du gigaoctet, il n'est pas possible d'effectuer une maintenance incrémentielle, mais il est nécessaire de reconstruire l'ensemble une fois à chaque fois. Par conséquent, le moteur de recherche conserve un baril auxiliaire où toutes les données se présentent sous la forme d'un grand tableau à partir duquel les barils sont construits - par exemple, Google construit des barils environ toutes les 12 heures. Le grand défi est précisément que les barils doivent être au moins partiellement triés et refléter l'état réel de l'Internet. Ainsi, tant que le moteur de recherche ne met pas à jour les barils, l'utilisateur ne trouvera pas de nouvelles pages web.

Structure du baril
----------------

En pratique, un baril est stocké sous la forme d'un grand fichier texte qui contient l'ID de chaque document où le mot recherché apparaît et la position de l'occurrence du mot actuellement recherché.

Un exemple de très petit baril à trois pages :

```txt
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

Cet exemple de baril contient des pages avec les identifiants `1`, `5` et `12`. Les numéros associés indiquent la position de l'occurrence du mot dans le document. Un tel baril ne saisit que les occurrences d'un seul mot, de sorte que chaque mot sur Internet a son propre baril. Lors de la recherche de plusieurs mots-clés, il est nécessaire de lire plusieurs barils (un différent pour chaque mot) puis d'effectuer les opérations selon l'arbre binaire (plus dans les chapitres précédents).

L'intersection se fait généralement en parcourant l'un des documents et en effectuant une recherche dans l'autre. Si les barils sont volumineux, ils risquent de ne pas être lus complètement, de sorte que les moteurs de recherche ne parviennent souvent à en lire qu'une partie et doivent ensuite renvoyer le résultat. Il est donc judicieux de trier les barils au moins partiellement, de sorte que les pages les plus importantes (celles que l'utilisateur est susceptible de rechercher) se trouvent au début du baril et que tout le reste se trouve à la fin du baril.

StopLead
---------

Vous pensez peut-être que pour certains mots, les barils pourraient être vraiment énormes et donc difficiles à travailler. Par exemple, le mot "a" ou le mot "1" se trouvent dans plus de la moitié des documents, et pourtant ils n'ont aucune signification pour le chercheur (parce qu'ils sont rarement recherchés et nécessitent plus de mots environnants). Ces mots sont appelés "mots d'arrêt".

Le problème des mots-clés est qu'ils ne peuvent pas tous être recherchés, et que les moteurs de recherche ne montrent donc qu'une réalité déformée de ce à quoi ressemble réellement l'internet pour le mot recherché.

Un exemple de recherche avec StopSlovy est la requête `Prague 1`.

Le moteur de recherche ne donne que 3 020 000 résultats pour cette requête. En réalité, ces sites sont beaucoup plus nombreux. Cependant, tous ne sont pas recherchés pour des raisons de performance.

Recherchez les options d'accès avec StopSlovy :

- ne pas les indexer du tout et ne pas faire de barils pour eux (les petits moteurs de recherche font cela)
- Les indexer, mais ne stocker que les pages pertinentes dans des tonneaux (c'est ce que font Google et Seznam, par exemple)
- Les indexer comme des mots ordinaires et créer des barils complets (cette solution présente le problème que les barils peuvent facilement dépasser la taille d'un disque dur ordinaire et ne peuvent donc pas être sauvegardés et peuvent prendre des secondes à lire - cette solution n'est donc pas utilisée en pratique ou seulement pour les petits moteurs de recherche spécialisés)

En général, il n'existe pas d'approche universellement correcte et même Google ne l'utilise pas parfaitement. Ce problème est si vaste qu'il pourrait prendre une vie entière à résoudre. Toutefois, aux fins du présent document, cette description générale est suffisante.

Crawler (robot, également araignée)
---------------------------

Un crawler est un programme qui parcourt systématiquement l'internet et garde la trace de quels mots apparaissent sur quelles pages et collecte également des informations auxiliaires (également appelées méta-informations). Un crawler fonctionne généralement sur plusieurs serveurs, car l'exploration de l'internet est exigeante en termes de bande passante et de nombreuses pages doivent donc être explorées simultanément. Google estime que jusqu'à 30 000 pages sont explorées simultanément à un moment donné.

Avant d'ouvrir une page, Crawler consulte une base de données des adresses qu'il prévoit de visiter à l'avenir. S'il est déjà allé à l'adresse, il ne fera que mettre à jour ses enregistrements (il va sur les sites majeurs toutes les quelques heures, sur les sites d'actualité toutes les quelques minutes, sur les sites ordinaires une fois par mois au maximum).

Si le robot arrive à une nouvelle adresse qu'il ne connaît pas, il examinera les liens vers d'autres documents (pages) qu'elle contient. Pour chaque lien, il calcule son importance, et s'il s'agit d'un lien important, il vient aussi sur la page plus tard.

Par exemple, List n'explore de cette manière que quelques niveaux de liens sur chaque site, tandis que Google essaie d'explorer l'ensemble du site, y compris les documents sans importance, ce qui en fait le moteur de recherche le plus complet d'Internet.
Le robot essaie également de classer la page au fur et à mesure qu'il la parcourt et calcule son "rang", qui est une représentation numérique de l'importance de la page sur l'internet. Dans le passé, Google a utilisé le PageRank, qui a une fourchette de 0 à 10. Google ne calcule le PageRank 10 que pour lui-même et pour des sites tels que microsoft.com ou w3c.org. Aujourd'hui, il calcule la valeur différemment (intelligence artificielle et mathématiques vectorielles).

Le PageRank le plus élevé en République tchèque est celui de Seznam.cz, avec une valeur de 7. Ce rang a une grande influence sur la fréquence à laquelle un robot visitera la page et sur la place qu'elle occupera dans les résultats de recherche. Les rangs sont calculés uniquement à partir des backlinks pointant vers la page souhaitée.

Le crawler ne fait rien d'autre avec les données, il les télécharge simplement depuis l'internet et les stocke dans une base de données où elles attendent le processus d'indexation.
