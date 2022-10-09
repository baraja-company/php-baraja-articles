PHP nøgleord
============

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	da: php-nogleord
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Næsten alle programmeringssprog består af nøgleord, som er særlige udtryk i sproget, der har en særlig betydning.

Et eksempel på et nøgleord er ordet `string`, `if` eller `echo`.

Det er vigtigt at bemærke, at nøgleord (undertiden også `kommandoer`) <a href="/kommandoer-og-funktioner">ikke er funktioner</a>, så `echo`, for eksempel, er heller ikke en funktion.

Listen over nøgleord er god at kende, fordi de har særlige betydninger i PHP og ikke altid kan bruges til alt. F.eks. må navnet på en klasse ikke være det samme som et af de eksisterende nøgleord.

Eksempel på en parsefejl
-------------------

Når man forsøger at behandle en klasse med navnet `String`, opstår der en `Parse error`, fordi PHP ikke ved, om det er et klassens navn eller en datatype:

Det er forkert:

```php
class String
{
   // ...
}
```

Liste over nøgleord
-------------------

Opdateret liste over nøgleord til PHP 7.1:

`og`, `eller`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `extends`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`
