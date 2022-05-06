Basic philosophy of object-oriented programming
===============================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slug:
> 	cs: filosofie-oop
> 	en: basic-philosophy-of-object-oriented-programming
> 
> publicationDate: '2020-01-02 22:21:56'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3841046b40a039413bacd045f275c68

Object-oriented programming is a paradigm, a view of how to program. You'll soon see for yourself that OOP brings a pretty fundamental simplification to all the common problems and difficulties that are solved over and over again in real programming.

The basic idea
-----------------

The basic idea of object-oriented programming is to divide a large application (a complex task) into many small parts that we can solve elegantly and independently.

For example, if we are programming a reservation system for airline tickets, it is a very complex project that solves thousands of cases. If we can decompose the whole complex logic into many layers and parts, we can easily understand the whole complex system and program the individual subtasks independently.

Practical advantages of OOP and the division of code into objects
------------------------------------------------

Apart from the academic viewpoint, there are many practical reasons to use OOP:

- The application will create a <a href="/encapsulation">encapsulation</a>, which means that data is always processed in a local context, which is few, so we don't have to think about the whole application at once.
- We can split an application into hundreds of thousands of small parts that can be developed by different people in parallel and just arrange a common interface.
- We can look at the application from the perspective of different layers (abstractly at whole modules, or locally at specific functions solving a specific algorithm).
- When debugging the application (debugging) we are able to step the application, omit or replace some parts.
- We can better apply **design patterns** and thus prevent errors and complexities in the code before they occur.

Personally, I can't imagine teams with more than one person programming any differently.

> **Note:**
>
> Using objects puts a little more memory and CPU overhead on the computer, so this will reduce application performance a little. In a real-world environment, however, this doesn't matter, because reprogramming the application to objects does lose some performance (usually units of percent), but it saves programmers time (usually tens to hundreds of percent). Human time is always much more expensive (and very limited) than computer time.
>
> > Don't forget also that OOP brings great simplification to the whole application and allows large applications to be completed in a reasonable amount of time. A large number of complex applications would be almost impossible to program without objects.

Relationship to the real world
-------------------------

The basic goal of OOP in software design is to simulate as much as possible the properties, behaviors, and principles of the real world. Objects in OOP represent real entities. This way of thinking allows us to build huge complex systems that can be well understood, solve real-world problems internally as they would be solved without a computer, and the principles can be explained to real people.

For example, if we are implementing a content management application, it makes sense to lay out all the internal logic in many real entities (article, author, category) and to build sessions not according to artificially generated record IDs (as is conventionally done in databases) but according to real relationships.

Example of concrete implementation:

```php
class Article
{
    private Author $author;

    /** @var Category[] */
    private array $categories;

    private string $title;
}

class Author
{
    private string $name;
}

class Category
{
    private string $name;
}
```

As we can see, the `Article` class does not only contain technical parameters, such as the ID of the record with the author, but it is a real binding to the Author entity, which again has its own properties.

For an explanation of the specific implementation and syntax, see the <a href="/uvod-do-oop">introductory tutorial</a>.
