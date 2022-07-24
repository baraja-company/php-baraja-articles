Object Immutability - a key design concept
==========================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	en: object-immutability-a-key-design-concept
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

Immutability is one of the most important design concepts for building stable applications. The basic principle states that once a state is written, it can only be read later without the possibility of modifying it. If we need to change the state, we have to create a new instance and replace the whole object with another one.

Data types can therefore be very roughly divided into two broad categories:

- **Mutable** (mutable state within a single instance)
- **Immutable** (immutable internal state)

Mutable objects can be changed internally. That is, they provide operations that, when called in different combinations, cause us to get different results. Immutability tries to prevent this behavior.

Definition
--------

> A class is **immutable** precisely if the instance data cannot be changed in any way after the instance is created.

So all data is fixed in the constructor. All scalar data types are also automatically immutable.

A major advantage
--------------

Designing applications with immutable states provides a fundamental advantage in the safety of executing operations. If we know that once written, the data cannot be changed (mutated) later, we can, for example, debug very reliably, or split the application into sub-functions without the risk of forgetting any of the intermediate states.

The idea of immutability is generally opposed to the principle of storing states in properties in objects/entities, and rather describes a functional approach where data just "flows" through the application, like javascript does for example.

From a performance perspective, we can automatically say of immutable objects that they can be cached indefinitely, because they will never be out of date.

A real example from PHP
--------------------

By far the most common use of immutable objects in PHP is the `DateTimeImmutable` object, which once created can only be called by formatting methods. If we modify the internal settings, the method will return a new instance. This feature is crucial when using an ORM that uses the so-called identity pattern - it allows us, for example, to guarantee that when we read the creation date of an order, it will be the same everywhere in the application and the referential integrity will not be corrupted.

A concrete example of a mutable object:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 day');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

The same date was printed because the `modify()` method only changed the internal state of the `DateTime` object and returned the same instance. Thus, there was no so-called **mutation of internal state**, which is a basic behavior of object-oriented programming. Updating the variable also changed the original one.

And now an example of an immutable object:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 day');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

The `DateTimeImmutable` object is immutable, which means that its internal state never changes. A new modified instance (also immutable) is stored in the variable after the `modify()` method is called. If we did not store the new value in the variable, it would not be usable later.

The original value is never touched and remains safely stored.

When should a class be immutable?
---------------------------

Unless you have a very good reason to make it mutable, always write a class or function as immutable. This will simplify your design in the future.

Mutable classes should change as little as possible. I always recommend documenting the behavior of immutability.

Perhaps the only drawback with immutability is that a new instance must be created with each attribute change, which has a minor performance impact. If your application (like most applications) tends to display data and change it less frequently, this disadvantage is rather insignificant with the performance of today's computers.

What types of data should be immutable?
------------------------------------

- All identifiers and unique codes
- Most ManyToOne and OneToOne database sessions
- Dates, times, calendar values
- Cycled through arrays where we want to do the same operation on each element
- Intervals, pairs, triples, ...
- Geometric figures, points, lines, GPS coordinates, physical addresses, ...
- Logs and historical records
- Information on processed orders and most financial data
- Meta data about the related entity

What should not be immutable:

- Large objects with lots of properties
- Most tabular output from a database, such as Doctrine entities
- Progressively constructed objects from smaller parts
