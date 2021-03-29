Ternární operátory v PHP (?:) - podmínka v jednom řádku
=======================================================

> id: "1191be9b-ff9d-4725-b0b4-17b60de2b935"
> slugCS: ternarni-operator
> perex: Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> publicationDate: "2019-11-26 11:59:18"
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc

Ternární operátor umožňuje zkrátit jednoduchou podmínku do jednoho řádku na místě, kde je rozepisování zbytečné, složité, nebo přímo nevhodné.

TL;DR
------

Ternární operátory jsou zkratkou pro `if` a `else` podmínky, proto je nemusíte používat. Hodí se pro stále se opakující kusy ověřovací logiky. Používejte formát `(podmínka ? pozitivní logika : negativní logika)` vždy včetně závorek. Používejte pro krátké ověření pro zpřehlednění a zkrácení kódu.

Základní definice
------------------

Často máme podmínku třeba ve tvaru:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'Je větší';
} else {
     echo 'Je menší';
}
```

Pokud tedy chceme vypsat jen jednu jednoduchou větu, musíme použít zbytečně 4 řádky kódu, což by šlo zkrátit na jeden.

```php
$a = 5;
$b = 3;

echo 'Je ' . ($a > $b ? 'větší' : 'menší');
```

Obecně se ternární operátor zapisuje za pomoci 3 částí (proto se jmenuje "ternární"):

```php
(podmínka ? pokud je pravda : pokud není pravda)
```

Ternární operátory se v praxi používají opravdu často, třeba k označení sudých řádků v tabulce:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class="' . ($i % 2 ? 'odd' : 'even') . '">';

          // Tady se nějak pracuje s tabulkou...
          // třeba: echo '<td>' . $pole[$i] . '</td>';

     echo '</tr>';
}
```

Příklad použití ternárního operátoru
------------------------------------

Docela často potřebujeme zkontrolovat existenci hodnoty nějaké proměnné a případně jí ihned použít. Pokud neexistuje, tak chceme vrátit defaultní hodnotu.

Klasickým přístupem se to řeší třeba takto:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Pokud existuje proměnná `$a`, použijte se jako hodnota `$a`, jinak `$b`.

Někdy však hodnotu získáváme z nějaké funkce:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ? funkce($a, $b) : $default;
```

Tento způsob volání je velice neefektivní vzhledem k systémovým prostředkům. Nejprve se totiž musí funkce zavolat, a pokud existuje, tak se volá znovu, pro získání hodnoty, která se uloží do proměnné `$c`.

To by šlo lépe vyřešit přes pomocnou proměnnou:

```php
$a = 5;
$b = 3;
$pomocnaPromenna = funkce($a, $b);
$default = 42;

$c = $pomocnaPromenna ? $pomocnaPromenna : $default;
```

Nevhodné použití
------------------

Ne vždy se ternární operátor vyplatí použít, protože při zápisu komplikovaných a vnořených podmínek způsobuje spíše zmatky.

Podívejte se sami na příklad:

```php
$valid = true;
$lang = 'french';

$x = $valid
     ? ($lang === 'french' ? 'oui' : 'yes')
     : ($lang === 'french' ? 'non' : 'no');
```

Poznali byste na první pohled, že proměnná `$x` bude obsahovat hodnotu `oui`?

Po trochu cviku možná ano, ale to není ta správná odpověď. :)

Ověření (ne)existence hodnoty a použití výchozí
--------------------

Největší sílu mají ternární operátory při rutinním ověřování (ne)existence hodnot a použití jiných výchozích.

Například chceme zjistit, jestli pro článek existuje jeho hlavní kategorie a pokud ne, tak vypsat náhradní hlášku:

```php
echo $mainCategory ?? 'Kategorie neexistuje';
```

Operátor `??` (dva otazníky) ověří, jestli proměnná `$mainCategory` existuje a není `null`. Funguje stejně, jako funkce `isset()`.

Ověření prázdnosti hodnoty
-----------------------------

Velmi často se hodí konstrukce pro ověření, že výstupní hodnota není prázdná (není `null`, `0`, `false` a podobně).

To se dá zapsat takto:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ?: $default;
```

Nejprve se zavolá `funkce($a, $b)`, poté se otestuje její hodnota a když není prázdná, tak se ihned předá do proměnné `$c`, jinak bude použita proměnná `$default`.

Operátor `?:` funguje jako operátor `!=` v podmínce (nepravda bez ohledu na datový typ) a zapamatovat se dá třeba tak, že vypadá jako smajlík s vlasy Elvise.

Podpora starších verzí PHP
----------------------------

Operátor `?:` funguje od PHP 7. Ve starších verzí si musíme vystačit s podmínkou `if (...)`, kterou lze docílit stejného chování. Nezapomínejte na to, že ternární operátory jsou ve skutečnosti jen zkratka k zápisu toho samého, co se řeší podmínkami.

Neexistence hodnoty lze ověřit funkcí `isset()`, která kontroluje, že proměnná existuje a není prázdná (`null`).

Místo kódu:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Potom zapíšeme starší alternativu:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Upozornění:** Na pořadí `isset()` a samotné proměnné záleží. Pokud bychom pořadí otočili a proměnná neexistovala, tak by se vyvolala chyba přístupu do neexistující proměnné.
