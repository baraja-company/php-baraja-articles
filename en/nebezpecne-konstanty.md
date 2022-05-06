Unsafe use of constants in PHP
==============================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	en: unsafe-use-of-constants-in-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

There are two tricky things to keep in mind when using constants in PHP.

Dynamic and static constants
------------------------------

A constant can be defined in PHP either statically directly in the class (the best solution), for example as follows:

```php
class Region
{
	public const PREFIX = 420;
}
```

And the usage is quite clear. At class compile time, the value of the constant is decided and we can access it by calling the class name and the constant itself. Most often by writing `Region::PREFIX`.

The other (much worse way) is to define the constant dynamically at runtime (most often somewhere in a configuration script), where it is then something like:


```php
define('BASE_DIR', __DIR__ . '/../');
```

The major disadvantage of defining a constant via the `define` function is that the script that defines the constant may not have been called, so the constant will not exist when you try to read it.

When combined with the use of a dynamic constant within the definition of a static one in a class, this can even result in a fatal reflection error:

```php
class InvoiceGenerator
{
	// This is completely wrong!
	public const DATA_DIR = BASE_DIR . '/data/invoice';
}
```

> **Explanation:**
>
> Using a dynamic constant within a static constant has the major disadvantage that the value of the dynamic constant cannot be read at compiletime. This script must therefore be processed again in each request (i.e. it cannot be stored in the OPCache for speed optimization), and if the constant did not exist at all, a fatal compile-time error is thrown and the application cannot run at all.

If you use PhpStan, it can automatically warn you about this problem:

```
Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Lessons learned:**
>
> The value of all constants should always be constant.


Inheritance of constants when using static
-------------------------------------

In some cases it makes sense to use inheritance to override the value of a constant. But in that case, the ancestor cannot read the value from the descendant (or shouldn't).

An example is when defining countries and regions:

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// Fatal error!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Paradoxically, the above code does not necessarily throw an error, but it can be thrown by inappropriate use of inheritance.

If we call the `getPrefix()` method on a descendant of `CzechRepublic`, everything will be correct because the value of the constant will be read correctly. However, if the descendant did not set the constant value, a fatal non-existent constant error would be thrown. The worst part of the whole thing is that it is a **hidden dependency** that is created in the implementation of the method, and the developer who inherits the class may not even know about this dependency.

The best solution in this case is to either define a constant directly in the ancestor with a default value (so that the logic always passes), or at least throw an exception in the getter.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('Region has not been defined.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan responds to this error as follows:

```
Access to undefined constant static(Region):REGION.
```
