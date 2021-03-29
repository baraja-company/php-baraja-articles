Paginator a stránkování výpisu výsledků v PHP
=============================================

> id: a1450160-e320-414a-8266-128465632e94
> slugCS: paginator
> perex: Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1

Když máme na výpis mnoho dat, tak je slušnost je rozdělit na více stránek. Tento článek neřeší praktickou implementaci předávání čísel stránek a vypisování výsledků, pouze teoretické získání hodnot a výpočet optimálního číselníku tak, aby bylo procházení velkého množství stránek co nejvíce uživatelsky přívětivé.

Kolik máme k dispozici výsledků
----------------------

Na začátku musíme zjistit, kolik máme vůbec k dispozici výsledků. Pokud data pochází z databáze, tak je lze velice efektivně spočítat následujícím SQL příkazem:

```php
SELECT COUNT(*) FROM tabulka
```


Výpočet to je velice rychlý, protože si databáze vede statistiky v pomocném souboru, proto na data vůbec nesahá.

Pokud data pochází odjinud (a máme je například v poli), tak je lze spočítat funkcí count():

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Pole obsahuje ' . count($cisla) . ' čísel.';
```


Omezení počtu výsledků
----------------------

Další problém je omezení počtu výsledků. Pokud jsou data v databázi, tak stačí do SQL příkazu vložit parametr `LIMIT`:

```php
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```


Tímto příkazem dostaneme vždy maximálně 10 výsledků, zároveň bude také dotaz rychlejší, protože databáze nebude muset procházet celé datové soubory.

Pokud máme data z jiného zdroje (zase opět pole), tak můžeme výsledky omezit také na úrovni PHP s použití pomocné proměnné `$iterator`:

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {

	// tady se nějak vypisují data

	$iterator++;
	if ($iterator >= $limit) break; // Zastaví cykl, až proběhne 10x
}
```


Přeskočení prvních X výsledků
----------------------

Když jsme na první stránce, je to celkem jednoduché, to stačí pouze za pomoci `LIMITu` omezit počet výsledků. Ale co když jsem na třetí stránce? Pak musíme prvních `X` výsledků přeskočit.

V SQL na to máme opět elegantní zápis:

```php
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```


Přeskočí prvních 20 výsledků a další výpis omezí na 10 výsledků, vypíše proto tedy interval `<21 - 30>`.

V čistém PHP se toto řeší dvěma způsoby.

Pokud známe indexy pole, tak jej můžeme začít číst až od určitého místa (což je velice rychlé):

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {

	// tady se nějak vypisují data

}
```


Ovšem u neznámého pole musíme opět použít iterátor a položky přeskočit:

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

	// tady se nějak vypisují data

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```


Zobrazení optimálního paginatoru / krokovače
----------------------

Dejme tomu, že známe celkový počet položek, počet položek na stránce a číslo aktuální stránky. Nyní chceme vykreslit lištu, která umožní rychlé procházení všech stránek s výsledky hledání. Protože je ovšem stránek hodně (řádově tisíce), tak nemůžeme vypsat všechny najednou, musíme si proto inteligentně vybrat je nějaké reprezentativní, které nejlépe vystihují rozmezí mezi stránkami.

Vypadat to může třeba takto:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```


Zadání:

Jsem na stránce 36 ze 72, jak optimálně rozmístit čísla stránek?
No přeci přes posloupnost.

> **Tip:** Praktickým pozorováním jsem zjistil, že levou část Paginatoru je vhodné počítat přes aritmetickou posloupnost (abych se mohl lineárně přesouvat o stejný počet kroků) a pravou část přes **geometrickou posloupnost**, která zase umožňuje snadno udělat velký krok. Pokud se tedy chci dostat na nějakou konkrétní stránku, tak nejprve přeskočím velké množství nepotřebných položek a poté zpřesňuji volbu vracením směrem doleva.

Teorie aritmetické posloupnosti (stále přičítáme stejné číslo):

```php
$d = 10;   // velikost kroku
$a[1] = 1; // první prvek
$a[2] = $a[1] + $d; // druhý prvek
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n-1) * $d; // n-tý prvek

function getAritmeticItem($start, $step, $n) {
	return $start + ($n-1) * $step;
}
```


Teorie geometrické posloupnosti (stále násobíme stejným číslem):

```php
$q = 10;   // velikost kroku
$a[1] = 1; // první prvek
$a[2] = $a[1] * $q; // druhý prvek
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n-1); // n-tý prvek

function getGeometricItem($start, $step, $q) {
	return $start * pow($q, $step-1);
}
```



```php
$start = 1;
$current = 36;
$end = 72;
```
