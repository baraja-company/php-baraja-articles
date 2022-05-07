Cyklar och deras typer i PHP
============================

> id: e2927790-d0fb-4de5-9848-01fdd088464b
> slug:
> 	cs: cykly
> 	sv: cyklar-och-deras-typer-i-php
> 
> perex:
> 	- 'Cykl for, while, foreach a rekurze v PHP + vysvětlení rozdílů mezi cykly. Možnosti iterativního zpracování úloh.'
> 	- 'Cycle for, while, foreach och rekursion i PHP + förklaring av skillnaderna mellan cyklerna. Möjlighet till iterativ behandling av uppgifter.'
> 
> publicationDate: '2020-04-11 18:56:34'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '297931d6e7b8c9e011dbbe06b414cdde'

Slingor används i allmänhet för att upprepa samma kodavsnitt (vanligtvis över en datamängd). För varje typ av uppgift är en annan typ av slinga lämplig, som främst skiljer sig åt i betydelse och syntax. Det är också viktigt att notera att alla typer av slingor kan konverteras mellan varandra, men det är inte alltid värt det i fråga om kodkomplexitet och beräkningstid.

Den grundläggande indelningen av slingor efter elementtyp:

- `for`: En generisk slinga där vi känner till antalet upprepningar i förväg eller helt enkelt kan beräkna det i förväg,
- `while` och `until-while`: En allmän slinga där vi inte känner till antalet upprepningar i förväg,
- `foreach`: Bläddrar i matriser och objekt,
- `recursion`: Att dela upp ett komplext problem i en uppsättning mindre problem.

Allmänna egenskaper hos slingor
-----------------------

Cykler fungerar i allmänhet genom att en markerad koddel upprepas **om och om igen**, **så länge som det avslutande villkoret** gäller. Slingan stoppas antingen genom att det avslutande villkoret inte uppfylls eller genom att den stoppas manuellt genom att kalla `break;`.

Om inget stoppar slingan, finns det inget som hindrar den från att köras på obestämd tid, eller tills programmet avslutas manuellt eller tills tidsgränsen för behandling av PHP-skriptet löper ut (vanligtvis 30 sekunder).

** Viktiga villkor:**

- `Cycle` - ett språkuttryck som gör att du kan upprepa en viss del av koden.
- `Iteration` - en specifik exekvering av slingan.
- Initialisering - startar exekveringen av en slinga (innan den första iterationen börjar).
- `Increment` - ökar en iterationsvariabel med ett
- `Exit condition` - Ett villkor som verifieras vid varje iteration (platsen för verifieringen varierar beroende på typ av slinga). Slingan körs så länge villkoret gäller

För slinga
----------

`For` är användbart i fall där vi vet antalet upprepningar i förväg (eller lätt kan beräkna det). Den är perfekt för linjära intervaller, till exempel.

Den är i allmänhet skriven i tre delar:

```php
for (inicializace; výraz; inkrementace) {
    // cykelns huvuddel
}
```

- `Initialisering`: Definition av loopens utgångsläge (vilka variabler vi behöver),
- `expression`: Villkor. Så länge den är giltig kommer slingan att utföras,
- `increment`: Ändra loopens status från föregående gång (`increment` - ökas med ett).

Ett exempel skulle vara att skriva ut en serie siffror från 0 till 10:

```php
for ($i = 0; $i <= 10; $i++) {
    echo $i . '<br>';
}
```

Eller multiplar av två från "2" till "100":

```php
for ($i = 2; $i <= 100; $i += 2) {
    echo $i . '<br>';
}
```

For-slingan är användbar för olika knep, till exempel <a href="/abeceda-cisla-intervals">hämta alfabetet, en matris med siffror och intervaller</a>.

Medan en do-while-slinga
-----------------------

En "while"-slinga körs så länge villkoret gäller. Skillnaden mellan `while` och `do-while` är när villkoret utvärderas.

```php
while (výraz) {
    // cykelns huvuddel
}
```

Alternativt:

```php
do {
    // cykelns huvuddel
} while(výraz);
```

- I `while` utvärderas först villkoret och sedan körs den inre slingan,
- `do-while` bearbetar först den inre slingan och utvärderar sedan villkoret.

Detta är särskilt fördelaktigt när vi inte kan veta i förväg om slingan någonsin kommer att köras och vill garantera att den alltid körs minst en gång (då använder vi `do-while`).

Exempel:

> Vi har ett tal lagrat i variabeln `$x = 2`. Vi vill också generera ett tal i intervallet `<0; 10>` i variabeln `$y` så att det skiljer sig från talet i variabeln `$x`. Enkelt uttryckt: Generera ett slumpmässigt tal mellan 0 och 10 som inte är 2.

I det här fallet är det lämpligt att använda en slinga med "gör-det-under-tiden"-slinga. Vi vet inte i förväg hur många gånger slingan måste köras (vi kan ha otur och få samma nummer, vilket innebär att slingan kommer att köras länge), och vi vill också garantera att den kommer att köras minst en gång innan villkoret utvärderas, eftersom vi måste generera numret först.

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

`foreach` är perfekt för att bläddra i fält och objekt. Vi kan inte bara hämta specifika värden utan även nycklar.

Om vi bara vill få fram specifika värden:

```php
foreach ($data as $value) {
    // databehandling
}
```

Alternativt kan vi hämta nycklarna:

```php
foreach ($data as $key => $value) {
    // databehandling
}
```

`foreach` är perfekt för att bläddra i riktiga data - till exempel resultat från en databas eller fält med nycklar:

```php
$countries = [
    'på' => 'Tjeckien',
    'Se' => 'Slovakien',
];

foreach ($countries as $shortcut => $name) {
    echo $shortcut . ':' . $name . '<br>';
}
```

Rekursion
-------

Rekursion uppstår när en funktion eller metod anropar sig själv. Det tjänar till att dela upp ett komplext problem i en grupp mindre problem.

> Att förstå rekursion kan vara en utmaning för nybörjare eftersom det bygger på idén om att lämna över ansvar.
>
> Funktionen säger faktiskt något i stil med "Jag kan inte lösa det här problemet, men jag känner någon som kan...", så den ringer honom, han ringer någon, ... tills den sista medlemmen anropas, och det löser problemet.

Det är värt att notera att alla rekursiva algoritmer kan skrivas om för att använda slingor där rekursion inte behövs (det omvända gäller också). Rekursion är en bra tjänare, men en dålig herre. Det hjälper till att lösa vissa typer av problem på ett enkelt och mycket effektivt sätt, medan det är användbart för andra saker att gå igenom cykler.

I allmänhet är rekursion minneskrävande eftersom den hela tiden skapar nya instanser och kontexter av funktioner och metoder. När det gäller till exempel trädstrukturer är detta dock det enda rimliga sättet att lösa problemet.

Exempel:

Vi behöver beräkna faktorialen för talet 5, så här gör man i matematiken:

`5! = 5 * 4 * 3 * 2 * 1`

Det vill säga, det finns en successiv multiplikation av en serie tal från "1" upp till det tal som vi är intresserade av faktorn för.

Detta kan lösas på ett mycket elegant sätt genom rekursivitet:

```php
function factorial(int $n): int
{
   if ($n === 0) {
      return 1;
   }

   return $n * factorial($n - 1);
}
```

Alternativt kan ett ännu kortare genomförande göras med hjälp av den ternära operatören:

```php
function factorial(int $n): int
{
    return $n === 0 ? 1 : $n * factorial($n - 1);
}
```

Samma sak kan skrivas om för att använda cykler:

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

Att skriva med en cykel tar något längre tid, men har återigen ett betydligt mindre minnesavtryck. Vi använder endast en variabel `$result` för beräkningen, där vi ändrar värdet kontinuerligt och inte behöver skapa nya instanser av `factorial()`.

Skriv oändliga slingor mycket noggrant
-----------------------------------

Det kan vara mycket lätt för en slinga att bli oändlig (vi uppnår detta genom att aldrig uppfylla avslutningsvillkoret). Du bör ta hänsyn till detta och eventuellt stoppa slingan själv med `break;`.

Till exempel, kasta tärningen tills du får siffran "6". Genomförandet lockar dig att använda slingan `while`:

```php
while (true) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Värdet har sjunkit' . $value;
      break;
   }
}
```

Genom att skriva `while(true)` har vi säkerställt ett oändligt antal upprepningar, eftersom `true` alltid kommer att vara sant. Så vi måste stoppa slingan själva med konstruktionen `break;`, men vad händer om värdet `6` aldrig sjunker och vi aldrig stoppar slingan?

Personligen räknar jag alltid antalet upprepningar och om det överskrider en kritisk gräns avslutar jag slingan med våld. Om vi till exempel överskrider 1000 försök. I det fallet är det bättre att använda `for` i stället för `while` och räkna antalet körningar.

Om vi överskrider antalet körningar är det artigt att informera utvecklaren genom att skapa ett undantag.

```php
for ($i = 0; $i <= 1000; $i++) {
   $value = rand(1, 6);

   if ($value === 6) {
      echo 'Värdet har sjunkit' . $value;
      break;
   }
}

if ($i === 1000) {
   throw new \Exception('Det maximala antalet kast har överskridits.');
}
```
