Cyklusser og deres typer i PHP
==============================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	da: cyklusser-og-deres-typer-i-php
> 
> perex:
> 	- 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> 	- 'Cyklus for, while, foreach og rekursion i PHP + forklaring af forskellene mellem cyklusser. Muligheder for iterativ behandling af opgaver.'
> 
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Loops bruges generelt til at gentage den samme kodeafsnit (normalt over et datasæt). For hver type opgave er der forskellige typer af loops, som primært adskiller sig i betydning og syntaks. Det er også vigtigt at bemærke, at alle typer loops kan konverteres mellem hinanden, men det kan ikke altid betale sig med hensyn til kodekompleksitet og beregningstid.

Den grundlæggende opdeling af sløjfer efter elementtype:

- `for`: En generisk løkke, hvor vi kender antallet af gentagelser på forhånd eller blot kan beregne det på forhånd,
- `while` og `until-while`: En generel løkke, hvor vi ikke kender antallet af gentagelser på forhånd,
- `foreach`: Gennemse arrays og objekter,
- `recursion`: Nedbrydning af et komplekst problem i en række mindre problemer.

Generelle egenskaber ved sløjfer
-----------------------

Cyklusser fungerer generelt ved at gentage et markeret kodestykke **og igen og igen**, **så længe den afsluttende betingelse** er opfyldt. Løkken stoppes enten ved at den ikke opfylder den afsluttende betingelse eller ved manuelt at stoppe den ved at kalde `break;`.

Hvis intet stopper sløjfen, er der intet, der forhindrer den i at køre i det uendelige, eller indtil programmet afsluttes manuelt, eller indtil tidsgrænsen for behandling af PHP-scriptet udløber (normalt 30 sekunder).

**Vigtige vilkår:**

- `Cycle` - et sprogudtryk, der giver dig mulighed for at gentage en bestemt del af koden
- `Iteration` - en specifik udførelse af løkkekroppen
- `Initialisering` - starter udførelsen af en løkke (før den første iteration starter)
- `Increment` - forøgelse af en iterationsvariabel med én
- `Exit condition` - En betingelse, der kontrolleres ved hver gentagelse (placeringen af kontrollen varierer alt efter løkketype). Løkken kører, så længe betingelsen er opfyldt

For loop
----------

`For` er nyttig i tilfælde, hvor vi kender antallet af gentagelser på forhånd (eller let kan beregne det). Den er perfekt til lineær gennemløb af f.eks. intervaller.

Den er generelt skrevet (3 dele):

```php
for (inicializace; výraz; inkrementace) {
    // cyklussens krop
}
```

- `Initialisering`: Definition af loopens starttilstand (hvilke variabler vi har brug for),
- `expression`: Betingelse. Så længe den er gyldig, vil løkken blive udført,
- `increment`: Ændrer sløjfens tilstand fra det foregående gennemløb (`increment` - forøgelse med én).

Et eksempel kunne være at udgive en række tal fra `0` til `10`:

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

Eller multipla af to fra `2` til `100`:

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

For-loop'en er nyttig til forskellige tricks, f.eks. <a href="/abeceda-cisla-intervals">hentning af alfabetet, et array af tal og intervaller</a>.

Mens en do-while-loop
-----------------------

En `While`-sløjfe kører så længe betingelsen gælder. Forskellen mellem `while` og `do-while` er, hvornår betingelsen vil blive evalueret.

```php
while (výraz) {
    // cyklussens krop
}
```

Alternativt:

```php
do {
    // cyklussens krop
} while(výraz);
```

- `while` evaluerer først betingelsen og kører derefter den indre sløjfe,
- `do-while` behandler først den indre sløjfe og evaluerer derefter betingelsen.

Dette er især en fordel, når vi ikke på forhånd kan vide, om løkken nogensinde vil blive kørt, og vi ønsker at garantere, at den altid kører mindst én gang (i så fald bruger vi `do-while`).

Eksempel:

> Vi har et tal gemt i variablen `$x = 2`. Vi ønsker også at generere et tal i intervallet `<0; 10>` i variablen `$y`, så det er forskelligt fra tallet i variablen `$x`. Kort sagt: Generer et tilfældigt tal mellem `0` og `10`, som ikke er `2`.

I dette tilfælde er det praktisk at bruge en `do-while`-loop. Vi ved ikke på forhånd, hvor mange gange løkken skal køre (vi kan være uheldige og blive ved med at ramme det samme tal, og i så fald vil løkken køre i lang tid), og vi vil også gerne garantere, at den kører mindst én gang, før betingelsen evalueres, da vi skal generere tallet først.

```php
$x = 2;
$y = $x;

do {
   $y = rand(0, 10);
} while($x == $y);

echo 'X :' . $x . '<br>';
echo 'Y:' . $y;
```

Foreach
-------

`foreach` er perfekt til at gennemse felter og objekter. Vi kan ikke kun hente specifikke værdier, men også nøgler.

Hvis vi kun ønsker at få specifikke værdier:

```php
foreach ($data as $value) {
    // databehandling
}
```

Alternativt kan vi få nøglerne:

```php
foreach ($data as $key => $value) {
    // databehandling
}
```

`foreach` er perfekt til at gennemse rigtige data - f.eks. resultater fra en database eller felter med nøgler:

```php
$countries = [
    'på' => 'Tjekkiet',
    'Se' => 'Slovakiet',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ':' . $name . '<br>';
}
```

Rekursion
-------

Rekursion opstår, når en funktion eller metode kalder sig selv. Det tjener til at opdele et komplekst problem i en gruppe mindre problemer.

> Det kan være en udfordring for nybegyndere at forstå rekursion, fordi det er baseret på idéen om at videregive ansvar.
>
> Funktionen siger faktisk noget i retning af "Jeg kan ikke løse dette problem, men jeg kender nogen, der kan...", så den ringer til ham, han ringer til nogen, ... indtil det sidste medlem bliver kaldt, og så er problemet løst.

Det er bestemt værd at bemærke, at enhver rekursiv algoritme kan omskrives til at bruge loops, hvor rekursion ikke er nødvendig (det omvendte er også tilfældet). Rekursion er en god tjener, men en dårlig mester. Det hjælper med at løse nogle typer problemer enkelt og meget effektivt, mens det er nyttigt at gennemløbe cyklusser til andre formål.

Generelt er rekursion hukommelseskrævende, fordi den hele tiden opretter nye instanser og kontekster af funktioner og metoder. Men når der f.eks. skal gennemløbes træstrukturer, er dette den eneste fornuftige måde at løse problemet på.

Eksempel:

Vi skal beregne faktorialen af tallet `5`, sådan gøres det i matematikken:

`5! = 5 * 4 * 3 * 2 * 1`

Det vil sige, at der er en successiv multiplikation af en række tal fra "1" og op til det tal, som vi er interesseret i faktorialen af.

Dette kan løses meget elegant ved hjælp af en rekursiv metode:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Alternativt kan en endnu kortere implementering ske ved hjælp af den ternære operatør:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

Det samme kan omskrives til at bruge cyklusser:

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

Det tager lidt længere tid at skrive med en cyklus, men har igen et betydeligt mindre hukommelsesaftryk. Vi bruger kun én variabel `$result` til beregningen, hvor vi ændrer værdien løbende og ikke behøver at oprette nye forekomster af `factorial()`.

Skriv uendelige loops meget omhyggeligt
-----------------------------------

Det kan være meget nemt for en løkke at blive uendelig (vi opnår dette ved aldrig at opfylde afslutningsbetingelsen). Du bør tage højde for dette og eventuelt selv stoppe sløjfen med `break;`.

Kast f.eks. terningen, indtil tallet `6` er kastet. Implementeringen er fristet til at bruge en "mens"-sløjfe:

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Værdien er faldet' . $value;
      break;
   }
}
```

Ved at skrive `while(true)` har vi sikret et uendeligt antal gentagelser, fordi `true` altid vil være sandt. Så vi er nødt til selv at stoppe løkken med konstruktionen `break;`, men hvad nu hvis værdien `6` aldrig falder, og vi aldrig stopper løkken?

Personligt tæller jeg altid antallet af gentagelser, og hvis det overstiger en kritisk grænse, afslutter jeg sløjfen med magt. Hvis vi f.eks. overskrider `1000` forsøg. I så fald er det bedre at bruge `for` i stedet for `while` og tælle antallet af kørsler.

Hvis vi overskrider antallet af kørsler, er det høfligt at informere udvikleren ved at kaste en undtagelse.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Værdien er faldet' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Det maksimale antal kast er blevet overskredet.');
}
```
