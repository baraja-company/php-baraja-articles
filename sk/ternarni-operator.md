Ternárne operátory v PHP (?:) - podmienka v jednom riadku
=========================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	sk: ternarne-operatory-v-php-podmienka-v-jednom-riadku
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- Operátor ternárnych operátorov umožňuje zapísať jednoduchú podmienku do jedného riadku ako výraz.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '20ff690401289869eae2a2584fae7568'

Operátor ternárneho typu umožňuje skrátiť jednoduchú podmienku do jedného riadku na mieste, kde je rozbor zbytočný, zložitý alebo úplne nevhodný.

TL;DR
------

Ternárne operátory sú skratkou pre podmienky `if` a `else`, takže ich nemusíte používať. Sú užitočné pre stále sa opakujúce časti overovacej logiky. Vždy používajte formát `(podmienka ? kladná logika : záporná logika)` vrátane zátvoriek. Použite na krátke overenie, aby bol kód jasnejší a kratší.

Základné definície
------------------

Často máme podmienku napríklad v tvare:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'Je väčší';
} else {
     echo 'Je menší';
}
```

Ak teda chceme napísať len jednu jednoduchú vetu, musíme použiť 4 riadky kódu, ktoré by sa dali zredukovať na jeden.

```php
$a = 5;
$b = 3;

echo 'Je to' . ($a > $b ? 'väčšie' : 'menšie');
```

Všeobecne sa ternárny operátor zapisuje pomocou 3 častí (preto sa nazýva "ternárny"):

```php
(condition ? 'áno' : 'z adresy')
```

Ternárne operátory sa v praxi používajú veľmi často, napríklad na označenie párnych riadkov v tabuľke:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'z adresy' : 'aj') . '">';

          // Toto je nejakým spôsobom práca s tabuľkou...
          // napríklad: echo '<td>' . $field[$i] . '</td>';

     echo '</tr>';
}
```

Príklad použitia ternárneho operátora
------------------------------------

Často potrebujeme overiť existenciu hodnoty premennej a v prípade potreby ju okamžite použiť. Ak neexistuje, chceme vrátiť predvolenú hodnotu.

Klasický prístup je takýto:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Ak existuje premenná `$a`, použije sa ako hodnota `$a`, inak `$b`.

Niekedy však získame hodnotu z funkcie:

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ? my_function($a, $b) : $default;
```

Tento spôsob volania je z hľadiska systémových zdrojov veľmi neefektívny. Najprv sa musí zavolať funkcia, a ak existuje, zavolá sa znova, aby sa získala hodnota, ktorá sa uloží do premennej `$c`.

To by sa dalo lepšie riešiť pomocou pomocnej premennej:

```php
$a = 5;
$b = 3;
$helper = my_function($a, $b);
$default = 42;

$c = $helper ? $helper : $default;
```

Nevhodné používanie
------------------

Trojčlenný operátor sa neoplatí vždy používať, pretože pri písaní zložitých a vnorených podmienok spôsobuje zmätok.

Pozrite si príklad:

```php
$valid = true;
$lang = 'Francúzsky';

$x = $valid
     ? ($lang === 'Francúzsky' ? 'oui' : 'áno')
     : ($lang === 'Francúzsky' ? 'nie' : 'z adresy');
```

Vedeli by ste na prvý pohľad, že premenná `$x` bude obsahovať hodnotu `oui`?

Po troche praxe by ste mohli, ale to nie je správna odpoveď. :)

Overenie (ne)existencie hodnoty a použitie predvolenej hodnoty
--------------------

Ternárne operátory sú najsilnejšie pri rutinnom overovaní (ne)existencie hodnôt a používaní iných predvolených hodnôt.

Napríklad chceme skontrolovať, či pre článok existuje hlavná kategória, a ak nie, vypísať náhradnú správu:

```php
echo $mainCategory ?? 'Kategória neexistuje';
```

Operátor `??` (dva otázniky) kontroluje, či premenná `$mainCategory` existuje a nie je `null`. Funguje rovnako ako funkcia `isset()`.

Overovanie prázdnoty hodnoty
-----------------------------

Veľmi často užitočná konštrukcia na overenie, či výstupná hodnota nie je prázdna (t. j. nie je `null`, `0`, `false` alebo `''*(empty string)*).

To sa dá zapísať takto:

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ?: $default;
```

Najprv sa zavolá `funkcia($a, $b)`, potom sa otestuje jej hodnota a ak nie je prázdna, okamžite sa odovzdá premennej `$c`, inak sa použije premenná `$default`.

Operátor `?:` funguje podobne ako operátor `!=` v podmienke (false bez ohľadu na typ údajov) a možno si ho zapamätať napríklad tak, že vyzerá ako smajlík s vlasmi Elvisa.

Podpora starších verzií PHP
----------------------------

Operátor `?:` funguje od PHP 7. V starších verziách sa musíme uspokojiť s podmienkou `if (...)`, ktorá môže dosiahnuť rovnaké správanie. Nezabudnite, že ternárne operátory sú v skutočnosti len skratkou na zápis toho istého, čo sa rieši pomocou podmienok.

Neexistenciu hodnoty možno skontrolovať pomocou funkcie `isset()`, ktorá overí, či premenná existuje a nie je prázdna (`null`).

Namiesto kódu:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Potom zapíšeme staršiu alternatívu:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Upozornenie:** Záleží na poradí `isset()` a samotnej premennej. Ak by sme zmenili poradie a premenná by neexistovala, vyvolalo by to chybu prístupu k neexistujúcej premennej.
