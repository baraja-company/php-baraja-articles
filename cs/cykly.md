Cykly a jejich druhy v PHP
==========================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 
> perex: "Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh."
> publicationDate: "2020-04-11 18:56:34"
> mainCategoryId: "6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070"

Cykly v obecné rovině slouží k opakování stejného úseku kódu (obvykle nad množinou dat) Pro každý typ úlohy se hodí jiný typ cyklu, který se liší zejména významem a syntaxí. Důležité je také poznamenat, že všechny druhy cyklů jsou mezi sebou převoditelné, jenom se to nemusí vždycky z hlediska složitosti kódu a výpočetního času vyplatit.

Základní rozdělení cyklů z hlediska typu prvků:

- `for`: Obecný cyklus, kde předem známe počet opakování, nebo ho můžeme předem jednoduše vypočítat,
- `while` a `do-while`: Obecný cyklus, kde předem neznáme počet opakování,
- `foreach`: Procházení polí a objektů,
- `rekurze`: Rozložení složitého problému na sadu menších.

Obecné vlastnosti cyklů
-----------------------

Cykly obecně fungují tak, že **stále dokola** opakují označenou část kódu, a to **dokud platí ukončovací podmínka**. Cykl se zastaví buď nesplněním ukončovací podmínky, nebo ručním zastavením zavoláním příkazu `break;`.

Pokud nic cykl nezastaví, tak mu nic nebrání v tom, aby běžel do nekonečna, resp. dokud aplikaci neukončíme ručně, nebo nedojde časový limit pro zpracování PHP scriptu (obvykle 30 sekund).

**Důležité pojmy:**

- `Cykl` - výraz jazyka, který umožňuje opakovat určitou část kódu
- `Iterace` - jedno konkrétní provedení těla cyklu
- `Inicializace` - zahájení provádění cyklu (ještě předtím, než začne první iterace)
- `Inkrementace` - navýšení iterační proměnné o jedničku
- `Ukončovací podmínka` - Podmínka, která se ověřuje v každé iteraci (místo ověření se liší podle typu cyklu). Cykl běží, dokud podmínka platí

For cyklus
----------

`For` se hodí pro případy, kdy předem známe počet opakování (nebo lze snadno vypočítat). Perfektně se hodí třeba na lineární procházení intervalů.

Obecně se zapisuje (3 části):

```php
for (inicializace; výraz; inkrementace) {
    // tělo cyklu
}
```

- `inicializace`: Definice počátečního stavu cyklu (jaké potřebujeme proměnné),
- `výraz`: Podmínka. Dokud platí, tak se bude cykl vykonávat,
- `inkrementace`: Změna stavu cyklu oproti předchozímu průchodu (`inkrementovat` - navýšit o jedničku).

Příkladem může být například vypsání řady čísel od `0` do `10`:

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

Nebo násobky dvojky od `2` do `100`:

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

For cyklus se hodí na různé triky, jako je třeba <a href="/abeceda-cisla-intervaly">získání abecedy, pole čísel a intervalů</a>.

While a do-while cyklus
-----------------------

`While` cyklus běží, dokud platí podmínka. Rozdíl mezi `while` a `do-while` je v tom, kdy se bude podmínka vyhodnocovat.

```php
while (výraz) {
    // tělo cyklu
}
```

Případně:

```php
do {
    // tělo cyklu
} while(výraz);
```

- `while` nejprve vyhodnotí podmínku a poté proběhne vnitřek cyklu,
- `do-while` nejprve zpracuje vnitřek cyklu a až poté vyhodnocuje podmínku.

Výhoda je zejména v tom, kdy nemůžeme předem poznat, jestli vůbec cyklus někdy proběhne a chceme zaručit, aby proběhl vždycky minimálně jednou (pak použijeme `do-while`).

Příklad:

> Máme v proměnné `$x = 2` uložené číslo. Chceme do proměnné `$y` vygenerovat taky číslo v intervalu `<0; 10>` tak, aby bylo různé, než je v proměnné `$x`. Jednoduše řečeno: Vygenerujte náhodné číslo mezi `0` až `10`, které není `2`.

V takovém případě se hodí použít `do-while` cyklus. Předem nevíme, kolikrát bude muset cyklus proběhnout (můžeme mít smůlu a budeme se stále trefovat do stejného čísla, v takovém případě poběží cykl dlouho), a zároveň chceme zaručit, že proběhne aspoň jednou, než se vyhodnotí podmínka, protože číslo přeci nejdříve musíme vygenerovat.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X : ' . $x . '<br>';
echo 'Y: ' . $y;
```

Foreach
-------

`foreach` se perfektně hodí na procházení polí a objektů. Můžeme získat jen konkrétní hodnoty, ale i klíče.

Pokud chceme získat jen konkrétní hodnoty:

```php
foreach ($data as $value) {
    // zpracování dat
}
```

Případně můžeme získat i klíče:

```php
foreach ($data as $key => $value) {
    // zpracování dat
}
```

`foreach` se bezvadně hodí pro procházení reálných dat - třeba výsledků z databáze, nebo polí s klíči:

```php
$countries = [
    'cz' => 'Česká republika',
    'sk' => 'Slovensko',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ': ' . $name . '<br>';
}
```

Rekurze
-------

Rekurze nastává ve chvíli, kdy funkce nebo metoda volá sama sebe. Slouží k rozložení složitého problému na skupinu menších.

> Pochopení rekurze může být pro začátečníky náročné, protože je založena na myšlence předávání zodpovědnosti.
>
> Funkce vlastně říká něco se smyslu: "Já neumím tento problém vyřešit, ale znám někoho, kdo ano...", tak ho zavolá, on zase někoho, ... až dojde k zavolání finálního členu, který problém rozsekne.

Určitě stojí za zmínku, že lze každý rekurzivní algoritmus přepsat na použití cyklů, kde rekurze není potřeba (platí to i obráceně). Rekurze je dobrý sluha, ale špatný pán. Některé typy problémů pomáhá řešit jednoduše a velice efektivně, na jiné věci se zas hodí procházení přes cykly.

Obecně platí, že rekurze má vysokou paměťovou náročnost, protože vytváří stále nové instance a kontexty funkcí a metod. Například pro procházení stromových struktur to je však jediný rozumný způsob, jak problematiku řešit.

Příklad:

Potřebujeme vypočítat faktoriál čísla `5`, to se v matematice dělá takto:

`5! = 5 * 4 * 3 * 2 * 1`

Tedy, dojde k postupnému vynásobení řady čísel od `1` až po číslo, u kterého nás zajímá faktoriál.

Rekurzivně to lze řešit velice elegantně:

```php
function factorial(int $n)
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Případně ještě kratší implementace s použitím ternárního operátoru:

```php
function factorial(int $n)
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

To samé můžeme přepsat na použití cyklů:

```php
function factorial(int $n)
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

Zápis s cyklem je o něco delší, ale zase má výrazně nižší paměťovou náročnost. Pro výpočet používáme jen jednu proměnnou `$result`, kde průběžně měníme hodnotu a nemusíme stále vytvářet nové instance funkce `factorial()`.

Nekonečné cykly pište hodně opatrně
-----------------------------------

Velice snadno se může stát, že bude cykl nekonečný (toho dosáhneme tak, že nikdy nebude splněna ukončovací podmínka). S tímto bychom měli počítat a případně si cyklus sami zastavit příkazem `break;`.

Například hážeme kostkou, dokud nepadne číslo `6`. Implementace svádí použít `while` cyklus:

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Padla hodnota ' . $value;
      break;
   }
}
```

Zápisem `while(true)` jsme si zajistili nekonečný počet opakování, protože `true` bude pravda vždy. Cykl si musíme tedy sami zastavit konstrukcí `break;`, co když ale hodnota `6` nikdy nepadne a cykl si nikdy nezastavíme?

Já osobně vždy počítám počet opakování a pokud přesáhne nějakou kritickou mez, tak cykl násilně ukončím. Například pokud přesáhneme `1000` pokusů. V takovém případě je vhodnější místo `while` použít spíše `for` a počet průběhů si počítat.

Pokud přesáhneme počet průběhů, je slušnost o tom vývojáře informovat vyhozením výjimky.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Padla hodnota ' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Byl přesáhnut maximální počet hodů.');
}
```
