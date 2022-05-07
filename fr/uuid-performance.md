UUID et performances des applications à grande échelle
======================================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	fr: uuid-et-performances-des-applications-a-grande-echelle
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

Lorsque la taille de la base de données dépasse les millions de lignes, il est conseillé de commencer à mettre l'application à l'échelle et de répartir la base de données sur plusieurs serveurs physiques.

Le plus gros problème de la division de la base de données en plusieurs parties est sa synchronisation ultérieure si l'utilisateur demande des données spécifiques.

Pourquoi utiliser l'UUID et quels sont ses avantages par rapport à l'auto-incrément ?
--------------------------------------------------------

Supposons que vous ayez une table de `articles`, mais comme votre site est énorme, il y a des dizaines de millions d'articles supplémentaires et vous devez les répartir physiquement sur plusieurs machines.

Si nous devions utiliser un nombre entier ordinaire comme "id" (clé primaire) avec un paramètre comme l'auto-incrément, nous nous apercevrions très vite que lorsque nous créons des enregistrements sur différentes machines de manière décentralisée et que nous les synchronisons ensuite, il y a des collisions d'ID et nous devons renuméroter les enregistrements de manière compliquée. En outre, si nous résolvons de nombreuses sessions vers d'autres tables, il peut s'agir d'une opération très complexe dans laquelle il est facile de faire des erreurs.

Par conséquent, au lieu d'un identifiant numérique, nous pouvons générer un `UUID`, qui est une chaîne de texte générée par un algorithme complexe qui garantit qu'elle sera unique même si elle est générée indépendamment sur plusieurs machines.

Avantages :

- Si vous disposez de plusieurs bases de données indépendantes que vous synchronisez ensuite, l'utilisation d'un UUID signifie qu'un identifiant est unique dans toutes les bases de données, et pas seulement dans celle où vous vous trouvez et où il a été généré. Lorsqu'ils sont fusionnés en un seul cluster, aucun conflit ne se produit.
- Vous pouvez connaître votre "clé primaire" avant d'insérer l'enregistrement dans la base de données. Cela réduit le nombre de requêtes SQL, simplifie la logique de transaction et vous pouvez facilement l'utiliser comme clé étrangère avant que la collection d'enregistrements n'existe.
- L'UUID ne divulgue pas d'informations sur le nombre de dates et de séquences et est plus sûr pour une utilisation dans les URL. Par exemple, si je constate que je suis l'utilisateur `19010018`, il est facile de deviner que l'utilisateur `19010017` et d'autres existent également. Cette attaque est appelée attaque vectorielle.

Génération d'un nouvel UUID
----------------------

L'UUID peut être obtenu par une simple requête SQL `SELECT UUID();`, mais cela augmente le nombre de requêtes à la base de données et nous perdons la possibilité de préparer les données en vrac dans la logique applicative et de les écrire en une seule fois.

Par conséquent, j'aime utiliser le paquet <a href="https://github.com/ramsey/uuid">ramsey/uuid</a> obtenu par Composer comme une bonne solution. L'UUID lui-même a plusieurs versions, et le paquet peut s'amuser à en générer toutes sortes selon les besoins.

Cela le rend facile à utiliser :

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Génère un objet UUID version 1 (basé sur le temps)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Génère un objet UUID version 3 (basé sur le nom et haché en MD5).
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Génère un objet UUID version 4 (aléatoire).
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Génère un objet UUID version 5 (basé sur le nom et haché en SHA1).
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Si vous utilisez Doctrine, il existe une extension <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a> qui génère l'ID directement en tant que type de données.

Stockage physique dans la base de données
---------------------------

Dans mes premiers essais, j'ai utilisé `varchar(36)` comme clé primaire (ID), mais <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">ce n'est pas du tout une bonne idée</a>.

> **Explication de la logique interne:**
>
> > Les bases de données MySql (et beaucoup d'autres) ne peuvent pas utiliser efficacement `varchar`, `char` ou d'autres types de données exprimant une chaîne de caractères comme clé primaire.
> Dans certaines bases de données, il existe un type de données `GUID` qui est conçu pour stocker directement les UUID. Si vous ne pouvez pas utiliser ce type, il existe un substitut approprié sous la forme `binary(16)`.

Lors de l'examen physique de la base de données, l'ID est alors représenté au format HEX (puisque le format binaire ne peut pas être affiché), au lieu du bel ID `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6`, vous verrez juste `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, qui ressemble à '?kYߟKg2c;'` dans la requête INSERT.

Conversion des données originales de `varchar(36)` en `binary(16)`.
----------------------------------------------------

Je suppose que vous représentez (ou prévoyez de représenter) l'ID nouvellement défini dans la base de données comme :

```sql
`id` binary(16) NOT NULL
```

Cependant, le simple fait de changer le type de données ne fonctionne pas, donc quelque chose comme :

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

Il y a essentiellement deux raisons :

- La clé primaire et la session qui lui correspond `doivent avoir le même type de données`. Par conséquent, vous devez modifier à la fois le type de données pour l'ID de l'article et, par exemple, dans la table relationnelle qui associe les articles aux auteurs.
- Le format binaire contient quelque chose de légèrement différent de la chaîne originale. Vous devez utiliser une fonction de conversion.

Par conséquent, la seule solution correcte consiste à sauvegarder les données (mais vous devriez de toute façon le faire avant chaque migration), à préparer une base de données vide avec des relations fonctionnelles et à y replacer les données via la migration.

Si vous avez déjà généré des UUID de façon étrange, il est préférable de choisir une méthode séquentielle pour obtenir l'UUID et renuméroter tous les enregistrements. La raison est que la disposition séquentielle permet de mieux ordonner les valeurs et de créer un `btree`, ce qui rend les performances presque identiques à celles de `bigint`.

**Si vous connaissez une meilleure façon de convertir une base de données existante d'un UUID stocké en tant que varchar à un format binaire sans avoir à concevoir des migrations complexes et en préservant les clés étrangères, je vous en serais très reconnaissant**.
