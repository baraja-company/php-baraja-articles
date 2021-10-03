PHP funkce var_export()
=======================

> id: "162ded4a-1246-4bd0-b710-2ff64bfb16e1"
> slug:
> 	cs: funkce-var-export
>
> publicationDate: "2019-10-07 10:28:27"
> mainCategoryId: "0eeab3a7-a54b-46db-a253-ca6100145648"

Dostupné od verze: `PHP 4.2.0`

Funkce vrátí string, který reprezentuje vloženou proměnnou jako kdyby byla napsána v PHP scriptu. Výstup lze rovnou použít pro PHP parser.

Parametry
---------

| Parametr | Datový typ | Výchozí hodnota | Poznámka |
|----------|------------|-----------------|----------|
| `$expression` | `mixed` |  | Proměnná s daty, které chceme exportovat. |
| `$return` | `bool` | null | Pokud je `true`, funkce hodnotu parametru vrátí, místo toho, aby se rovnou použila. |

Návratové hodnoty
-----------------

`mixed`

Funkce vrátí reprezentaci vložené proměnné, pokud byl druhý parametr `true`.

Pokud byl druhý parametr `false`, funkce vrátí vždy hodnotu `null`.

Příklad dumpování pole
----------------------

Mějme vstup:

```php
$a = [1, 2, ['a', 'b', 'c']];
var_export($a);
```

Vrátí:

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

Dumpování složitějších objektů a formátování
--------------------------------------------

Často je potřeba dumpovat (exportovat) objekty s velmi sloužitou strukturou, nebo dokonce vygenerovat celou PHP třídu podle zadání.

Dobře k tomu slouží balík [PhpGenerator](https://github.com/nette/php-generator), který je přímo součástí Nette framework.

Použití je jednoduché:

```php
$class = new Nette\PhpGenerator\ClassType('Demo');

$class
    ->setFinal()
    ->setExtends('ParentClass')
    ->addImplement('Countable')
    ->addTrait('Nette\SmartObject')
    ->addComment("Popis třídy.\nDruhý řádek\n")
    ->addComment('@property-read Nette\Forms\Form $form');

// kód jednoduše vygenerujete přetypováním na řetězec nebo použitím echo:
echo $class;
```

Vygeneruje:

```php
/**
 * Popis třídy
 * Druhý řádek
 *
 * @property-read Nette\Forms\Form $form
 */
final class Demo extends ParentClass implements Countable
{
    use Nette\SmartObject;
}
```

Dumpování konstant a polí v proměnných:

```php
$class->addConstant('ID', 123);

$class->addProperty('items', [1, 2, 3])
    ->setVisibility('private')
    ->setStatic()
    ->addComment('@var int[]');
```

Vygeneruje:

```
const ID = 123;

/** @var int[] */
private static $items = [1, 2, 3];
```

Další příklady jsou v [oficiální dokumentaci](https://doc.nette.org/cs/3.0/php-generator).

Další zdroje
------------

- [Oficiální manuál pro var_export](https://www.php.net/manual/en/function.var-export.php)
- [Česká dokumentace pro Php Generator](https://doc.nette.org/cs/3.0/php-generator)
