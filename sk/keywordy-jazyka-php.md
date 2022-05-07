Kľúčové slová PHP
=================

> id: d58620d7-57a0-410a-ba73-31a1f9d984fb
> slug:
> 	cs: keywordy-jazyka-php
> 	sk: klucove-slova-php
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '38fbdb53b4455903f3300c3c8b4a8ad2'

Takmer všetky programovacie jazyky sa skladajú z "kľúčových slov", čo sú špeciálne výrazy jazyka, ktoré majú špeciálny význam.

Príkladom kľúčového slova je slovo `string`, `if` alebo `echo`.

Je dôležité poznamenať, že kľúčové slová (niekedy tiež `príkazy`) <a href="/príkazy-a-funkcie">nie sú funkcie</a>, takže napríklad `echo` tiež nie je funkcia.

Zoznam kľúčových slov je dobré poznať, pretože v PHP majú špeciálny význam a nedajú sa vždy použiť na všetko. Napríklad názov triedy nesmie byť rovnaký ako jedno z existujúcich kľúčových slov.

Príklad chyby pri analyzovaní
-------------------

Pri pokuse o spracovanie triedy s názvom `String` sa vyskytne chyba `Parse error`, pretože PHP nevie, či ide o názov triedy alebo dátový typ:

To je nesprávne:

```php
class String
{
   // ...
}
```

Zoznam kľúčových slov
-------------------

Aktualizovaný zoznam kľúčových slov pre PHP 7.1:

`and`, `or`, `xor`, `for`, `do`, `while`, `foreach`, `as`, `return`, `die`, `exit`, `if`, `then`, `else`, `elseif`, `new`, `delete`, `try`, `throw`, `catch`, `finally`, `class`, `function`, `string`, `array`, `object`, `resource`, `var`, `bool`, `boolean`, `int`, `integer`, `float`, `double`, `real`, `string`, `array`, `global`, `const`, `static`, `public`, `private`, `protected`, `published`, `extends`, `switch`, `true`, `false`, `null`, `void`, `this`, `self`, `struct`, `char`, `signed`, `unsigned`, `short`, `long`
