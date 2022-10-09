Paginator og paginering af resultater i PHP
===========================================

> id: a1450160-e320-414a-8266-128465632e94
> slug:
> 	cs: paginator
> 	da: paginator-og-paginering-af-resultater-i-php
> 
> perex:
> 	- Stránkování dlouhého seznamu položek. Jak řešit omezení počtu vypsaných položek a vypočítat stránkování.
> 	- Paginering af en lang liste af emner. Hvordan man løser begrænsningen af antallet af listede elementer og beregner paginering.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: d0bc71ded032401875396b0fd263a820

Når vi har mange data, der skal dumpes, er det smart at dele dem op på flere sider. Denne artikel omhandler ikke den praktiske implementering af videregivelse af sidetal og oplistning af resultater, men kun den teoretiske udtrækning af værdier og beregning af den optimale kodebog for at gøre det så brugervenligt som muligt at gennemse store mængder af sider.

Hvor mange resultater har vi?
----------------------

Til at begynde med skal vi finde ud af, hvor mange resultater vi overhovedet har. Hvis dataene kommer fra en database, kan de tælles meget effektivt med følgende SQL-anvisning:

```sql
SELECT COUNT(*) FROM tabulka
```

Beregningen er meget hurtig, fordi databasen fører statistik i en hjælpefil, så den ikke rører dataene overhovedet.

Hvis dataene kommer et andet sted fra (og vi f.eks. har dem i et array), kan de tælles med funktionen count():

```php
$cisla = [3, 1, 4, 1, 5, 9, 2];

echo 'Feltet indeholder' . count($cisla) . 'numre.';
```

Begrænsning af antallet af resultater
----------------------

Et andet problem er begrænsningen af antallet af resultater. Hvis dataene findes i databasen, skal du blot indsætte parameteren `LIMIT` i SQL-meddelelsen:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10
```

Denne kommando vil altid få maksimalt 10 resultater, og den vil også gøre forespørgslen hurtigere, fordi databasen ikke behøver at gennemgå alle datafiler.

Hvis vi har data fra en anden kilde (igen et array), kan vi også begrænse resultaterne på PHP-niveau ved hjælp af hjælpevariablen `$iterator`:

```php
$pole = [...];

$iterator = 0;
$limit = 10;
foreach ($pole as $prvek) {
	// det er her, dataene dumpes

	$iterator++;
	if ($iterator >= $limit) {
	    break; // Stopper cyklussen, når den har kørt 10 gange
	}
}
```

Spring de første X resultater over
----------------------

Når vi er på den første side, er det ret simpelt, du skal blot begrænse antallet af resultater ved hjælp af `LIMIT`. Men hvad nu, hvis jeg er på tredje side? Så skal vi springe de første `X` resultater over.

I SQL har vi en elegant notation for dette igen:

```sql
SELECT * FROM tabulka WHERE (cokoli) LIMIT 10 OFFSET 20
```

Den springer de første 20 resultater over og begrænser det næste output til 10 resultater, så den udsender intervallet `<21 - 30>`.

I ren PHP håndteres dette på to måder.

Hvis vi kender arrayets indekser, kan vi begynde at læse det fra et bestemt punkt (hvilket er meget hurtigt):

```php
$pole = [...];

$start = 20;
$limit = 10;
for ($i = $start; ($i <= $start + $limit && isset($pole[$i])); $i++) {
	// det er her, dataene dumpes
}
```

For et ukendt felt skal vi imidlertid bruge iteratoren igen og springe elementerne over:

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

	// på en eller anden måde bliver dataene dumpet her

	$iterator++;
	if ($iterator >= $start + $limit) break;
}
```

Visning af den optimale paginator/stepper
----------------------

Lad os antage, at vi kender det samlede antal elementer, antallet af elementer på siden og det aktuelle sidetal. Nu ønsker vi at lave en bjælke, der gør det muligt at gennemse alle sider med søgeresultater hurtigt. Men da der er mange sider (i størrelsesordenen tusindvis), kan vi ikke opregne dem alle på én gang, så vi er nødt til på intelligent vis at vælge nogle repræsentative sider, der bedst repræsenterer intervallet mellem siderne.

Det kan se således ud:

```php
1 | 15 | 30 | 36 | 45 | 60 | 72
```

Opgave:

Jeg er på side 36 ud af 72, hvordan placerer jeg sidetallene optimalt?
Nå, gennem sekvensen.

> **Tip:** Ved praktisk observation fandt jeg ud af, at den venstre del af Paginator skal tælles gennem en aritmetisk sekvens (så jeg kan bevæge mig lineært med det samme antal trin) og den højre del gennem en **geometrisk sekvens**, hvilket igen gør det nemt at tage et stort skridt. Så hvis jeg vil til en bestemt side, skal jeg først springe et stort antal unødvendige elementer over og derefter præcisere valget ved at gå tilbage til venstre.

Aritmetisk sekvensteori (vi bliver ved med at lægge det samme tal sammen):

```php
$d = 10;   // trinstørrelse
$a[1] = 1; // første element
$a[2] = $a[1] + $d; // andet element
$a[3] = $a[1] + 2 * $d;
$a[3] = $a[2] + $d;
$a[$n] = $a[1] + ($n - 1) * $d; // niende element

function getAritmeticItem(int $start, int $step, int $n): int
{
	return $start + ($n - 1) * $step;
}
```

Geometrisk sekvensteori (altid multiplicere med det samme tal):

```php
$q = 10;   // trinstørrelse
$a[1] = 1; // første element
$a[2] = $a[1] * $q; // andet element
$a[3] = $a[1] * $q * $q;
$a[3] = $a[1] * pow($q, 2);
$a[3] = $a[2] * $q;
$a[$n] = $a[1] * pow($q, $n - 1); // niende element

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
