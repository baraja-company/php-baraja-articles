Betingelser i PHP - IF() {...} - forgreningsmuligheder
======================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	da: betingelser-i-php-if-forgreningsmuligheder
> 
> perex:
> 	- 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> 	- 'Betingelser, boolsk logik, boolsk logik og PHP-scriptforgreningsmuligheder. Evaluering af boolske udtryk og operatorer. Muligheder for foldning af udtryk.'
> 
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '0d8a00928bcfa899d4e6dbf6c067394e'

> **Note:** Denne artikel kan være lidt rodet for begyndere, da den forudsætter en grundlæggende forståelse af PHP. Hvis du er interesseret i, hvordan betingelser fungerer, kan du læse <a href="/betingelser">om betingelser i begynderkurset</a>.

| Understøttelse: | Alle versioner: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Kort beskrivelse: | Validering af en eller flere erklæringer
| Type: | <a href="/statements-and-functions">Statement, konstrukt</a> (ikke en funktion)

Beskrivelse
-----

Ofte skal du afgøre, om en lighed gælder, eller om et udsagn er sandt, og det er det, som betingelser er beregnet til. PHP bruger følgende syntaks ligesom mange andre sprog (især C):

```php
if (/* logisk erklæring */) {
    // konstruere
}
```

Hvert logisk udtryk har værdien `TRUE` (sandt) eller `FALSE` (falsk), der er ingen andre muligheder.

Eksempel på sammenligning af, om variablen `$x` er større end variablen `$y`:

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Denne del af scriptet udføres, hvis betingelsen er sand
} else {
    // Denne del af scriptet køres, hvis betingelsen ikke er opfyldt
}
```

Betingelseskonstruktionen har et obligatorisk indhold i runde parenteser, hvori det udtryk, der skal testes, er angivet, sammensat af operatorer (oversigt nedenfor), flere udtryk kan sammenkædes ved hjælp af logiske operatorer (oversigt nedenfor).

Desuden indeholder betingelsen to valgfrie erklæringsblokke.

- Hvis denne betingelse er opfyldt
- Hvis denne betingelse ikke er opfyldt

Af praktiske årsager skal du altid inkludere mindst den første blok af udsagn, når betingelsen er opfyldt, da det ellers ikke ville give mening at teste udtrykket.

Brug af semikolon og parenteser
--------------------------

Generelt:
- **Runde** parenteser bruges til at adskille logiske udtryk (andre runde parenteser kan bruges til at adskille mere komplekse udtryk).
- **komplekse** parenteser bruges til at afgrænse en blok af kommandoer og funktioner.
- **midten** bruges ikke til at angive en betingelse (blokken af kommandoer er afgrænset af en sammensat parentes), men til at adskille de enkelte kommandoer inden for betingelsen).

Den eneste mulige notation med et semikolon (undtagen ved brug af **endif**-konstruktionen):

```php
if ($x > $y);
```

En sådan betingelse er imidlertid meningsløs, da resultatet af sammenligningen i begge tilfælde vil blive kasseret, og ingen af de sætninger, der hører til betingelsen, vil blive udført.

Alternativ notation
--------------------------

Under visse omstændigheder kan "hvis"-konstruktionen anvendes uden de sammensatte parenteser. Dette kan kun opnås i følgende tilfælde:

- Hvis vi kun udfører én erklæring i betingelsen.
- Hvis vi bruger et kolon og et endif i stedet for sammensatte parenteser;
- Hvis vi bruger "in-line" notation.

Du kan finde mere detaljerede oplysninger i de følgende 3 kapitler.

> **`1. Kun én kommando ~ forkortet syntaks`**

Hvis du opretter en betingelse, hvor du kun ønsker at udføre én konstruktion (udsagn), kan du enten bruge den klassiske sammensatte parentes-formulering:

```php
if ($x > 10) { $y = $x; }
```

Eller vi kan udelade parenteserne:

```php
if ($x > 10) $y = $x;
```

Denne adfærd gælder dog kun for en kommando i umiddelbar nærhed af tilstanden.

Et bedre eksempel (kun konstruktionen `$y = $x` udføres betinget, resten udføres altid):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. kolon og endif;`**

```php
if (/* udtryk */):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Denne notation er imidlertid længe blevet betragtet som forældet, fordi den reducerer orienteringen, når flere betingelser er fordybet i hinanden.

> **Note:** Jeg vil gerne bemærke, at denne stil også er populær hos nogle folk, såsom Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">Se hans artikel</a>). Gud forbyde, at du bruger det et eller andet sted.

> **`3. Ternært udtryk ~ en enkelt linje "in-line" notation`**

Det kan være nyttigt at foretage en simpel in-line-sammenligning sammen med en anden handling (f.eks. sammen med definitionen af en ny variabel). Hvis vi kun ønsker at udføre en enkelt erklæring, kan hele proceduren reduceres til en enkelt linje, samtidig med at den holdes så enkel som muligt.

```php
$x = 5;
$isBiggerThanTwo = ($x > 2 ? true : false);

// eller endnu kortere:

$isBiggerThanTwo = ($x > 2);

// eller uden parenteser:

$isBiggerThanTwo = $x > 2;
```

Operatørtabeller
-----------------

Der anvendes to typer af operatorer i betingelsen:

- **Komparativ** ~ de sammenligner et bestemt forhold,
- **Logisk** ~ kombinere flere udtryk for at skabe komplekse betingelser.

Sammenlignende operatører
-----------------------

| Operatør | Betydning |
|----------|----------|
| `==` | Lig med
| `===` | Er lig med og har den samme datatype
| `!=` | Er ikke lig med
| `>=` | Er lig med eller større
| `<=` | Er lig med eller mindre
| `>` | Større
| `<` | Mindre

Eksempel (gyldigt, når `$x ikke er 5`):

```php
if ($x != 5) { ... }
```

Logiske operatører
--------------------

| Operatør | Alternativ | Betydning | Sandt når:
|-----------|--------------|----------------|--------------------------------------------
| `&&` | AND | og samtidig | begge værdier er sande
| `|||` | OR | eller | mindst én værdi er sand
| `^^` | XOR | eksklusiv OR | mindst én er sand eller falsk, men aldrig begge dele
| `!` | *gør ikke* | negation af udtryk | `true` når `false` og omvendt
| `()` | *gør ikke* | udtryk negation| afhænger af omstændighederne

Et mere komplekst eksempel:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

udeladelse af logiske og sammenligningsoperatorer
---------------------------------------------

Ofte kan vi tillade os at udelade en af operatorerne (eller endda begge), men vi må aldrig glemme reglerne for korrekt brug for at få det resulterende udtryk til at fungere.

Når vi tester et udtryk uden en operatør, tester vi generelt, om dets værdi er `TRUE` eller ikke-tomt (f.eks. om det indeholder et tal, der ikke er nul, en streng, der ikke er tom, ...).

Eksempler:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // godkendes, fordi $x ikke er tomt
if ($x && $y) { ... }   // godkendes, fordi $x og $y ikke er tomme
if (!$x) { ... }        // mislykkes, fordi TRUE er negeret
if (isset($z)) { ... }  // godkendes, fordi variablen $z findes
```

Men der kan opstå vanskelige situationer, især når:
- Jeg spørger efter `hvis ($x)` og variablen `$x` indeholder nul (`0`), så er betingelsen ikke opfyldt.
- Eller også indeholder variablen `$x` strengen ``0``` (tallet nul), fordi den overløber til nul, og udtrykket er derfor ikke sandt.
- Der er en funktion i betingelsen, som altid returnerer en ikke-tom streng, fordi betingelsen så altid er sand.
- Eller hvis vi kontrollerer input fra brugeren, og han returnerer `'false'` som en streng, er betingelsen igen sand, fordi strengen ikke er tom.

Jeg anbefaler en enkel og effektiv løsning på dette problem - spørg efter antallet af tegn, der returneres. Hvis strengen er tom (eller hvis variablen ikke findes), returneres nul tegn, og betingelsen er ikke opfyldt. Et enkelt eksempel:

```php
$x = '0';
if ($x) { ... }			// betingelsen gælder normalt ikke
if (strlen($x)) { ... }	// betingelsen er gyldig, fordi $x indeholder 1 tegn
```

Dernæst kan vi teste, om en variabel findes ved hjælp af funktionen `isset()`.

Sammenligning af strenge
-----------------

Det er nemt at finde ud af, at strengene er identiske:

```php
$a = 'Kat';
$b = 'cat';

if ($a === $b) {
    // Hvis strengene er de samme
} else {
    // Hvis strengene er forskellige
}
```

Det er vigtigt at holde et godt øje med datatyperne, hvis en post kan være ækvivalent med en anden.

For eksempel er den tomme streng `$a = '';` forskellig fra strengen `NULL`: `$b = NULL;`. Vi er nødt til at skelne mellem dette, f.eks. på grund af databaser, hvor der er forskel på, om en værdi ikke findes eller er tom.

```php
$a = '';
$b = null;

if ($a == $b) {
    // Det vil blive evalueret som TRUE, fordi
    // datatypen konverteres.
}

if ($a === $b) {
    // Udfører en meget mere stringent validering
    // og det vil ikke blive godkendt, fordi det er en anden
    // indhold og en anden datatype, og derfor
    // denne kode vil aldrig køre.
}
```

Det er også en god idé at ignorere hvide (usynlige) tegn som f.eks. mellemrum, tabulatorer og linjeskift, når du sammenligner strenge. Dette er f.eks. nyttigt, når du indtaster en adgangskode og sender den til en hashing-funktion:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = '1234';

if (md5(trim($userPassword)) === $password) {
    // Funktionen trim() sletter automatisk mellemrum.
}
```

Ukendt (ikke-eksisterende) værdi?
--------------------------

Nogle gange kan det ske, at værdien ikke eksisterer (den er hverken `TRUE` eller `FALSE`), det er hovedsageligt en værdi, der er hentet fra databasen (f.eks. spørger vi efter en kolonne, der ikke eksisterer), i dette tilfælde returneres datatypen `NULL`.

Generelt evalueres `NULL` som `FALSE`, dvs. at betingelsen ikke gælder. Denne fremgangsmåde er dog ikke altid praktisk, da en ikke-eksisterende værdi ikke nødvendigvis betyder, at der ikke findes nogen registrering.

> Eksempel fra praksis: Vi har en brugerprofil, og vi forespørger på brugerens webside. Ikke alle brugere behøver at have en webside, så i dette tilfælde returneres `NULL`, men brugeren eksisterer stadig. Så i dette tilfælde bør vi hellere bruge funktionen `isset()` til at teste, om variablen (ikke) eksisterer, og ikke drage en konklusion baseret på en bestemt værdi.
