PHP Słowa kluczowe
==================

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	pl: php-slowa-kluczowe
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Prawie wszystkie języki programowania zawierają `słowa kluczowe`, które są specjalnymi wyrażeniami języka o specjalnym znaczeniu.

Przykładem słowa kluczowego jest słowo `string`, `if` lub `echo`.

Należy pamiętać, że słowa kluczowe (czasem także `polecenia`) <a href="/polecenia-i-funkcje">nie są funkcjami</a>, więc na przykład `echo` też nie jest funkcją.

Dobrze jest znać listę słów kluczowych, ponieważ mają one specjalne znaczenie w PHP i nie zawsze mogą być używane do wszystkiego. Na przykład nazwa klasy nie może być taka sama jak jedno z istniejących słów kluczowych.

Przykładowy błąd parsowania
-------------------

Podczas próby przetworzenia klasy o nazwie `String` pojawia się błąd `Parse error`, ponieważ PHP nie wie, czy jest to nazwa klasy, czy typ danych:

To błąd:

```php
class String
{
   // ...
}
```

Lista słów kluczowych
-------------------

Zaktualizowana lista słów kluczowych dla PHP 7.1:

`and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `extends`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`.
