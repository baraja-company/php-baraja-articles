Retour de l'UUID à l'entier
===========================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	fr: retour-de-l-uuid-a-l-entier
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

Dans le développement de logiciels, un programmeur se retrouve souvent dans une impasse lorsqu'il est confronté à une décision architecturale qui aura un impact considérable sur l'avenir de son travail pendant des décennies. En même temps, il s'agit d'une décision irréversible et le prix à payer pour chaque erreur est élevé. Les bases de données sont un exemple typique de décision architecturale où les têtes tombent à chaque petite erreur.

L'une des grandes décisions de ces derniers temps a été de savoir comment stocker les clés primaires dans les tables de base de données. Bien que cela semble être un problème trivial, il y a beaucoup plus derrière que vous ne le pensez.

Options de clé primaire
-------------------------

Il existe essentiellement quatre options de base :

- nombre entier
- entier non signé
- grand int
- <a href="/uuid-performance">UUID</a>

Integer est simplement un nombre entier (dans le cas de `unsigned` alors non signé, donc toujours positif, et dans le cas de `big int` alors il peut atteindre des valeurs extrêmement grandes). Un concept très simple. Un UUID est alors une chaîne de texte (par exemple, de la forme `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`) qui se compose de plusieurs parties, chacune d'entre elles pouvant avoir certaines propriétés et étant utile pour construire d'énormes applications multiserveurs ou décentralisées. Il existe un large écosystème de technologies utiles autour des UUIDs qui résolvent des problèmes que vous ne savez peut-être même pas que vous avez, ou que vous aurez à l'avenir.

Utilisez le bon marteau
-------------------------

Il n'y a pas longtemps (hiver 2020), mon ami Paul expliquait le concept d'application de la solution appropriée à un problème d'une taille donnée. Il s'agit d'une idée géniale et importante que de nombreux développeurs aiment oublier - cela crée des solutions extrêmement complexes alors que ce n'est pas nécessaire. En anglais, nous avons une belle expression pour cette **over-engineering**.

Taille et unicité de l'UUID
--------------------------

L'avantage fondamental de l'UUID est que si l'application devient trop importante et que nous répartissons la base de données sur de nombreux serveurs web, où une table de base de données est si énorme qu'elle ne tient pas sur le disque d'une seule machine, nous pouvons la répartir sur de nombreuses machines physiques, chacune d'entre elles connaissant sa propre partie de la table et interrogeant ses collègues pour le reste. L'UUID résout également le problème fondamental de l'insertion de nouvelles lignes, lorsque, dans le cas d'applications extrêmement volumineuses, nous devons écrire des lignes en parallèle à de nombreux endroits et que nous ne voulons pas attendre que la capacité d'écriture du serveur principal se libère.

Le concept d'écriture à plusieurs endroits en même temps est utilisé, par exemple, par les applications de chat. Lorsque vous envoyez un message via Messenger, il est transmis au serveur de base de données Facebook le plus proche, qui attribue un UUID et un horodatage au message et l'écrit dans sa base de données locale. Votre ami à l'autre bout du monde écrit à son tour les messages dans son centre de données local et, pendant ce temps, l'ensemble de l'infrastructure en nuage assure la synchronisation à travers le monde. Ça a l'air cool, non ? :)

Pour qu'une telle écriture parallèle fonctionne, le problème de la collision des enregistrements doit être résolu. Si les bases de données locales individuelles utilisent un simple entier, très vite, deux serveurs indépendants écriront deux enregistrements différents sous le même identifiant. Lorsque ces enregistrements sont synchronisés, une collision se produit alors. Il n'y a généralement pas de solution - vous ne pouvez pas renuméroter les ID, car d'autres sessions peuvent y conduire.

UUID résout ce problème en donnant, par exemple, à chaque serveur un préfixe convenu qu'il insère au début de chaque UUID, puis insère un horodatage, puis l'identifiant lui-même.

> **Fait intéressant:** Lors de l'écriture d'une telle quantité de données, nous ne sommes pas tant intéressés par le moment où l'enregistrement a été écrit, mais dans quel ordre il a été écrit (afin de ne pas intervertir l'ordre des messages pour l'utilisateur, par exemple).

Vous pouvez vous demander comment gérer une situation dans laquelle les serveurs ne parviennent pas à se mettre d'accord sur le préfixe à utiliser. Ce problème se pose, par exemple, dans les applications décentralisées ou hors ligne. Dans ce cas, l'UUID peut même être généré de manière aléatoire.

La question est donc de savoir quelles sont les chances qu'un conflit se produise lors de la génération aléatoire d'un UUID</a> par <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">. Eh bien, ça ne vous arrivera probablement pas. Il y a environ `2^122` UUIDs uniques (parce que c'est un `128-bit number`). Dans la pratique, la probabilité d'un conflit est d'environ `0,00000000006 (6 × 10-11)`. En pratique, cela signifie que si nous générons **1 milliard d'UUID** chaque seconde pendant les 100 prochaines années, le risque de conflit sera de `50%`. Il est donc plus probable qu'un conflit ne se produise pas, et l'UUID est la solution définitive à vos problèmes de base de données.

Une solution aussi robuste est-elle nécessaire ?
-------------------------------

Si vous ne le savez pas, la réponse est **non**.

Avec la clé primaire définie sur `int` avec le drapeau `unsigned`, il y a `4,294,967,295` valeurs possibles (4 milliards). Pour une comparaison des tailles des entiers, voir la <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">documentation MySql</a>.

Si vous stockez 4 milliards d'enregistrements dans une table, vous serez probablement à court d'espace disque plus tôt.

Performances des nombres entiers et des jointures
----------------------

Les nombres entiers sont vraiment très rapides. Il existe des optimisations natives pour eux dans MySql. Les index fonctionnent correctement (et sont beaucoup plus petits), ils n'occupent que 4 octets, ils sont très rapides à joindre, et ils conviennent à la plupart des cas.

Si vous êtes confronté à un problème de réplication de la base de données, la meilleure solution pourrait être de placer l'ensemble de la base de données dans le nuage, comme MS Azure, et de l'interroger en externe. Même en stockant des dizaines de millions d'enregistrements, la vitesse d'accès par entier à une ligne spécifique est de l'ordre de la milliseconde (moins de 3 ms sur un serveur bien configuré), et avec un index en cluster, le temps peut être bien supporté même avec un grand nombre de requêtes.

Si vous avez vraiment besoin d'utiliser des UUID, il vaut mieux quitter le monde de MySql et emprunter la voie de la base de données Postgres, qui, contrairement à MySql, possède son propre type de données pour <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUIDs</a>. Le travail avec les jointures est un énorme problème avec les UUIDs et MySql, et lorsque l'on joint seulement 3 tables (avec chacune ayant seulement quelques dizaines de milliers d'enregistrements), la requête entière peut prendre plusieurs centaines de millisecondes à quelques secondes à traiter. Et c'est malheureusement un problème MySql que vous ne pouvez probablement pas résoudre.
