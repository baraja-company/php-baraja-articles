Keywordy jazyka PHP
================================

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slugCS: keywordy-jazyka-php
> publicationDate: 2021-03-27 22:01:32
> mainCategoryId: 6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070

Skoro všechny programovací jazyky se skládají z tzv. `klíčových slov` (anglicky `keywords`), což jsou speciální výrazy jazyka, které mají speciální význam.

Příkladem keywordu je například slovo `string`, `if` nebo `echo`.

Důležité je si uvědomit, že klíčová slova (někdy také `příkazy`) <a href="/prikazy-a-funkce">nejsou funkce</a>, proto ani například `echo` není funkce.

Seznam keywordů je dobré znát, protože mají v jazyce PHP speciální význam a nelze je použít vždy na všechno. Například název třídy nesmí být stejný, jako některý z existujících keywordů.

Příklad parse error
-------------------

Při pokusu o zpracování třídy s názvem `String` dojde k `Parse error`, protože PHP neví, jestli jde o název třídy, nebo o datový typ:

Toto je špatně:

```php
class String
{
   // ...
}
```

Seznam keywordů
-------------------

Aktualizovaný seznam keywordů pro PHP 7.1:

`and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `extends`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`
