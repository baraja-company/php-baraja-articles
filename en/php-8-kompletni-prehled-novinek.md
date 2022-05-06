PHP 8 is out - complete overview
================================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	en: php-8-is-out---complete-overview
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

Today, November 26, 2020, the new major version of PHP 8 was released after several years, and it includes a bold set of new features. This is one of the biggest updates in a long time and deserves a special article.

In this article, we'll summarize all the major new features and the differences in syntax and options compared to the older version. Most of the new features are backwards compatible and bring behavioral improvements that you will enjoy.

> **Important information:** PHP 8 is now in a `feature freeze` phase, which means that new behaviors can no longer be added and only bugs are fixed. So you can count on compatibility and fully debug your applications.

Union types
----------

PHP in general has been shifting in recent years from a purely dynamic language where any variable could contain anything to a strict form where we know in advance what data type will be in what variable, parameter, argument or property. The use of [data-types](/datove-types) is still optional, but I recommend the use of strong typing and use it myself on all projects.


Union types express a collection of multiple types, accepting any argument or property in them.

For example:

```php
function validatePsc(string|int $psc): bool
{
	// implementation
}
```

The `validatePsc()` function in the `$psc` variable accepts the `string` (string) and `int` (integer) data types.

In the previous version of PHP 7.4, this notation was not possible and was typically bypassed by [comment](/document-comment):

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// implementation
}
```

However, this annotation comment is ignored by PHP (it is a comment, after all) and we had to perform an additional check with an external tool such as PhpStan, which many developers ignored. Now the check is done directly at runtime (when the application is running) and cannot be bypassed.

However, PHP has known a certain type of union type since version 7, when it was possible to say that the main type could also be `nullable`, i.e. it accepts the main data type plus the value `null`.

This was written in two ways, each with a different meaning:

```php
function setPhone(?string $phone): void
{
	// implementation
}

// or

function setPhone(string $phone = null): void
{
	// implementation
}

// or combination

function setPhone(?string $phone = null): void
{
	// implementation
}
```

All the notations say that the phone `int` (integer) is either a `string` or `null`.

- The first notation always requires passing a value
- The second notation does not require any value to be passed; if nothing is passed, `null` is used as the default value (it is an optional argument)
- The third entry is a combination of the options and behaves like the second entry

When using union types, we will no longer be able to use a notation with a question mark and must strictly define the `null` data type, for example:


```php
function setPhone(string|int|null $phone = null): void
{
	// implementation
}
```

The phone number must now be `string`, `int` or `null`.

Union types still have a number of uses, which advanced developers will read about in the documentation or implementation of specific libraries.


JIT - faster script processing
----------------------------------

The JIT (just in time) compiler brings a significant improvement in script complication (parsing and understanding) performance. However, this behavior may vary in the context of web requests.

You can now see if you have JIT enabled in the Tracy bar within the Nette framework, and see [separate article](https://stitcher.io/blog/php-jit) for more details.

The general thing to say about compilation is that PHP tries to process code up front so that when it processes a particular request, it doesn't have to go through a physical script file, parse it, and interpret it. In the past, this was handled via the OPCache extension (which servers and hosts have available by default) and it improved processing speed by about half.

As a general rule of thumb, if you have a slow application, it's always better to choose a suitable algorithm to handle a particular task than to make micro-optimizations in the code. Usually the big delays are caused by waiting for the database and its slow queries, saving sessions, waiting for hard disk space to free up, and other hardware operations.

Nullsafe operator (optional chaining)
-------------------------------------

Very often in a real application, it is necessary to verify the existence of a return value (that it is not `null`) from one method and then conditionally call another. The [ternary operators](/ternary-operator) are great for this, but they only work with one condition and cannot be nested. The nullsafe operator allows for nesting natively.

> **TIP:** Virtually the same behavior is already supported by the Latte templating system, but it overrides this type of syntax in native PHP code, so you can use the nullsafe operator on older versions of PHP (from PHP 7 onwards). Kudos to David for this modification!

This makes it easy to use:

```php
$orderId = $order?->getId();
```

The `$orderId` variable contains either the value returned by the `getId()` method, or `null` if the `$order` variable contains the value `null` and the `getId()` method could not be called.

This type of problem was circumvented in PHP 7 by the following syntax via the ternary operator:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Alternatively, a condition:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

The entry can be written further into the call. I took the example from [Latte documentation](https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), which describes it perfectly:

```php
$orderName = $order->item?->name;

// same as:

$orderName = isset($order->item) ? $order->item->name : null;
```

Typical use is when listing more complex structures in a template, for example in Latte it looks like this (sample taken from documentation):

```php
{$user?->address?->street}
// means approx ($user !== null) && ($user->address !== null) ? $user->address->street : null

{$items[2]?->count}
// replace approx ($items[2] !== null) ? $items[2]->count : null

{$user->getIdentity()?->name}
// replace approx $user->getIdentity() !== null ? $user->getIdentity()->name : null
```

In real code it may look like this, for example, that we want to find out the country of a customer by reading his profile (and you have the data stored in the database nicely via sessions, as it is supposed to be done), then in old PHP it looked like this:

```php
$country = null;
if ($session !== null) {
    $user = $session->user;
    if ($user !== null) {
        $address = $user->getAddress();
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

Now it can be shortened to a single line:

```php
$country = $session?->user?->getAddress()?->country;
```

The use of the nullsafe operator also prevents various errors that could not be easily detected by an inexperienced developer in PHP 7.

For example, this entry will generate a fatal error:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// returns: fatal error: uncaught Error: call to a member function format() on null
```

The correct syntax should look like this:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// return: null
```

Named arguments
---------------------

In good old PHP, function calls with arguments had to be written by passing the arguments in the exact order defined by the target function. There is nothing wrong with this, however, when using a number of parameters with similar values, it could cause poorer readability. Or if we wanted to pass up to the nth parameter in the order, all optional parameters had to be passed before, which could have a negative effect on readability and forward compatibility.

Imagine, for example, the `setCookie()` function in Nette, which has a lot of arguments:

```php
public function setCookie(
	string $name,
	string $value,
	$time,
	string $path = null,
	string $domain = null,
	bool $secure = null,
	bool $httpOnly = null,
	string $sameSite = null
)
```

The first three arguments (`$name`, `$value` and `$time`) are mandatory, but if we wanted to pass in the `$httpOnly` argument, we had to pass in all the previous ones and calculate the order correctly:


``php
$http->setCookie(
	'myCookie',
	'David likes horses',
	'now',
	null, // path
	null, // domain
	null, // secure
	true
);
```

Which you simply don't want to do if you don't have to.

Elegant writing then looks like:

```php
$http->setCookie(
    name: 'myCookie',
    value: 'David likes horses',
    time: 'now',
    httpOnly: true
);
```

This type of syntax requires that the names of the arguments in the target function never change, because they will still be typed when called. At least the developers will be able to name them better.

If we want to use only one of the arguments, the syntax can be combined and condensed to just one line:

```php
$http->setCookie('myCookie', 'David likes horses', 'now', httpOnly: true);
```

The first 3 arguments are passed in the original way, then the optional `httpOnly` argument is passed (because it is named).

Attributes
---------

Most major languages such as Java or C# already natively include so-called annotations, which is a native language syntax that allows you to add meta information to other language constructs.

In PHP, this type of syntax has long been missing, and has been circumvented by using DOC comments, which is a classic comment over a method, except it has two `/**` asterisks.

These comments are ignored during script processing and special user logic must be added to parse and interpret them at runtime via reflection. You can probably understand the performance impact this can have, plus the syntax of the comments cannot be required and is very hard to check at compiletime (when the script is processed before it is run), and again you have to use additional tools outside of the normal PHP toolkit to do this.

To preserve backward compatibility, PHP provides attributes with syntax similar to the alternate comment notation, which does not break running the script on legacy PHP.

The original notation (used, for example, for Inject dependencies in Nette Presenter):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

You can now remove the comment and use the native attribute:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

What is very important is that attribute is no longer just some piece of string in a comment, but a physical class that is valid PHP code.

This is great because you can now safely validate the inputs to an attribute, and using an attribute actually becomes a call to its constructor where other logic can be used. I'm looking forward to seeing this natively supported by Doctrine, which uses annotations for everything.

The implementation of the attribute itself might then look something like this:

```php
#[Attribute]
class Inject
{
    public string $value;

    public function __construct(string $value)
    {
        $this->value = $value;
    }
}
```

Again, strict logic can be used within attribute, such as checking argument data types, union types, and other language conveniences.

Match expression
----------------

The new language construct `match()` is a modernized improvement on the good old `switch()` (which I try not to use), and brings a number of cool features (which will make me start using it again).

For example, we want to modify the value of a variable based on the input:

```php
$health = match(bool $formal) {
    true => 'Hello',
    false => 'Hello',
};
```

One important new syntax feature is that we don't have to use `break` (like the old `switch`) and the syntax is generally much more economical.

At the same time, multiple inputs can be verified at once within a condition (separated by a comma) and possibly return a default value (when none satisfies).

This comes in handy when rewriting HTTP condition code to an error message, for example (you'll definitely appreciate it when handling exception codes):

```php
$message = match($statusCode) {
    200, 300 => null,
    400 => 'not found',
    500 => 'server error',
    default => 'unknown status code',
};
```

The comparison of values is done strictly by the `===` operator (the switch only uses `==`), which again shows that PHP follows the strict design path. Therefore, the input ``200`` (a string containing a number) will not be accepted in the previous case.

If you do not specify a value for `default` and there is no match, an `UnhandledMatchError` is thrown.

The new syntax also allows an expression or function call to be used for matching (it behaves like a condition). In case of an error, we can then throw an exception (since the `throw` token is now an expression and can be used this way):

```php
$message = match($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'unknown status code',
};
```

Propagation of properties into the constructor
-------------------------------------------------

This is just syntactic sugar, which will be useful for quick and easy definition of an entity and its properties directly in the constructor.


For example, the original entity:

```php
final class User
{
    public string $name;

    public function __construct(
        string $name,
    ) {
        $this->name = $name;
    }
}
```

Can only be shortened to:

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

The `$name` property is validated against the `string` data type, and its value can be read directly from the instance because it is a public property. Additionally, if you use SmartObject in Nette (which I rather don't recommend for PHP 8), you can access private properties by calling their getter method first, and this syntax will again simplify things.

Return type static
--------

In the past, we could use the `self` data type as the return value of a method, but it returns an instance of the very class where it is defined. The `static` datatype can generally do this even in the case of inheritance, and will return the datatype of the class from which the instance was executed, not its ancestor.

For example:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Data type mixed
------------------

The `mixed` type can now be used as a function or method argument. This means that the method must always accept some input (and is therefore a mandatory argument).

If you can at least a little bit, always use a direct data type, or at least union. Mixed is only useful if the function accepts really anything. In practice, the usage is useful, for example, for various dump functions that accept arbitrary input and must be able to display it.

The `mixed` type accepts the following types: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David will then use the mixed type for his function:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Token throw as an expression
------------------------

The `throw` token has now become an expression, in practice this means that an exception can be thrown when the `fn()` lambda function is truncated, or when a ternary operator is checked:


```php
$error = fn() => throw new \InvalidArgumentException('This always throws an error.');

$userName = $user['name'] ?? throw new \LogicException('The user must have a name.');
```

The str_contains() function
-----------------------

Finally, PHP includes a native function to verify that the default string contains a substring.

For example:

```php
if (str_contains('Honzik likes cats.', 'cats') {
	echo 'The function handles cats.';
}
```

In the past, the occurrence of a substring was checked by the [strpos](/function-strpos) function:

```php
if (strpos('Honzik likes cats.', 'cats') !== false) {
	echo 'The function handles cats.';
}
```

Functions str_starts_with() and str_ends_with()
----------------------------------------------

A pair of new functions to check if a string starts or ends with a substring:

```php
str_starts_with('Honzik likes cats.', 'Honzik'); // true

str_ends_with('Honzik likes cats.', 'cats.'); // true
```

Function get_debug_type()
-------------------------

Enhance the output of the existing [gettype](/function-gettype) function, which only returned the generic type of the passed variable. The function is used, for example, when throwing an exception, when we get non-valid input and want to tell the user what they actually passed.

When we call the `gettype()` function with a variable containing an instance of class `\App\User`, the function returns `object`, so we don't know what class it is. The new function `get_debug_type()` returns the name of the class.

The function get_resource_id()
--------------------------

This function returns the identifier of an external resource from a variable.

For example, connecting to a MySql database is handled by PHP by using a special `resource` data type, now it is possible to find out what ID has been assigned to it.

> **Historical note:**
>
> The `resource` type in PHP was created at a time when it didn't yet know how to use objects, and had to somehow figure out how to pass references to something like a `data type`. In the future, you can expect `resource` to be removed from the language altogether, so it's best not to use this feature.

The ext-json extension is always available
-------------------------------------

In the past, PHP could be compiled without support for json. Now, json will always be available, so you can remove the `ext-json` dependency from your `composer.json` files and always know that json can be used.

Concatenation precedence
-------------------------------------------------------

Imagine something like:

```php
echo 'Sum: ' . $a + $b;
```

Is the addition of the numbers done first, or is the variable `$a` appended to the string first and then the whole new string is added to `$b`?

One would expect addition to be done first, but that is a nice assumption. PHP actually does something like this:

```php
echo ('Sum: ' . $a) + $b;
```

PHP 8 now behaves predictably:

```php
echo 'Sum: ' . ($a + $b);
```

In general, however, it is always better to use parentheses to enclose an expression.

Stable ordering
---------------

Prior to PHP 8, string sorting was done using the so-called unstable algorithm, which means that PHP did not guarantee that elements with the same (or otherwise equivalent) value would not be swapped. The new version changes the behavior of all sorting functions to stable, so the sorting is always done deterministically and you always get the same output.

This solves, for example, cases where we were ranking user ratings by relevance, but some ratings had the same score. Now they will appear in the same order each time you sort and will not continuously skip.

Other new features
---------------

PHP has many other minor new features and improvements. For example, errors will be thrown differently (but that doesn't bother us who write error-free code, right?).

You can always see the full list of changes in the official documentation and RFC post.

What I miss in the new PHP
-----------------------

I'd like it if PHP finally supported composite array types, for example when a method returns an array of identifiers we still have to specify just `getIds(): array` and something like `getIds(): int[]` would be much better. Maybe we'll see this soon and it will make strong type checking complete.

More resources
------------

David Grudl gave a nice talk about the new stuff at Posobot. I recommend to watch the recording:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

This is to thank David for his lecture, as I have drawn some information from it for this article. In particular, stuff about Nette's move towards PHP 8 and other behind-the-scenes tips about PHP.
