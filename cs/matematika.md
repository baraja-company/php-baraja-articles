Matematika v PHP
================

> id: "69a432ee-7635-46e2-bb21-8492cb62c4e6"
> slug:
> 	cs: matematika
> 
> perex: "Matematika a matematické funkce v PHP. Definice čísel, operátorů a vhodné typy pro výpočty a ukládání čísel."
> publicationDate: "2020-02-16 18:24:10"
> mainCategoryId: c2134b23-9b10-46b3-aa54-e3996707255e

Stejně jako v ostatních jazycích, tak i v PHP jsou čísla reprezentována ve dvojkové soustavě (soustava jedniček a nul), při některých matematických operacích proto může vycházet lehce odlišný výsledek, než bychom čekali. Tato stránka obsahuje přehled chování výpočtů v různých kontextech. Pro pochopení je potřeba aspoň základní znalost **číslicové techniky** a **číselných soustav**.

Reprezentace čísel v paměti
---------------------------

Způsob reprezentace čísel charakterizuje datový typ, například integer je převeden přímo ze zdrojového kódu z desítkové soustavy na dvojkovou (binární). O převod čísel se nemusíme vůbec starat, vše probíhá automaticky.

Důsledek tohoto převodu je, že maximální hodnota integeru (celé číslo) je určena podle počtu dostupných bitů v paměti. Na 32-bitovém PHP je rozsah od `-2,147,483,648` do `2,147,483,647` (~ ± 2 miliardy), 64-bitové PHP má rozsah od `-9,223,372,036,854,775,808` do `9,223,372,036,854,775,807` (~ ± 9 quintillionů). Maximální hodnotu můžeme vždy získat zavoláním konstanty `PHP_INT_MAX`.

Desetinná čísla se ukládají do datového typu **float**, který má omezenou přesnost, proto se interně ukládá jako dvojice čísel, které podléhají matematické operaci `mantisa * (2 ^ exponent)`. Praktický důsledek je ten, že jsme schopni na malé množství bitů uložit i relativně velké číslo (v řádu miliard), akorát ne příliš přesně.

Důsledek použití datového typu **float** je, že výsledkem výpočtu `1 - 0.9` není přesně `0.1`.

Příklad z <a href="https://diskuse.jakpsatweb.cz/?action=vthread&forum=9&topic=97912">webového fóra</a>:

```php
$x = 0.3;
$y = 0.4;
echo 0.7 - $x - $y; // vypíše -5.5511151231258E-17
```

Pokud ovšem čísla lehce upravíme:

```php
$x = 6.5;
$y = 7.5;
echo 14.0 - $x - $y; // vypíše 0
```

Obecně o této problematice pojednává <a href="http://vtm.e15.cz/proc-pocitacum-delaji-problemy-desetinna-cisla">samostatný článek na webu VTM.e15.cz: Proč počítačům dělají problémy desetinná čísla</a>, případně obecně o <a href="https://cs.wikipedia.org/wiki/Pohyblivá_řádová_čárka">plovoucí řádové čárce na Wikipedii</a>.

> Obecně je dobré po výpočtu výsledek zaokrouhlit, nebo se desetinným číslům úplně vyhnout. Například cenu nebudeme ukládat v korunách (jako 14.90), ale v haléřích (jako 1490) a poté s ní pracovat přesně. O zaokrouhlování pojednává další kapitola článku.

Matematické operace
-------------------

```php
$a = 5;
$b = 3;

echo $a + $b;
```

Pro operace lze použít běžné matematické zvyklosti, při kterých se respektuje přednost operátorů podle pravidel matematiky (násobení má přednost před sčítáním a podobně). Hodnoty lze zapsat přímo, nebo je načítat přes proměnné.

> Možná to není na první pohled zřejmé, ale u matematického příkladu se nezapisuje znak rovnítka (`=`). Z toho tedy vyplývá, že lze řešit jen jednoduché `binární operace`, které vždy pracují jen s `dvěma prvky` (s rovnicemi nepočítejte, na to se musí použít specializovaná knihovna).

Přehled základních operátorů
----------------------------

> Ve sloupci `výsledek` uvádím, co se vypíše za předpokladu, že:

```php
$a = 5;
$b = 3;
$c = 10;
```

| Operace			| Značka    | Značka slovně	| Příklad zápisu		| Výsledek
|-------------------|-----------|---------------|-----------------------|----------
| Sčítání			| `+`		| Plus			| `echo $a + $b;` 		| 8
| Odčítání			| `-`		| Mínus 		| `echo $a - $b;` 		| 2
| Násobení			| `*`		| Hvězdička		| `echo $a * $b;` 		| 15
| dělení			| `/`		| Lomítko 		| `echo $a / $b;` 		| 1.66666666667
| Závorky			| `( )`	    | Závorky 		| `echo $a + ($b * $c);`| 35
| Spojování řetězců | `.`		| Tečka     	| `echo $a . $b . $c;`	| 5310
| Zbytek po dělení	| `%`		| Procento 		| `echo $a % $b;` 		| 2
| Přičtení jedničky	| `++`	    | dva plusy		| `echo $a++;`			| 6
| Odečtení jedničky	| `--`	    | dva mínusy	| `echo $a--;`			| 4
| Mocnina			| `**`      | dvě hvězdičky	| `echo $a ** $b;`		| 125

> Operátor mocniny (dvojitá hvězdička) je dostupný až od PHP 7.1. V jiných verzí PHP je nutné použít univerzální funkci `pow($a, $b)`.

Přehled základních matematických funkcí
---------------------------------------

Pokud řešíte nějaký běžný výpočetní úkol, tak s největší pravděpodobností již existuje funkce přímo v PHP, zde je jejich komentovaný přehled.

Zaokrouhlování
--------------

Běžné zaokrouhlení se provádí funkcí <a href="/funkce-round">round()</a>, má 3 parametry:

- Zaokrouhlované číslo
- Nepovinný: Přesnost (na kolik desetinných míst)
- Nepovinný: Půlící hodnota (od jaké hodnoty se má zaokrouhlovat nahoru)

```php
round(5);               // 5
round(5.1);             // 5
round(5.4);             // 5
round(5.5);             // 6
round(5.8);             // 6
round(5483.47621, 2);   // 5483.47
round(5483.47621, -2);  // 5500
```

Dále existuje funkce <a href="funkce-floor">floor()</a> pro zaokrouhlení směrem dolů a funkce <a href="/funkce-ceil">ceil()</a> pro směr nahoru.

> **Pozor na datové typy!**
>
> Všechny zaokrouhlovací funkce vrací výsledek jako `float` a to i v případě, že je výsledkem celé číslo.
>
> Pokud chceme s výsledkem pracovat jako s celým číslem, je důležité ho ještě dodatečně přetypovat. Například: `echo (int) round(3.14);`

PHP pracuje s různými datovými typy, například u **floatu** se může snadno stát, že výsledkem funkce `round()` nebude číslo s konečným desetiným rozvojem. Pro výpis doporučuji použít funkci `number_format()`, o které pojednávám níže.

Goniometrické funkce
--------------------

Používají se na hodně různých výpočtů a vychází z jednotkové kružnice. Využití je například při vykreslování kružnic, elips, posunů a podobně.

- <a href="/funkce-sin">sin()</a>
- <a href="/funkce-cos">cos()</a>
- <a href="/funkce-tan">tan()</a>

Cyklometrické funkce
--------------------

Výpočet úhlu z `x` a `y`. Převody kartézských a polárních souřadnic.

- <a href="/funkce-asin">asin()</a>
- <a href="/funkce-acos">acos()</a>
- <a href="/funkce-atan">atan()</a>

Hyperbolické funkce
-------------------

Vychází z jednotkové hyperboly. Jejich definice lze jednoduše popsat pomocí přirovnání:

- Elipsa, Kružnice, Kružnice s poloměrem 1
- Hyperbola, Rovnoosá hyperbola, Jednotková hyperbola

Používají se například při generování terénu a fyzikální simulace.

- <a href="/funkce-sinh">sinh()</a>
- <a href="/funkce-cosh">cosh()</a> (řetězovka)
- <a href="/funkce-tanh">tanh()</a>

Hyperbolometrické funkce
------------------------

Vychází z jednotkové hyperboly.

- <a href="/funkce-asinh">asinh()</a>
- <a href="/funkce-acosh">acosh()</a>
- <a href="/funkce-atanh">atanh()</a>

Příklady použití goniometrických funkcí
---------------------------------------

```php
echo sin(30);            // -0.98803162409286
echo sin(deg2rad(30));   // 0.5
echo cos(deg2rad(123));  // -0.54463903501503
echo 1/tan(deg2rad(45)); // 1 (onen cotangens)
echo sin(deg2rad(500));  // 0.64278760968654
echo atan2(deg2rad(50), deg2rad(23)); // 1.1396575860761
```

Kalkulačka - zpracování matematického výrazu
--------------------------------------------

Někdy se může stát, že budeme potřebovat zpracovat matematický výraz jako uživatelský řetězec, tedy třeba `5+2^(1+3/2)`.

V PHP neexistuje jednoduchý způsob, jak takový vstup zpracovat, ale naprogramoval jsem sérii knihoven, které tuto problematiku řeší.

Podrobnosti si přečtěte v samostatném článku <a href="/pokrocila-kalkulacka">Kalkulačka v PHP: Zpracování matematického výrazu jako řetězec</a>.

Operace s velkými čísly a přesnost výpočtů
------------------------------------------

Velká čísla a desetinná čísla se v PHP ukládají jako float, navíc se zaokrouhlují přibližně na 15 desetinných míst, což může být někdy nepříjemné. Proto vznikla knihovna **<a href="https://php.net/manual/en/book.bc.php">BCMath Arbitrary Precision Mathematics</a>**.

Čísla se v této knihovně reprezentují jako **string**, proto nejsou limitovány nepřesností **floatu**, nicméně prováděné operace jsou o několik řádů pomalejší (ale stále jde o rychlejší způsob, než kdybychom si takovou funkci naprogramovali sami).

Pokud chceme získat například odmocninu ze 2 na 3 desetinná místa, tak stačí jen zavolat:

```php
echo bcsqrt('2', 3); // 1.414
```
