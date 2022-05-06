Methods in PPE and input transfer
=================================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	en: methods-in-ppe-and-input-transfer
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

Methods represent the behavior of an object because they allow you to work with its internal state as well as to influence objects with each other.

Representing methods in the real world
----------------------------------

Consider any real-world object, say a cat. The cat has certain properties (name, color, weight, ...) which we describe using properties and then also behavior (meowing, walking, sleeping, ...) and we describe this using methods. Since there are many cats in the world ("And so I bark like thunder, let there be a million cats") it is important to remember that a method is something general that applies to all objects of a given type equally.

Practical implementation of a method
-----------------------------

In terms of language syntax, let's define a class for a cat:

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';
}
```

After creating an instance of a particular cat, we can simply list the sound, for example:

```php
$cat = new Cat;

echo $cat->sound; // prints "Meow"
```

But what if we want to format the sound in some way when it is output? Then the following methods come into play:

```php
class Cat
{
    public string $name;

    public string $sound = 'Meow';

    public function getFormattedSound(): string
    {
        return 'I'm making a sound' . $this->sound . '"!';
    }
}
```

You probably already know the principle of functions from the past. Classes allow a function to be written directly into its body with defined accessibility (as with properties) and are called methods.

The `$this` variable behaves a bit "magically". It stores the current instance of the object we are currently in. If we want to read a value from a property or call another method within a method, we just need to do it over the `$this` variable.

The method is then called from inside the object as a classic function (with a parenthesis at the end) to say that it is not a property:

```php
$cat = new Cat;

echo $cat->sound; // prints "Meow"

echo $cat->getFormattedSound(); // prints 'I'm making a "Meow" sound!'
```

Constructor - method called when creating an instance
--------------------------------------------------

When creating an object instance, we often need to define how to set its basic state and which parameters (input data) are mandatory.

In OOP to solve this problem there is a special public method `__construct` that we can implement voluntarily and it is always and only called when creating an instance.

Practical example:

```php
class Cat
{
    public string $name;

    public string $sound;

    public function __construct(string $name, string $sound)
    {
        $this->name = $name;
        $this->sound = $sound;
    }

    public function getFormattedSound(): string
    {
        return 'I'm making a sound "' . $this->sound . '"!';
    }
}
```

By defining the constructor, we have made sure that when creating an instance, we always have to pass 2 mandatory parameters (`name` and `sound`) and the object will be set directly.

Since the constructor is a classical method, it can do something else inside with the inserted data. For example, reformat the string appropriately, but we'll deal with that later.

Creating an instance is then easy again, we just need to pass the initial parameters (they are written in parentheses to the class name where the class name is called and the instance is created):

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // Prints "Minda"
echo $cat->sound; // prints "Vrr"

echo $cat->getFormattedSound(); // prints 'I'm making the "Vrr" sound!'
```

Getters and setters
-----------------

Some methods are called `getters` and `setters`. These are common methods, it's just a convention to call them that. Methods are used to get and insert data into an object. The main advantage is that we can validate the data before inserting it and possibly correct it or reject the data and throw an error.

For each property that can be retrieved (we want to enable data retrieval), it is customary to create a getter starting with the word `get`. If the property returns a boolean (`true` or `false`), it is customary to name the beginning of the method name with the word `is`.

Simplified example:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->setName($name);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function isEmpty(): bool
    {
        return $this->name === '';
    }

    public function setName(string $name): void
    {
        $this->name = trim($name);
    }
}
```

When we create a cat instance, we pass its name to the constructor (which is always mandatory). However, since we want to share the logic for inserting the name for both the constructor and the setter (the method for inserting a new name), we call the `setName()` method inside the constructor to ensure that the name is inserted.

Inside the setter, we use the <a href="/function-trim">trim()</a> function, which automatically removes whitespace from both sides of the inserted string.

We can use the `getName()` method for the output. The `isEmpty()` method can be used to check for emptiness.

> **Practical advantages:**
>
> A huge advantage of getters and setters are **guaranteed data types**. If a getter claims to return a `string`, we can always rely on it and always get a string.
>
> A setter in its implementation also claims to accept a `string`, which means for the application that whatever the user enters will either get its input converted to a `string` (if it uses a compatible type, such as `integer`) or get an error that the type cannot be converted (such as `array`).

The greatest added value of getters and setters is appreciated in practical use.

The magic method `__toString()`
-----------------------------

Within a class, you can define a `__toString()` magic method that is automatically called when you try to write out an object as a string. The method must always return a string.

Example implementation:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function __toString(): string
    {
        return 'Hi, I'm ' . $this->name . ' ;)';
    }
}
```

The usage is then as follows:

```php
$cat = new Cat('Minda');

echo $cat; // Print "Hi, I'm Minda ;)"
```

Summary
-------

We have shown how to define methods and then call them within an object instance.

Next time, we'll look at the <a href="/encapsulation">encapsulation principle</a>, which takes full advantage of the properties of methods.
