Miestne premenné v PHP
======================

> id: '9c3fbf4e-8612-4599-8127-d66b9ec5e980'
> slug:
> 	cs: lokalni-promenna
> 	sk: miestne-premenne-v-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: cffee22c4dbcc2ec331b73235349c409

Lokálne premenné sú platné len v tele **<a href="/prikazy-a-funkcia">funkcie</a>** alebo **metódy** (v objektovo orientovanom programovaní).

Ak pracujeme v kontexte bežného skriptu, všetko sa deje podľa očakávania:

```php
$x = 5;

echo $x; // vypíše 5
```

Keď však definujeme vlastnú funkciu, správanie sa mierne zmení:

```php
$x = 5;

funkcia myFunction(): int
{
    $x = 3;

    echo $x; // vypíše 3
}

echo $x; // vypíše 5
```


Dôvodom je, že premenná $x existuje len v kontexte aktuálnej funkcie alebo metódy. Toto správanie je zámerné.

Ak chceme funkcii odovzdať hodnotu z okolitého kódu, musíme ju zavolať s potrebnými parametrami:

```php
echo myFunction(5); // vypíše 6

funkcia mojeFunkcia(int $x): int
{
    vrátiť $x + 1;
}
```


Hodnoty sa do funkcií odovzdávajú pomocou parametrov. Ak chcete do funkcie odovzdať ďalšie premenné nad rámec parametrov, môžete použiť <a href="/global-variable">globálne premenné</a>, ale nie je to dobrý nápad.

> Používanie lokálnych premenných je pri programovaní väčších aplikácií veľmi dôležité. Ak by sme nerozlišovali platnosť premenných v rôznych kontextoch, nebolo by možné zaručiť, že premenná nebude prepísaná na mieste, kde s ňou nepočítame (preto sú globálne premenné zlé).

```php
$x = 5;
$y = 3;

funkcia sum(int $x, int $y): int
{
    vrátiť $x + $y;
}

echo $x; // vypíše 5
echo sum($x, $y); // vypíše 8
```
