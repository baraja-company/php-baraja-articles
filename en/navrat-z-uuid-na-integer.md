Return from UUID to integer
===========================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	en: return-from-uuid-to-integer
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

In software development, a programmer will quite often come to a dead end when faced with an architectural decision that will have a huge impact on the future of his or her work for decades to come. At the same time, it is a decision that cannot be reversed, and there is a dear price to pay for every mistake. Databases are a typical example of an architectural decision where heads roll with every small mistake.

One of the big decisions of recent times has been how to store primary keys in database tables. While this seems like a trivial problem, there is a lot more behind it than you might think.

Primary key options
-------------------------

There are basically four basic options:

- integer
- integer unsigned
- big int
- <a href="/uuid-performance">UUID</a>

Integer is simply an integer (in case of `unsigned` then unsigned, so always positive, and in case of `big int` then it can reach extremely large values). A very simple concept. A UUID is then a text string (for example, of the form `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`) that consists of several parts, each of which can have certain properties and are useful for building huge multiserver or decentralized applications. there is a large ecosystem of useful technologies around UUIDs that solve problems you may not even know you have, or will have in the future.

Use the right hammer
-------------------------

Not long ago (winter 2020) my friend Paul was explaining the concept of applying the appropriate solution to a problem of a given size. This is a great and important idea that many developers like to forget - it creates hugely complex solutions when they don't need to. In English, we have a nice phrase for this **over-engineering**.

UUID size and uniqueness
--------------------------

The fundamental advantage of the UUID is that if the application gets too big and we split the database across many web servers, where one database table is so huge that it won't fit on the disk of one machine, we can split it across many physical machines, each of which will know its own piece of the table and query its colleagues for the rest. UUID also solves the fundamental problem of inserting new rows, when in the case of extremely large applications we need to write rows in parallel in many places and we don't want to wait for the main server's write capacity to free up.

The concept of writing in many places at the same time is used, for example, by chat applications. When you send a message via Messenger, it goes to the nearest Facebook database server, which assigns a UUID and timestamp to the message and writes it to its local database. Your friend on the other side of the world, in turn, writes the messages to his local data center, and in the meantime, the entire cloud infrastructure ensures synchronization across the globe. Sounds cool, right? :)

For such parallel writing to work, the problem of record collision needs to be solved. If the individual local databases use a simple integer, very soon two independent servers will write two different records under the same identifier. When these records are synchronized, a collision will then occur. There is usually no solution - you can't renumber the IDs, because other sessions may lead to that.

UUID solves this by, for example, giving each server an agreed prefix that it inserts at the beginning of each UUID, then inserts a timestamp, then the identifier itself.

> **Interesting fact:** When writing such a huge amount of data, we are not so much interested in when what record was written, but in what order it was written (so that we don't switch the order of messages for the user, for example).

You might ask how to handle a situation where the servers can't agree with each other on which prefix to use. This problem arises, for example, in decentralized or offline applications. In this case, the UUID can even be generated randomly.

The question then is what are the chances that a conflict will occur when <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">randomly generating a UUID</a>. Well, it probably won't happen to you. There are approximately `2^122` unique UUIDs (because it's a `128-bit number`). In practice, the chance of a conflict is approximately `0.00000000006 (6 × 10-11)`. In practice, what this means is that if we generate **1 billion UUIDs** every second for the next 100 years, the chance of conflict will be `50%`. So conflict is more likely not to occur, and UUID is the definitive solution to your database problems.

Is there a need for such a robust solution?
-------------------------------

If you don't know, the answer is **no**.

With the primary key set to `int` with the `unsigned` flag, there are `4,294,967,295` possible values (4 billion). For a comparison of integer sizes, see the <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">MySql documentation</a>.

By the time you store 4 billion records in one table, you will probably run out of disk space sooner.

Integer and join performance
----------------------

Integers are really very fast. There are native optimizations for them in MySql. Indexes work properly (plus they are much smaller), they only take 4 bytes, they are very fast to join, and they will be fine for most cases.

If you're dealing with a database replication problem, the best solution might be to put the entire database in the cloud like MS Azure and query it externally. Even when storing tens of millions of records, the speed of access via integer to a specific row is in the order of milliseconds (under 3 ms on a well-configured server), and with a clustered index, the time can be well sustainable even with a large number of requests.

If you really need to use UUIDs, you'd better leave the MySql world and go the Postgres database route, which unlike MySql has its own data type for <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUIDs</a>. Working with joins is a huge problem with UUIDs and MySql, and when joining just 3 tables (with each having only sub tens of thousands of records), the entire query can take several hundred milliseconds to low seconds to process. And this is unfortunately a MySql problem that you probably can't solve.
