Data type Enum object in PHP
============================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	de: type-php
>
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

As of PHP 8.1, the Enum data type can be used to define exact enumeration values for a list. This is useful for cases where we know that the value of a variable can only take on a specific few values.

For example, this is how I store notification types:

```php
enum OrderNotificationType: string
{
    case Email = 'email';
    case Sms = 'text';
}
```

In PHP, the Enum data type is a classic object that behaves like a special type of constant, but also has an instance that can be passed on. However, unlike a regular object, it is subject to a number of restrictions.

Differences between Enum and objects
-----------------------

Although enums are built on top of classes and objects, they do not support all the functionality associated with objects. In particular, enum objects are prohibited from having internal state (they must always be a static class).

A specific list of differences:

- Constructors and destructors are forbidden.
- Inheritance is not supported. Enums cannot be extended or inherited by another class.
- Static or object properties are not allowed.
- Cloning of specific Enum values (instances) is not supported, each individual instance must be a singleton instance.
- Magic methods, except as noted below, are prohibited.

The following object features are available and behave just like any other object:

- Public, Private, and Protected methods.
- Public, private, and protected static methods.
- Public, private, and protected constants.
- Enums can implement any number of interfaces.
- Attributes can be attached to enums and cases. The target filter `TARGET_CLASS` includes the Enums themselves. The target filter `TARGET_CLASS_CONST` includes Enum cases.
- Magic methods `__call`, `__callStatic` and `__invoke`.
- The constants `__CLASS__` and `__FUNCTION__` behave like normal constants
- The magic constant `::class` on the Enum type is evaluated as the full name of the data type, including any namespace, exactly as for an object. The magic constant `::class` on an instance of type `Case` is also evaluated as type Enum, since it is an instance of a different type.

Using Enum as a data type
-----------------------------

Imagine we have an enum representing the types of suits. In this case, we just need to define the `Suit` type and store the individual valid values.

We then get an instance of the particular option classically via a quadratic, as when working with a static constant.

Example of defining an Enum, invoking it by a specific type and passing it to a function:

```php
enum Suit
{
	case Hearts;
	case Diamonds;
	case Clubs;
	case Spades;
}

function doStuff(Suit $s)
{
	// ...
}

doStuff(Suit::Spades);
```

Comparison of two values
---------------------

The fundamental advantage of enums over objects and constants is that it is easy to compare their values.

A basic comparison that we work with a specific value can be done as follows:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // true
```

Very often we also need to decide that a particular value belongs in a valid Enum value enumeration. This can be easily verified as follows:

```php
$a = Suit::Spades;

$a instanceof Suit;  // true
```

Reading the value of the type
---------------------

We can read a specific type value either as a name of a calling constant or directly as a real defined value (if it exists):

```php
enum Colors
{
	case Red;
	case Blue;
	case Green;

	public function getColor(): string
	{
	    return $this->name;
	}
}

function paintColor(Colors $colors): void
{
	echo "Paint:" . $colors->getColor();
}
```

The value of the calling constant is read via the `name` property. It is also important that a custom function can be implemented directly in the Enum data type, which can be called over each Enum.

If a particular Enum also implements real values (which are hidden under each constant), their value can be read as well:

```php
enum OrderNotificationType: string
{
    case Email = 'email';
    case Sms = 'text';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

All valid Enum values
-----------------------------

Often we need to list (for example, to the user in an error message) all possible values that Enum can take. When using constants this was not possible, Enum allows this easily:

```php
Suit::cases();
```

Returns `[Suit::Hearts, Suit::Diamonds, Suit::Clubs, Suit::Spades]`.

Verify that the variable is of type Enum
---------------------------------

We can easily verify that a particular unknown variable contains Enum by a condition:

```php
if ($haystack instanceof \BackedEnum) {
```

Each Enum object is automatically a child of the generic `\BackedEnum` interface.

For more information, see the discussion on the PhpStan GitHub: https://github.com/phpstan/phpstan/issues/7304
