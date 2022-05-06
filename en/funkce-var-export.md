PHP function var_export()
=========================

> id: '162ded4a-1246-4bd0-b710-2ff64bfb16e1'
> slug:
> 	cs: funkce-var-export
> 	en: php-function-var-export
> 
> publicationDate: '2019-10-07 10:28:27'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '0b248c5e87d6faeb11561d7a38935133'

Available from: `PHP 4.2.0`

This function returns a string that represents the inserted variable as if it were written in a PHP script. The output can be used directly for a PHP parser.

Parameters
---------

| Parameter | Data type | Default value | Note |
|----------|------------|-----------------|----------|
| `$expression` | `mixed` | *not* | Variable with the data we want to export. |
| `$return` | `bool` | null | If `true`, the function returns the value of the parameter instead of using it directly. |

Return values
-----------------

`mixed`

This function returns the representation of the inserted variable if the second parameter was `true`.

If the second parameter was `false`, the function always returns `null`.

Example of dumping an array
----------------------

Let's have an input:

```php
$a = [1, 2, ['a', 'b', 'c']];
var_export($a);
```

Returns:

```
array (
  0 => 1,
  1 => 2,
  2 =>
  array (
    0 => 'a',
    1 => 'b',
    2 => 'c',
  ),
)
```

Dumping more complex objects and formatting
--------------------------------------------

Often you need to dump (export) objects with a very useful structure, or even generate an entire PHP class according to a specification.

The [PhpGenerator](https://github.com/nette/php-generator) package, which is directly part of the Nette framework, is a good way to do this.

It is easy to use:

```php
$class = new Nette\PhpGenerator\ClassType('Demo');

$class
    ->setFinal()
    ->setExtends('ParentClass')
    ->addImplement('Countable')
    ->addTrait('Nette\SmartObject')
    ->addComment("Class description.\nSecond row\n")
    ->addComment('@property-read Nette\Forms\Form $form');

// you can generate the code simply by rewriting it to a string or using echo:
echo $class;
```

Generates:

```php
/**
 * Class description
 * Second line
 *
 * @property-read Nette\Forms\Form $form
 */
final class Demo extends ParentClass implements Countable
{
    use Nette\SmartObject;
}
```

Dumping constants and arrays in variables:

```php
$class->addConstant('ID', 123);

$class->addProperty('items', [1, 2, 3])
    ->setVisibility('private')
    ->setStatic()
    ->addComment('@var int[]');
```

Generates:

```
const ID = 123;

/** @var int[] */
private static $items = [1, 2, 3];
```

See [official documentation](https://doc.nette.org/cs/3.0/php-generator) for more examples.

Other resources
------------

[Official var-export documentation](- [Official var_export manual](https://www.php.net/manual/en/function.var-export.php))
- [Czech Documentation for Php Generator](https://doc.nette.org/cs/3.0/php-generator)
