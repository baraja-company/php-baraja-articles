PHP-funktionen date(), datum och tid
====================================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	sv: php-funktionen-date-datum-och-tid
> 
> perex:
> 	- 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> 	- 'Upptäckt av datum och tid, aktuellt datum, formatering av datum och tid samt formkonvertering.'
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

Funktionen `date()` är ett verktyg för att arbeta med datum och tid. Den används i två fall:

- **Hitta det aktuella tillståndet**, dvs. det aktuella datumet, klockan, ... och att den skrivs ut i ett visst format,
- **Konvertering** av ett visst datum till ett annat format (t.ex. månadsnummer till namn, årsnoteringsformat, 12- och 24-timmarssystem, ...).

Exempel
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> VARNING: PHP skriver inte ut din tid, utan serverns tid. Det kan därför hända att du får en annan tid än den som är inställd på din dator.

Syntax för inmatning
--------------------------

Funktionen anropas på normalt sätt och de enskilda kraven anges som funktionsargument.

```php
echo date('formateringsmärken', atribut konkrétního času);
```

Formateringsmärkena anger i vilket format datumet kommer att skrivas ut. Markeringar kan inkludera blanksteg, punkter, kolon, bindestreck, skiljetecken och andra tecken som inte själva är formateringsmärken (om du vill använda ett formateringsmärke måste du <a href="/carriage-notes">eskriva det</a>). Nedan följer en översikt över varje tagg.

Det andra (valfria) attributet anger det manuellt inmatade datumet eller klockslaget, som konverteras och skrivs ut i formatet enligt den första parametern. Den måste anges som en *tidsstämpel* (kan erhållas med hjälp av formateringstaggen "U").

Exempel:

```php
echo date('d. m. Y', 1405856605); // senast 20. 07. 2014
```

Tabell över tillåtna formateringsmärken
--------------------------

| Tecken | Beskrivning
|------|---------------------
| `Y` | År med fyra siffror (t.ex. 1998)
| `y` | År som en dubbelsiffra (t.ex. 98)
| `M` | Engelsk förkortning av månadens namn (t.ex. Jan)
| `m` | Månadsnummer (01-12)
| `F` | Engelska månadsnamnet (t.ex. januari)
| `D` | Engelsk förkortning av veckodag (t.ex. Fri)
| `l` | Engelska namnet på veckodagen (t.ex. fredag)
| `N` | Nummer på veckodagen (1 - måndag, 7 - söndag)
| `w` | Nummer på veckodagen (0 - söndag, 1 - måndag, 6 - lördag)
| `d` | Månadens dag (01-31)
| `j` | Nummer på månadens dag (1-31)
| `z` | Årets dag (001-365)
| `H` | Timme (00-23)
| `h` | Timme (01-12)
| `i` | Minuter (00-59)
| `s` | Andra (00-59)
| `U` | *Timestamp:* Antal sekunder sedan tidens början (sedan 1 januari 1970)
| `S` | Engelska ändelsen av ordningsnumret för månadens dag
| `A` | AM/PM-indikator
| `a` | Indikator för morgon/eftermiddag (am/pm)
| `P` | Skillnad från Greenwich-tid (GMT) med separator mellan timmar och minuter (läggs till i PHP 5.1.3), till exempel: `+02:00`.
| `g` | Timme i 12-timmarsformat (1-12)
| `G` | Timme i 24-timmarsformat (0-23)

Tidsformatering för webbplatskarta
---------------------------------

Mycket ofta behöver du formatera tiden för filen `sitemap.xml`, som innehåller datum och tid för den senaste ändringen i taggen `<lastmod>`.

Från och med PHP 5.1.3 kan följande syntax användas för att göra detta:

```php
date('Y-m-d\TH:i:sP');
```

Datum och tid visas med närmsta sekund och innehåller även information om den tidszon där servern är placerad (tidszonen bestäms av serverns operativsystem).

Få fram tjeckiska namn på dagar och månader
----------------------------------

Det är inte möjligt att få fram de tjeckiska namnen på dagar och månader i PHP på vanligt sätt, så vi måste skriva dessa värden själva. Det bästa sättet är att lagra posterna i en array och hämta dem med hjälp av ett indexanrop.

```php
$mesice = [
    1 => 'Januari', 'Februari', 'Mars', 'April', 'Maj',
    'Juni', 'Juli', 'augusti', 'September', 'Oktober',
    'November', 'December'
];

$dny = ['Söndag', 'Måndag', 'Tisdag', 'Onsdag', 'Torsdag', 'Fredag', 'Lördag'];
```

Det här exemplet är ett förenklat exempel från <a href="https://php.vrana.cz">**Jakub Vrana**</a> från artikeln <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">Tjeckiska namn på månader och veckodagar</a>.

Översättning av datum från engelska till engelska
--------------------------------------

Ofta har vi ett datum i formatet "fredag den 13 september" och vill översätta det till engelska, men hur gör vi det? Det bästa sättet är att anropa en funktion, till exempel `datumcesky()`, till vilken vi skickar det engelska datumet och den översätter det.

```php
function datumCesky(string $date): string
{
    $men = [
        'Januari', 'Februari', 'Mars', 'April', 'Maj',
        'Juni', 'Juli', 'augusti', 'September', 'Oktober',
        'November', 'December'
    ];

    $mcz = [
        'Januari', 'Februari', 'Mars', 'April', 'Maj',
        'Juni', 'Juli', 'augusti', 'September', 'Oktober',
        'November', 'December'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        'Måndag', 'Tisdag', 'Onsdag', 'Torsdag',
        'Fredag', 'Lördag', 'Söndag'
    ];

    $dcz = [
        'Måndag', 'Tisdag', 'Onsdag', 'Torsdag',
        'Fredag', 'Lördag', 'Söndag'
    ];

    return str_replace($den, $dcz, $date);
}
```

Exempel på användning:

```php
echo datumCesky('Fredagen den 13 september'); // Fredag den 13 december
```

Den här funktionen är definitivt inte idealisk för översättning, eftersom den bara ersätter engelska ord med tjeckiska ord, men den kan vara tillräcklig för många installationer. För mer avancerade översättningar bör du alltid garantera den exakta syntaxen för att översätta till en enhetlig stil, t.ex. `Fridag, December 13`.

Att hitta den första dagen i månaden
-----------------------------

Den första dagen i april 2018 var en söndag, men hur kan man enkelt ta reda på det?

Från och med PHP 5.1.0 finns det en enkel lösning:

```php
echo date('N', strtotime('2018-04-01')); // 1 (måndag), 7 (söndag)
```

I äldre versioner måste vi själva implementera funktionen:

```php
/**
 * @författare Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Eftersom modifiern `w` returnerar utdata i amerikanskt format, justerar jag fortfarande dagnumret genom en enkel beräkning. Funktionen returnerar ett heltal mellan 1 (måndag) och 7 (söndag).

Tidsförskjutningar/formatkonvertering och datumvalidering
--------------------------------------------------

Ofta måste vi hoppa med en relativ tid (t.ex. +5 dagar) eller hämta ett datum från en användares textinmatning för att det ska vara giltigt. För att göra detta kan funktionen <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a> använda följande syntax:

```php
echo strtotime('nu');
echo strtotime('10 september 2000');
echo strtotime('+1 dag');
echo strtotime('+1 vecka');
echo strtotime('+1 vecka 2 dagar 4 timmar 2 sekunder');
echo strtotime('nästa torsdag');
echo strtotime('i måndags');
```

Funktionens resultat är **tidsstämpel** *(universaltid)* för den angivna tiden som ett heltal.

> I allmänhet rekommenderar jag att den här funktionen används på alla formulär där vi på något sätt arbetar med användarens tid. Det är absolut ingen bra idé att tvinga användaren att skriva datumet i ett visst format, men skapa alltid ett sådant format automatiskt så att användaren kan skriva vad han eller hon vill. Ofta är texten till exempel kopierad från någonstans och det är för mycket arbete att formatera om den manuellt när det kan göras automatiskt.

Antal sekunder från början av tiden
--------------------------

Sedan den 1 januari 1970 adderas varje sekund till en för att få ett stort heltal som anger hur lång tid som förflutit sedan dess. Detta är särskilt användbart för att beräkna tidsskillnader. Det är en ganska tillförlitlig lösning, men den har sina risker (problemet 2038).

**Allmänna egenskaper hos denna notation:**
- Det är ett heltal, så det är lätt att arbeta med,
- Den är i decimalsystemet, så den är lätt att läsa för människor (förutom att man inte snabbt kan avgöra vad den exakta tiden är),
- Den kan inte användas för att representera perioden före den 1 januari 1970, eftersom den inte kan vara negativ *(vissa nya versioner av algoritmen implementerar negativa tider, men du kan inte alltid lita på det)*,
- År 2038 kommer det att sluta fungera på 32-bitarsdatorer och kommer att orsaka att programmet kraschar (på grund av stack overflow och maximal storlek på heltal).

Problemet 2038
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">Jag läser inte...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>

> Det aktuella tidsvärdet visas ovanför denna text, som ökas med +1 varje sekund. Det aktuella värdet laddas in av javascript, så det är inte alltid exakt (det anger datorns systemtid).

Datorer lagrar heltal i binär form, och om det är ett heltal har det ofta 32 bitar (32 ettor och nollor). Det högsta möjliga 32-bitarstalet är: `011 111 111 111 111 111 111 111 111 111 111 111 111 111 11`.

Men om vi lägger till +1 till denna konstant varje sekund kommer vi en dag att få ett sådant tal *(det blir den 19 januari 2038 kl. 03:14:07)*. Men när vi försöker lägga till ytterligare en 1 räcker inte längre till (talet skulle bli större än vad som kan lagras på 32 bitar), så det blir ett **stack overflow**, dvs. en 1 läggs till i början av talet, vilket i binär form innebär ett **negativt tal**, (så detta kommer troligen att få programmet att krascha), och vi hamnar någonstans i år 1901 (ugh!).

Det finns två lösningar på detta problem:

- Utvidgning av heltalsområdet från 32 bitar till 64 bitar, vilket ger oss ett spann på flera tusen år.
- Använd datatypen `\DateTime` (allmänt tillgänglig i PHP), som ger ett objektorienterat tillvägagångssätt för datumhantering, är väl kompatibelt med databasen och erbjuder ett antal år från `0001` till `9999`, vilket är tillräckligt för de flesta verkliga tillämpningar.

Personligen förespråkar jag att man använder datatypen `\DateTime` och inte alls använder heltalslagring.

Olika tidszoner
-----------------

PHP är intelligent, så det försöker alltid använda den bästa tidszonen där servern är belägen. Ibland kan det dock hända att tidszonen är felaktigt angiven, eller att vi programmerar en global applikation där användare från hela världen har tillgång till den och därför måste vi ändra tidszonen manuellt.

Detta kan göras globalt för hela PHP-skriptet genom att anropa en funktion:

```php
date_default_timezone_set('UTC');
```

Ibland kan du också stöta på den här lösningen (att servern har en annan tidszon än UTC):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Ändra datum och tid
--------------------------

Det är inte PHP självt som ansvarar för att få fram datum och tid, utan operativsystemet som det körs på. Om du behöver ändra tiden manuellt kan du ändra den där.
