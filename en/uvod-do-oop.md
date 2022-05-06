Introduction to object-oriented programming in PHP
==================================================

> id: '44a79461-dcfd-4dd0-b6fd-ba5db76db6de'
> slug:
> 	cs: uvod-do-oop
> 	en: introduction-to-object-oriented-programming-in-php
> 
> publicationDate: '2020-01-01 19:54:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '888db2cf35845892eebade9fdcc18871'

Welcome to the first article of the online course OOP in PHP. For a complete list of articles, visit the <a href="/oop">overview page</a>.

> **Content notes:**
>
> The goal of this series is to **best explain the essence** of object-oriented programming so that you don't have to spend hundreds of hours experimenting on things that don't make sense. All the techniques and tutorials are from practice and are explained as I would have wanted to read them myself when I was learning to program. Some **concepts are greatly simplified** at the expense of 100% factual correctness, because I believe it's always **better to understand mainly the principles** and have at least some idea of the issues than to get lost in programming and see everything as one big mess.
>
> As you will soon see, programming in OOP has huge benefits for your code. It will bring a new level of abstraction over your application and allow you to solve very complex problems that would not be possible any other way.

What are objects
------------------

In the basic concept of programming, objects are special data types that, in addition to their own value, can define detailed state, methods, behavior, and allow you to perform various operations, as in the real world.

An object in programming is something like a real-world "thing" that we can name (e.g., by a noun) and has properties like a real-world thing. For example, an object can be an article that has some values (title, content, author, publication date, ...) and methods (insert new title, read content, calculate number of days since publication, ...). In this article, we will mainly deal with how to create an object and work with it at a basic level.

Objects and classes
---------------

As we've already established, an **object is something that exists**. The word `exists` is very important at this stage, because as we will soon see, there is still a practical implementation to deal with in programming.

Because in programming we are writing code and an object is something `living', from this point on we will distinguish between two different concepts, for which it is important to understand the meaning in detail and to distinguish it strictly:

- An **object** is something "alive" that exists right now (has an instance), and can execute something or react to other objects.
- A **class** is statically written code that has yet to be evaluated and remains the same all the time.

Since we always write code as a static string (text) when programming, we will distinguish programs into `classes` (static definition of the whole object) and `objects` (a `live` instance of the class).

Example of a class definition:

``php
class Article
{
    public string $title;

    public ?string $author = null;
}
```

> **Practical notes:**
>
> In a real application, each class is usually written to a separate PHP file that is named the same as the class name.
>
> So for the `Article` class, for example, we create `src/Article.php`. This convention is not required in PHP, but it helps make the application more readable so you don't have a lot of code together in one place.
>
> Each class can exist at most once in the whole application (have a unique name), but there can be (almost) infinitely many instances (depending on RAM capacity). Furthermore, a single class cannot be split into multiple files and must be defined in a single place at once. If you need to create multiple classes with the same name, use **namespaces**.
>
> We will also show later that well-structured code will allow it to be reused across many projects.

This code defines an `Article` class with two properties (`title` and `author`). Comments are ignored by PHP and are only used for static type checking (typically read by PhpStorm).

Code:

```php
public string $title;
```

means the definition of `properties`, which is a property of the `Article` class. We can think of this as `Article` being a kind of array and `title` being its index, where we can write a value. Each property must have an accessibility defined (`public`, `protected` or `private`), we will explain later what each setting means. If you don't know what accessibility to choose in a particular case, put `public`.

Instantiating a class and creating an object
----------------------------------

The concept of `instance` is one of the most complex concepts to understand, so it is important that you read the following lines very carefully and I recommend that you give all the examples a fair try.

If our application has a class already defined in the source code, it is not actually used anywhere and PHP does not know about it. This is because OOP introduces the so-called `data encapsulation principle`, which means that a class has an internal (local) context that is valid only inside the class. This is because in object-less development, we were used to always evaluating the code in its entirety. Also, try to think of a class as some form of function or externally called program.

Now we can create our first instance, using the new `new` operator to do so.

```php
$myArticle = new Article;
$myArticle->title = 'My first article'; // writes the value

echo $myArticle->title; // prints "My first article"
```

Note that we put the entire `Article` object created by the `new` operator into the `$myArticle` variable. This means to us that the object is actually a `data type`, so we can move it across variables and continue working with it.

The `->` arrow operator is used to manipulate the object. If we have an instance of a particular object (we have a variable with its contents), we can easily write values to the internal properties, or read them or change them in various ways using methods (we will show later).

The **Instance of an object is the creation of a local "live" copy of the class in memory (a variable).**

Creating multiple instances of the same class at the same time
---------------------------------------------

As we have already discussed, each object has a local context that applies in its internals. Thanks to this principle, we can create many independent instances of the same class and manipulate them freely. We can even create a dynamic number of instances and, for example, store them as array elements. The possibilities are endless.

> **Warning:**
>
> When creating an extreme number of instances (hundreds or more), keep in mind the memory footprint, as PHP needs to keep information about all instances in RAM. In practice, however, the problem of memory overflow due to many instances does not occur.

Example:

```php
$firstArticle = new Article;
$firstArticle->title = 'My first article';

$secondArticle = new Article;
$secondArticle->title = 'About a dog and a cat';

echo $firstArticle->title; // prints "My first article"
echo $secondArticle->title; // prints "About dog and cat"
```

Note that the output of the `title` property (`->title`) depends on what instance we are currently reading from.

Summary
-------

We've shown how to define our first class and create an instance of it (an object) into which we've written several values.

In the next installment, we'll explain the concept of <a href="/methods-and-passing-input">Constructor, Methods and Passing Input</a>.
