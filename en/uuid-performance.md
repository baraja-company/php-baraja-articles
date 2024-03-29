UUID and large-scale application performance
============================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	en: uuid-and-large-scale-application-performance
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

When the size of the database grows beyond millions of rows, it is advisable to start scaling the application and split the database into multiple physical servers.

The biggest problem of splitting a database into multiple parts is its subsequent synchronization when a user requests specific data.

Why use UUID and what are its advantages over autoincrement
--------------------------------------------------------

Suppose you have a table of `articles`, but because you have a huge site, there are tens of millions more articles and you have to physically split them across multiple machines.

If we were to use an ordinary integer as the `id` (primary key) with the setting as autoincrement, we would very quickly find that when creating records on different machines in a decentralized way and then synchronizing them, there are ID collisions and we have to renumber the records in a complicated way. Additionally, if we are resolving many sessions to other tables, this can be a very complex overhead in which it is easy to make mistakes.

Therefore, instead of a numeric identifier, we can generate a `UUID`, which is a text string that is generated by a complex algorithm that guarantees that it will be unique even if it is generated independently on multiple machines.

Advantages:

- If you have multiple independent databases that you then synchronize, using a UUID means that one ID is unique across all databases, not just the one where you are and where it was generated. When merged into a single cluster, no conflicts arise.
- You can know your `primary key` before actually inserting the record into the database. This reduces the number of SQL queries, simplifies transaction logic, and you can easily use it as a foreign key before the record collection exists.
- The UUID does not disclose information about the number of dates and relationships, and is safer for use in URLs. For example, if I find that I am user `19010018`, it is easy to guess that user `19010017` and others also exist. The attack is called a vector attack.

Generating a new UUID
----------------------

We can get the UUID either simply by SQL query `SELECT UUID();`, but this increases the number of queries to the database and we lose the possibility to prepare the data first in bulk in the application logic and then write them at once.

Therefore, I like to use the <a href="https://github.com/ramsey/uuid">ramsey/uuid</a> package obtained by Composer as a good solution. The UUID itself has several versions, and the package can playfully generate all kinds as needed.

This makes it easy to use:

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Generates version 1 (time-based) UUID object
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Generates version 3 (name-based and hashed as MD5) UUID object
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Generates version 4 (random) UUID object
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Generates version 5 (name-based and hashed as SHA1) UUID object
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

If you use Doctrine, there is an extension <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a> that generates the ID directly as a data type.

Physical storage in the database
---------------------------

In my first attempts I used `varchar(36)` as the primary key (ID), but <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">that's not a good idea at all</a>.

> **Explanation of the internal logic:**
>
> > MySql databases (and many others) can't use `varchar`, `char` or other data types expressing a string as a primary key efficiently.
> In some databases, there is a `GUID` datatype that is designed to store UUIDs directly. If you cannot use this type, there is a suitable substitute in the form `binary(16)`.

When physically examining the database, the ID is then represented in HEX format (since binary format cannot be displayed), instead of the nice ID `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6`, you will just see `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, which looks like `'?kYߟKg2c;'` in the INSERT query.

Convert the original data from `varchar(36)` to `binary(16)`
----------------------------------------------------

I assume you represent (or plan to represent) the newly set ID in the database as:

``sql
`id` binary(16) NOT NULL
```

However, just changing the data type doesn't work, so something like:

```sql
SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;
```

There are basically two reasons for this:

- The primary key and the session to it `must have the same data type`. Therefore, you need to change both the data type for the article ID and, for example, in the relational table that matches articles to authors.
- The binary format contains something slightly different than the original string. You need to use a conversion function.

Therefore, the only correct solution is to back up the data (but you should do this before each migration anyway), prepare an empty database with functional relations and put the data there again via migration.

If you have generated UUIDs strangely before, it is better to choose some sequential method to get the UUID and renumber all records. The reason is that sequential layout allows better ordering of values and creating a `btree`, which makes the performance almost identical to `bigint`.

**If you know of a better way to convert an existing database from UUID stored as a varchar to binary format without having to devise complex migrations and with preservation of foreign keys, I would be very grateful for feedback**.
