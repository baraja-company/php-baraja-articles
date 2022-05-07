Paginator och sidoordning av resultat i PHP
===========================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	sv: paginator-och-sidoordning-av-resultat-i-php
> 
> perex:
> 	- Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> 	- Paginering av en lång lista med poster. Hur man löser begränsningen av antalet listade objekt och beräknar paginering.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

När vi har mycket data att dumpa är det lämpligt att dela upp den på flera sidor. Den här artikeln behandlar inte det praktiska genomförandet av att överföra sidnummer och lista resultat, utan endast det teoretiska uttaget av värden och beräkningen av den optimala kodboken för att göra det så användarvänligt som möjligt att bläddra i ett stort antal sidor.

Hur många resultat har vi?
----------------------

Till att börja med måste vi ta reda på hur många resultat vi har. Om uppgifterna kommer från en databas kan de räknas mycket effektivt med följande SQL-anvisning:

```sql
SELECT COUNT(*) FROM tabulka
```

Beräkningen är mycket snabb eftersom databasen för statistik i en hjälpfil, så den rör inte data alls.

Om uppgifterna kommer från någon annan plats (och vi har dem till exempel i en array) kan de räknas med funktionen count():

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Fältet innehåller' . count($cisla) . 'nummer.';
```

Begränsning av antalet resultat
----------------------

Ett annat problem är begränsningen av antalet resultat. Om uppgifterna finns i databasen är det bara att sätta in parametern `LIMIT` i SQL-anvisningen:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```

Det här kommandot ger alltid högst 10 resultat, och det gör också sökningen snabbare eftersom databasen inte behöver gå igenom alla datafiler.

Om vi har data från en annan källa (återigen en array) kan vi också begränsa resultaten på PHP-nivå med hjälp av hjälpvariabeln `$iterator`:

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// Här dumpas data.

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Stoppar cykeln när den har körts 10 gånger.
	}
}
```

Hoppa över de första X resultaten
----------------------

När vi är på första sidan är det ganska enkelt, du behöver bara begränsa antalet resultat med hjälp av `LIMIT`. Men vad händer om jag är på den tredje sidan? Då måste vi hoppa över de första `X` resultaten.

I SQL har vi en elegant notation för detta igen:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```

Den hoppar över de 20 första resultaten och begränsar nästa utdata till 10 resultat, så att intervallet `<21 - 30>` visas.

I ren PHP hanteras detta på två sätt.

Om vi känner till indexen i matrisen kan vi börja läsa den från en viss punkt (vilket är mycket snabbt):

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// Här dumpas data.
}
```

För en okänd array måste vi dock använda iteratorn igen och hoppa över objekten:

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

	// på något sätt dumpas data här

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

Visning av den optimala paginatorn/stegstegaren
----------------------

Antag att vi känner till det totala antalet objekt, antalet objekt på sidan och det aktuella sidnumret. Nu vill vi skapa en bar som gör det möjligt att snabbt bläddra på alla sidor med sökresultat. Men eftersom det finns många sidor (i storleksordningen tusentals) kan vi inte räkna upp dem alla på en gång, så vi måste på ett intelligent sätt välja ut några representativa sidor som bäst representerar intervallet mellan sidorna.

Det kan se ut så här:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

Uppdrag:

Jag är på sidan 36 av 72, hur placerar jag sidnumren på bästa sätt?
Ja, genom sekvensen.

> **Tip:** Genom praktisk observation kom jag på att den vänstra delen av Paginatorn bör beräknas genom en aritmetisk sekvens (så att jag kan förflytta mig linjärt med samma antal steg) och den högra delen genom en **geometrisk sekvens**, vilket i sin tur gör det lätt att ta ett stort steg. Så om jag vill komma till en viss sida måste jag först hoppa över ett stort antal onödiga objekt och sedan förfina urvalet genom att gå tillbaka till vänster.

Aritmetisk sekvensteori (vi fortsätter att addera samma tal):

```php
$d = 10;   // stegstorlek
$a[1] = 1; // första elementet
$a[2] = $a[1] + $d; // andra elementet
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // nionde element

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

Geometrisk sekvensteori (multiplicera alltid med samma tal):

```php
$q = 10;   // stegstorlek
$a[1] = 1; // första elementet
$a[2] = $a[1] * $q; // andra elementet
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // nionde element

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
