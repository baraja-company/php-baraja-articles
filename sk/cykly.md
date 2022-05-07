Cykly a ich typy v jazyku PHP
=============================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	sk: cykly-a-ich-typy-v-jazyku-php
> 
> perex: 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Slučky sa vo všeobecnosti používajú na opakovanie toho istého úseku kódu (zvyčajne nad množinou údajov) Pre každý typ úlohy je vhodný iný typ slučky, ktorý sa líši najmä významom a syntaxou. Dôležité je tiež poznamenať, že všetky typy slučiek sú navzájom konvertibilné, len sa to nemusí vždy oplatiť z hľadiska zložitosti kódu a výpočtového času.

Základné rozdelenie slučiek z hľadiska typu prvku:

- `for`: Všeobecný cyklus, pri ktorom vopred poznáme počet opakovaní alebo ho môžeme jednoducho vopred vypočítať,
- `while` a `until-while`: Všeobecný cyklus, pri ktorom vopred nepoznáme počet opakovaní,
- `foreach`: Prechádzanie polí a objektov,
- `Rekurzia`: Rozloženie zložitého problému na súbor menších problémov.

Všeobecné vlastnosti slučiek
-----------------------

Cykly vo všeobecnosti fungujú tak, že sa označená časť kódu **opakuje stále dokola**, **pokiaľ platí ukončovacia podmienka**. Cyklus sa zastaví buď nesplnením ukončovacej podmienky, alebo ručným zastavením volaním `break;`.

Ak cyklus nič nezastaví, nič mu nebráni v tom, aby bežal donekonečna, alebo kým aplikáciu neukončíte ručne, alebo kým nevyprší časový limit na spracovanie skriptu PHP (zvyčajne 30 sekúnd).

**Dôležité pojmy:**

- `Cycle` - jazykový výraz, ktorý umožňuje opakovať určitú časť kódu
- `Iterácia` - jedno konkrétne vykonanie tela slučky
- `Initialization` - začiatok vykonávania cyklu (pred začiatkom prvej iterácie)
- `Increment` - inkrementácia iteračnej premennej o jednotku
- `Exit condition` - Podmienka, ktorá sa overuje pri každej iterácii (miesto overenia sa líši podľa typu cyklu). Cyklus beží tak dlho, kým platí podmienka

Slučka For
----------

Funkcia `For` je užitočná v prípadoch, keď je počet opakovaní vopred známy (alebo sa dá ľahko vypočítať). Je ideálny napríklad na lineárne prechádzanie intervalov.

Všeobecne sa píše (3 časti):

```php
for (inicializace; výraz; inkrementace) {
    // telo cyklu
}
```

- `Initialization`: Definícia počiatočného stavu cyklu (aké premenné potrebujeme),
- `výraz`: Podmienka. Pokiaľ je platný, cyklus sa vykoná,
- `increment`: Zmena stavu slučky z predchádzajúceho priechodu (`increment` - prírastok o jedna).

Príkladom môže byť výstup série čísel od `0` do `10`:

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

Alebo násobky dvoch od `2` do `100`:

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

Cyklus for je užitočný na rôzne triky, ako napríklad <a href="/abeceda-cisla-intervaly">získanie abecedy, poľa čísel a intervalov</a>.

Kým cyklus do-while
-----------------------

Cyklus `While` beží tak dlho, kým platí podmienka. Rozdiel medzi `while` a `do-while` je v tom, kedy sa podmienka vyhodnotí.

```php
while (výraz) {
    // telo cyklu
}
```

Prípadne:

```php
do {
    // telo cyklu
} while(výraz);
```

- Funkcia `while` najprv vyhodnotí podmienku a potom spustí vnútorný cyklus,
- `do-while` najprv spracuje vnútorný cyklus a potom vyhodnotí podmienku.

Je to výhodné najmä vtedy, keď nemôžeme vopred vedieť, či cyklus niekedy prebehne, a chceme zaručiť, že vždy prebehne aspoň raz (vtedy používame `do-while`).

Príklad:

> V premennej `$x = 2` máme uložené číslo. Chceme tiež vygenerovať číslo v intervale `<0; 10>` v premennej `$y` tak, aby bolo iné ako číslo v premennej `$x`. Jednoducho povedané: vygenerujte náhodné číslo v rozsahu `0` až `10`, ktoré nie je `2`.

V tomto prípade je vhodné použiť cyklus `do-while`. Dopredu nevieme, koľkokrát bude musieť cyklus prebehnúť (môžeme mať smolu a stále narážať na to isté číslo, v takom prípade bude cyklus bežať dlho), a tiež chceme zaručiť, že prebehne aspoň raz pred vyhodnotením podmienky, pretože najprv musíme vygenerovať číslo.

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

`foreach` je ideálny na prehľadávanie polí a objektov. Môžeme získať nielen konkrétne hodnoty, ale aj kľúče.

Ak chceme získať len konkrétne hodnoty:

```php
foreach ($data as $value) {
    // spracovanie údajov
}
```

Prípadne môžeme získať kľúče:

```php
foreach ($data as $key => $value) {
    // spracovanie údajov
}
```

`foreach` je ideálny na prezeranie skutočných údajov - napríklad výsledkov z databázy alebo polí s kľúčmi:

```php
$countries = [
    'cz' => "Česká republika,
    'en' => "Slovensko,
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ': ' . $name . '<br>';
}
```

Rekurzia
-------

Rekurzia nastáva vtedy, keď funkcia alebo metóda volá samu seba. Slúži na rozdelenie zložitého problému na skupinu menších problémov.

> Pochopenie rekurzie môže byť pre začiatočníkov náročné, pretože je založená na myšlienke odovzdávania zodpovednosti.
>
> Funkcia vlastne povie niečo v zmysle "Neviem vyriešiť tento problém, ale poznám niekoho, kto to vie...", takže mu zavolá, on niekomu zavolá, ... kým sa nezavolá posledný člen, ktorý problém odstráni.

Určite stojí za zmienku, že každý rekurzívny algoritmus sa dá prepísať tak, aby používal slučky tam, kde rekurzia nie je potrebná (platí to aj naopak). Rekurzia je dobrý sluha, ale zlý pán. Pomáha jednoducho a veľmi efektívne riešiť niektoré typy problémov, zatiaľ čo cyklus je užitočný na iné veci.

Vo všeobecnosti je rekurzia náročná na pamäť, pretože neustále vytvára nové inštancie a kontexty funkcií a metód. Avšak napríklad pri prechádzaní stromových štruktúr je to jediný rozumný spôsob riešenia problému.

Príklad:

Potrebujeme vypočítať faktoriál čísla `5`, takto sa to robí v matematike:

`5! = 5 * 4 * 3 * 2 * 1`

To znamená, že existuje postupné násobenie radu čísel od `1` až po číslo, ktorého faktoriál nás zaujíma.

Rekurzívne sa to dá vyriešiť veľmi elegantne:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Alternatívou je ešte kratšia implementácia pomocou ternárneho operátora:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

To isté sa dá prepísať na použitie cyklov:

```php
function factorial(int $n): int
{
    $result = 1;

    for ($i = $n; $i > 0; $i--) {
        $result *= $i;
    }

    return $result;
 }
```

Zápis pomocou cyklu je o niečo dlhší, ale opäť má výrazne menšiu pamäťovú stopu. Na výpočet používame len jednu premennú `$result`, ktorej hodnotu priebežne meníme a nemusíme vytvárať nové inštancie `factorial()`.

Nekonečné slučky píšte veľmi opatrne
-----------------------------------

Veľmi ľahko sa môže stať, že cyklus bude nekonečný (dosiahneme to tým, že nikdy nesplníme ukončovaciu podmienku). Mali by ste to vziať do úvahy a prípadne sami zastaviť cyklus pomocou `break;`.

Napríklad hádžte kockou, kým nepadne číslo `6`. Implementácia zvádza k použitiu slučky `while`:

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo "Hodnota klesla . $value;
      break;
   }
}
```

Zápisom `while(true)` sme si zabezpečili nekonečný počet opakovaní, pretože `true` bude vždy pravdivé. Takže musíme sami zastaviť cyklus pomocou konštrukcie `break;`, ale čo ak hodnota `6` nikdy neklesne a my cyklus nikdy nezastavíme?

Osobne vždy počítam počet opakovaní a ak prekročí nejakú kritickú hranicu, cyklus násilne ukončím. Ak napríklad prekročíme počet pokusov `1000`. V takom prípade je lepšie použiť `for` namiesto `while` a spočítať počet behov.

Ak prekročíme počet behov, je slušné informovať vývojára vyhodením výnimky.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo "Hodnota klesla . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception("Maximálny počet hodov bol prekročený.);
}
```
