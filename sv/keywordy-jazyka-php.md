PHP-nyckelord
=============

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	sv: php-nyckelord
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Nästan alla programmeringsspråk består av nyckelord, som är särskilda uttryck i språket med en speciell betydelse.

Ett exempel på ett nyckelord är ordet `string`, `if` eller `echo`.

Det är viktigt att notera att nyckelord (ibland även kommandon) <a href="/commands-and-functions">inte är funktioner</a>, så till exempel `echo` är inte heller en funktion.

Listan över nyckelord är bra att känna till eftersom de har särskilda betydelser i PHP och inte alltid kan användas för allt. Namnet på en klass får till exempel inte vara detsamma som ett av de befintliga nyckelorden.

Exempel på fel vid analys
-------------------

När man försöker bearbeta en klass som heter `String` uppstår ett `Parse error` eftersom PHP inte vet om det är ett klassnamn eller en datatyp:

Detta är fel:

```php
class String
{
   // ...
}
```

Förteckning över nyckelord
-------------------

Uppdaterad nyckelordslista för PHP 7.1:

`and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `extends`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`
