Stránkovač a stránkovanie výsledkov v PHP
=========================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	sk: strankovac-a-strankovanie-vysledkov-v-php
> 
> perex: Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

Keď máme veľa údajov, ktoré chceme vyklopiť, je slušné rozdeliť ich na viacero stránok. Tento článok sa nezaoberá praktickou implementáciou odovzdávania čísel stránok a výpisu výsledkov, iba teoretickou extrakciou hodnôt a výpočtom optimálneho kódového zoznamu, aby bolo prezeranie veľkého počtu stránok čo najjednoduchšie pre používateľa.

Koľko výsledkov máme
----------------------

Na začiatku musíme zistiť, koľko výsledkov vôbec máme. Ak údaje pochádzajú z databázy, možno ich veľmi efektívne spočítať pomocou nasledujúceho príkazu SQL:

SELECT COUNT(*) FROM tabulka

Výpočet je veľmi rýchly, pretože databáza uchováva štatistiky v pomocnom súbore, takže sa vôbec nedotýka údajov.

Ak údaje pochádzajú z iného miesta (a máme ich napríklad v poli), môžeme ich spočítať pomocou funkcie count():

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Pole obsahuje ' . count($cisla) . ' čísla.;
```

Obmedzenie počtu výsledkov
----------------------

Ďalším problémom je obmedzenie počtu výsledkov. Ak sú údaje v databáze, stačí do príkazu SQL vložiť parameter `LIMIT`:

SELECT * FROM tabulka WHERE (cokoli) LIMIT 10

Týmto príkazom sa vždy získa maximálne 10 výsledkov a dopyt bude rýchlejší, pretože databáza nebude musieť prechádzať celé dátové súbory.

Ak máme údaje z iného zdroja (opäť pole), môžeme výsledky obmedziť aj na úrovni PHP pomocou pomocnej premennej `$iterator`:

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// sem sa vypúšťajú údaje

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Zastaví cyklus, keď prebehne 10-krát
	}
}
```

Vynechanie prvých X výsledkov
----------------------

Keď sme na prvej stránke, je to celkom jednoduché, stačí obmedziť počet výsledkov pomocou `LIMIT`. Ale čo keď som na tretej strane? Potom musíme preskočiť prvé výsledky `X`.

V jazyku SQL na to máme opäť elegantný zápis:

SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20

Vynechá prvých 20 výsledkov a obmedzí ďalší výstup na 10 výsledkov, takže vypíše interval `<21 - 30>`.

V čistom PHP sa to rieši dvoma spôsobmi.

Ak poznáme indexy poľa, môžeme ho začať čítať od určitého bodu (čo je veľmi rýchle):

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// sem sa vypúšťajú údaje
}
```

V prípade neznámeho poľa však musíme opäť použiť iterátor a položky preskočiť:

```php
$pole = [...];

$iterator = 0;
$start = 20;
$limit = 10;
foreach ($pole as $prvek) {
	if ($iterator < $start) {
		$iterator++;
		continue;
	}

	// nejakým spôsobom sa sem vypisujú údaje

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

Zobrazenie optimálneho stránkovača/prechodníka
----------------------

Predpokladajme, že poznáme celkový počet položiek, počet položiek na stránke a číslo aktuálnej stránky. Teraz chceme vykresliť panel, ktorý umožní rýchle prechádzanie všetkých stránok s výsledkami vyhľadávania. Keďže je však stránok veľa (rádovo tisíce), nemôžeme ich uviesť všetky naraz, takže musíme inteligentne vybrať niekoľko reprezentatívnych, ktoré najlepšie reprezentujú rozsah medzi stránkami.

Môže to vyzerať takto:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

Zadanie:

Som na strane 36 zo 72, ako optimálne umiestniť čísla strán?
No, prostredníctvom sekvencie.

> **Tip:** Praktickým pozorovaním som zistil, že ľavú časť Paginatora treba počítať cez aritmetickú postupnosť (takže sa môžem pohybovať lineárne o rovnaký počet krokov) a pravú časť cez **geometrickú postupnosť**, čo zase uľahčuje urobiť veľký krok. Ak sa teda chcem dostať na konkrétnu stránku, najprv preskočím veľké množstvo nepotrebných položiek a potom výber spresním návratom doľava.

Teória aritmetickej postupnosti (stále pridávame to isté číslo):

```php
$d = 10;   // veľkosť kroku
$a[1] = 1; // prvý prvok
$a[2] = $a[1] + $d; // druhý prvok
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // n-tý prvok

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

Teória geometrickej postupnosti (násobenie vždy rovnakým číslom):

```php
$q = 10;   // veľkosť kroku
$a[1] = 1; // prvý prvok
$a[2] = $a[1] * $q; // druhý prvok
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // n-tý prvok

function getGeometricItem(int $start, int $step, int $q): int
{
	return $start * pow($q, $step - 1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
