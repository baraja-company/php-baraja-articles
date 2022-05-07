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

```sql
SELECT COUNT(*) FROM tabuľka
```


Tento výpočet je veľmi rýchly, pretože databáza uchováva štatistiky v pomocnom súbore, takže sa vôbec nedotýka údajov.

Ak údaje pochádzajú z iného miesta (a máme ich napríklad v poli), môžeme ich spočítať pomocou funkcie count():

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Pole obsahuje ' . count($cisla) . ' čísla;
```


Obmedzenie počtu výsledkov
----------------------

Ďalším problémom je obmedzenie počtu výsledkov. Ak sú údaje v databáze, stačí do príkazu SQL vložiť parameter `LIMIT`:

```sql
SELECT * FROM tabuľka WHERE (čokoľvek) LIMIT 10
```


Týmto príkazom sa vždy získa maximálne 10 výsledkov a dopyt bude rýchlejší, pretože databáza nebude musieť prechádzať celé dátové súbory.

Ak máme údaje z iného zdroja (opäť pole), môžeme výsledky obmedziť aj na úrovni PHP pomocou pomocnej premennej `$iterator`:

```php
$field = [...];

$iterator = 0;
$limit = 10;
foreach ($field as $prvek) {
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

```sql
SELECT * FROM tabuľka WHERE (čokoľvek) LIMIT 10 OFFSET 20
```


Vynechá prvých 20 výsledkov a obmedzí ďalší výstup na 10 výsledkov, takže vypíše interval `<21 - 30>`.

V čistom jazyku PHP sa to rieši dvoma spôsobmi.

Ak poznáme indexy poľa, môžeme ho začať čítať od určitého bodu (čo je veľmi rýchle):

```php
$field = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($field[$i])); $i++) {
	// sem sa vypúšťajú údaje
}
```


V prípade neznámeho poľa však musíme opäť použiť iterátor a položky preskočiť:

```php
$field = [...];

$iterator = 0;
$start = 20;
$limit = 10;
foreach ($field as $prvek) {
	if ($iterator < $start) {
		$iterator++;
		pokračovať;
	}

	// nejakým spôsobom sa sem vypisujú údaje

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```


Zobrazenie optimálneho paginátora/steppera
----------------------

Predpokladajme, že poznáme celkový počet položiek, počet položiek na stránke a číslo aktuálnej stránky. Teraz chceme vykresliť panel, ktorý umožní rýchle prechádzanie všetkých stránok s výsledkami vyhľadávania. Keďže je však stránok veľa (rádovo tisíce), nemôžeme ich uviesť všetky naraz, takže musíme inteligentne vybrať niekoľko reprezentatívnych, ktoré najlepšie reprezentujú rozsah medzi stránkami.

Môže to vyzerať takto:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```


Zadanie:

Som na strane 36 zo 72, ako optimálne umiestniť čísla strán?
No, prostredníctvom sekvencie.

> **Tip:** Praktickým pozorovaním som zistil, že ľavú časť Paginatora treba počítať pomocou aritmetickej postupnosti (takže sa môžem pohybovať lineárne o rovnaký počet krokov) a pravú časť pomocou **geometrickej postupnosti**, čo zase uľahčuje urobiť veľký krok. Ak sa teda chcem dostať na konkrétnu stránku, najprv preskočím veľké množstvo nepotrebných položiek a potom výber spresním návratom doľava.

Teória aritmetickej postupnosti (stále pridávame to isté číslo):

```php
$d = 10; // veľkosť kroku
$a[1] = 1; // prvý prvok
$a[2] = $a[1] + $d; // druhý prvok
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // n-tý prvok

funkcia getAritmeticItem(int $start, int $step, int $n): int
{
	vrátiť $start + ($n - 1) * $step;
}
```


Teória geometrickej postupnosti (násobíme stále tým istým číslom):

```php
$q = 10; // veľkosť kroku
$a[1] = 1; // prvý prvok
$a[2] = $a[1] * $q; // druhý prvok
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // n-tý prvok

funkcia getGeometricItem(int $start, int $step, int $q): int
{
	vrátiť $start * pow($q, $step - 1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
