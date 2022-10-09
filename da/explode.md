PHP-funktion Explode - opdeler streng efter separator
=====================================================

> id: '4dc7cec6-0e96-4a6b-aee8-32c8817ba11e'
> slug:
> 	cs: explode
> 	da: php-funktion-explode-opdeler-streng-efter-separator
> 
> perex:
> 	- Rozdělení řetězce na více částí podle oddělovače.
> 	- Opdeling af kæden i flere dele i henhold til separatoren.
> 
> publicationDate: '2019-11-26 11:39:36'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '66725772be4aae0df8af399e7ce2ba07'

Explode bruges til nemt at opdele en streng ved hjælp af et separator.

- Den returnerer de enkelte resultater som et array nummereret fra nul,
- du kan ikke indsætte et array, kun strengen kan indlæses,
- kan ikke ændre afgrænseren under parsing, kan ikke vælge flere afgrænsere.

| Support | PHP 4 og nyere |
|---------------|-----------------|
| Kort beskrivelse | Opdeling af en streng i et array ved hjælp af separator.
| Krav | Ingen
| Bemærk | Du kan ikke indsætte et array, kun en streng.

Ofte har vi brug for at opdele en streng i henhold til en simpel regel. F.eks. en kommasepareret liste over tal.

Funktionen explode() er fantastisk til dette formål, idet den tager separatoren (som adskiller strengen) som første parameter og selve dataene som anden parameter:

```php
$cisla = '3, 1, 4, 1, 5, 9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
	echo $cislo . '<br>';
	// Her kan vi behandle tallene yderligere
}
```

Men hvad nu, hvis tallene er adskilt af kommaer, men der er mellemrum omkring dem?

Løsningen er at analysere efter den mindste fælles delstreng og derefter fjerne uønskede tegn omkring den (mellemrum og andet whitespace):

```php
$cisla = '3, 1,4, 1 , 5 ,9';
$parser = explode(',', $cisla);

foreach ($parser as $cislo) {
     echo trim($cislo) . '<br>';
     // Her kan vi behandle tallene yderligere
}
```

I dette tilfælde fjerner funktionen `trim()` på en elegant måde whitespace omkring tegnene (mellemrum, tabulatorer, linjeskift, ...) og giver kun rene data.

Begrænsning, begrænser antallet af analyserede enheder i et array
--------------------------

> TIP: For mange eksempler er explode() ikke egnet, og det er meget bedre at bruge regulære udtryk.

Ofte ønsker vi dog kun at analysere dataene op til en vis afstand, og den tredje (valgfrie) parameter limit kan bruges til dette formål.

Lad os f.eks. have strukturerede data med kolon-separerede data, hvor vi ønsker at hente indholdet efter det første kolon og ignorere de andre kolontegn.
Eksempel:

```php
$cas = 'format: "j.n.Y - H:i"';
```

Hvis vi kun analyserer strengen som:

```php
$parser = explode(':', $cas);
```

Vi ville få disse 3 understrenge (i andre tilfælde kan der være mange flere):

```php
'format'
'"j.n.Y - H'
'i"'
```

Derfor sætter vi en grænse for, hvor mange elementer der skal hentes (og eventuelt vil alle andre blive tilføjet til slutningen af det sidste element):

```php
$parser = explode(':', $cas, 2);

// komme tilbage:
echo $parser[0]; // format
echo $parser[1]; // "j.n.Y - H:i"
```

> **Note:** Uønskede anførselstegn kan fjernes fra strengen, f.eks. ved at bruge funktionen `trim()`:

```php
echo trim($parser[1], '"'); // den anden parameter angiver det kort over de tegn, der skal fjernes
```

Mellem, få en streng mellem to strenge
--------------------------

Ofte har vi brug for at få en streng, der er afgrænset af to andre strenge. Der findes ingen funktion til dette direkte i PHP, men vi kan skrive den selv:

```php
function between(string $left, string $right, string $data): string
{
   $l = explode($left, $data);
   $r = explode($right, $l[1]);

   return $r[0];
}
```

Regelmæssige udtryk
--------------------------

Meget mere elegant opdeling og arbejde med strenge kan opnås ved hjælp af <a href="/regex">regulære udtryk</a>, som jeg diskuterer på en separat side.

Lignende funktioner
--------------------------

- <a href="/function-implode">Implode()</a> - sammenkædning af et array til en streng

Eksempel på parsing af IP-adresse
--------------------------

```php
$ip = '10.0.0.138';
$parser = explode('.', $ip);
echo $parser[0]; // udskriver "10"
echo $parser[1]; // udskriver "0";
```

Variablen `$ip` indeholder en indtastningsstreng, der analyseres i overensstemmelse med afgrænseren `.`, og resultatet er et array. Analyseringen fortsætter til slutningen af strengen, medmindre der er angivet en grænse.

Parametre
--------------------------

| # | Type | Beskrivelse
|---|--------|------|
| 1 | string | Adskillelsesstreng.
| 2 | string | Parsed string.
| 3 | int | parsing limit. Dette er en valgfri parameter. Eksempel:

```php
$text = 'PHP er mit yndlingssprog!';
$parser = explode('', $text, 1);

echo $parser[0]; // udskriver det første ord
echo $parser[1]; // udskriver resten af teksten
echo $parser[2]; // udsender ikke noget, fordi der er fastsat en grænse!
```

Returneringsværdier
--------------------------

Returværdien er et array med en analyseret streng.

Indeksene er nummereret fra nul til `X`, medmindre der er angivet en grænse.

Forskelle fra tidligere versioner
--------------------------

| PHP-version | Beskrivelse |
|-----------|-------|
| 5.1.0 | Der er tilføjet understøttelse af negativ grænse for overførsler.
| 4.0.1 | Tilføjet valgfri parameter **limit**.

Tips og noter
--------------------------

Når der anvendes en negativ **grænse**, angives antallet af elementer fra slutningen af strengen.

Eksempel:

```php
$str = 'en|to|tre|fire';

// positiv grænse
print_r(explode('|', $str, 2));

// negativ grænse (siden PHP 5.1)
print_r(explode('|', $str, -1));
```

Følgende felter vil blive returneret:

```php
Array
(
    [0] => one
    [1] => two|three|four
)

Array
(
    [0] => one
    [1] => two
    [2] => three
)
```
