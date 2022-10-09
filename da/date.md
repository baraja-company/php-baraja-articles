PHP-funktion date(), dato og tid
================================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	da: php-funktion-date-dato-og-tid
> 
> perex:
> 	- 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> 	- 'Registrering af dato og klokkeslæt, aktuel dato, formatering af dato og klokkeslæt og konvertering af form.'
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

Funktionen `date()` er et værktøj til at arbejde med dato og tid. Den anvendes i to tilfælde:

- **Finder den aktuelle tilstand**, dvs. den aktuelle dato, klokkeslæt, ... og udsender det i et bestemt format,
- **Konvertering** af en bestemt dato til et andet format (f.eks. månedsnummer til navn, årsnotationsformat, 12- og 24-timers system, ...).

Eksempel
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> ADVARSEL: PHP udskriver ikke din tid, men tiden på serveren. Derfor kan du få et andet klokkeslæt end det, der er indstillet på din computer.

Indtastningssyntaks
--------------------------

Funktionen kaldes på normal vis, og de enkelte anmodninger angives som funktionsargumenter.

```php
echo date('formateringsmærker', atribut konkrétního času);
```

Formateringsmærkerne angiver, i hvilket format datoen skal udskrives. Markeringer kan omfatte mellemrum, punktum, kolon, bindestreger, bindestreger og andre tegn, som ikke selv er formateringstegn (hvis du vil bruge et formateringstegn, skal du <a href="/carriage-notes">eskrive det</a>). Nedenfor findes en oversigt over de enkelte mærker.

Den anden (valgfrie) attribut angiver den manuelt indtastede dato eller tid, som konverteres og udgives i formatet i henhold til den første parameter. Den skal angives som *timestamp* (kan fås via formateringstagget "U").

Eksempel:

```php
echo date('d. m. Y', 1405856605); // senest den 20. 07. 2014
```

Tabel over tilladte formateringsmærker
--------------------------

| Tegn | Beskrivelse
|------|---------------------
| `Y` | Årstal som fire cifre (f.eks. 1998)
| `y` | Årstal som et dobbeltciffer (f.eks. 98)
| `M` | Engelsk forkortelse af månedens navn (f.eks. Jan)
| `m` | Månedsnummer (01-12)
| `F` | Engelsk månedsnavn (f.eks. januar)
| `D` | Engelsk forkortelse for ugedag (f.eks. Fri)
| `l` | Engelsk navn på ugedagen (f.eks. fredag)
| `N` | Nummer på ugedagen (1 - mandag, 7 - søndag)
| `w` | Nummer på ugedagen (0 - søndag, 1 - mandag, 6 - lørdag)
| `d` | Dag i måneden (01-31)
| `j` | Nummer på månedens dag (1-31)
| `z` | Dag i året (001-365)
| `H` | Time (00-23)
| `h` | Time (01-12)
| `i` | Minut (00-59)
| `s` | Andet (00-59)
| `U` | *Timestamp:* Antal sekunder siden tidens begyndelse (siden 1. januar 1970)
| `S` | Engelsk endelse af det ordinale tal for dagen i måneden
| `A` | AM/PM-indikator
| `a` | Indikator for morgen/eftermiddag (am/pm)
| `P` | Forskel fra Greenwich-tid (GMT) med separator mellem timer og minutter (tilføjet i PHP 5.1.3), for eksempel: `+02:00`
| `g` | Timer i 12-timers format (1-12)
| `G` | Timer i 24-timers format (0-23)

Tidsformatering for sitemap
---------------------------------

Meget ofte har du brug for at formatere tiden for filen `sitemap.xml`, som indeholder dato og klokkeslæt for den seneste ændring i tagget `<lastmod>`.

Fra og med PHP 5.1.3 kan følgende syntaks bruges til at gøre dette:

```php
date('Y-m-d\TH:i:sP');
```

Dato og klokkeslæt vises med en nøjagtighed på et sekund og indeholder også oplysninger om den tidszone, hvor serveren er placeret (tidszonen bestemmes af serverens operativsystem).

Få tjekkiske navne på dage og måneder
----------------------------------

Det er ikke muligt at få de tjekkiske navne på dage og måneder i PHP på normal vis, så vi er nødt til selv at skrive disse værdier. Den bedste måde er at gemme posterne i et array og hente dem ved hjælp af et indeksopkald.

```php
$mesice = [
    1 => 'Januar', 'Februar', 'Marts', 'April', 'maj',
    'Juni', 'Juli', 'August', 'September', 'Oktober',
    'November', 'December'
];

$dny = ['Søndag', 'Mandag', 'Tirsdag', 'Onsdag', 'Torsdag', 'Fredag', 'Lørdag'];
```

Dette eksempel er et forenklet eksempel fra <a href="https://php.vrana.cz">**Jakub Vrana**</a> fra artiklen <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">Czech names of months and days of the week</a>.

Oversættelse af datoer fra engelsk til engelsk
--------------------------------------

Vi har ofte en dato i formatet `Fridag, 13 September` og vi ønsker at oversætte den til engelsk, men hvordan gør vi det? Den bedste måde er at kalde en funktion, f.eks. `datocesky()`, som vi sender den engelske dato til, og den oversætter den.

```php
function datumCesky(string $date): string
{
    $men = [
        'Januar', 'Februar', 'Marts', 'April', 'maj',
        'Juni', 'Juli', 'August', 'September', 'Oktober',
        'November', 'December'
    ];

    $mcz = [
        'Januar', 'Februar', 'Marts', 'April', 'maj',
        'Juni', 'Juli', 'August', 'September', 'Oktober',
        'November', 'December'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        'Mandag', 'Tirsdag', 'Onsdag', 'Torsdag',
        'Fredag', 'Lørdag', 'Søndag'
    ];

    $dcz = [
        'Mandag', 'Tirsdag', 'Onsdag', 'Torsdag',
        'Fredag', 'Lørdag', 'Søndag'
    ];

    return str_replace($den, $dcz, $date);
}
```

Eksempel på anvendelse:

```php
echo datumCesky('Fredag den 13. september'); // fredag den 13. december
```

Denne funktion er bestemt ikke ideel til oversættelse, da den kun erstatter engelske ord med tjekkiske ord, men den kan være tilstrækkelig til mange implementeringer. Ved mere avancerede oversættelser bør du altid sikre den nøjagtige syntaks for at oversætte i en ensartet stil, f.eks. `Fredag, 13. december`.

Finde den første dag i måneden
-----------------------------

Den første dag i april 2018 var en søndag, men hvordan finder man nemt ud af det?

Fra og med PHP 5.1.0 er der en simpel løsning:

```php
echo date('N', strtotime('2018-04-01')); // 1 (mandag), 7 (søndag)
```

I ældre versioner skal vi selv implementere denne funktion:

```php
/**
 * @forfatter Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Da `w`-modifikatoren returnerer output i amerikansk format, justerer jeg stadig dagstallet ved hjælp af en simpel beregning. Funktionen returnerer et heltal mellem 1 (mandag) og 7 (søndag).

Tidsforskydninger/formatkonvertering og datavalidering
--------------------------------------------------

Ofte skal vi springe med et relativt tidsrum (f.eks. +5 dage) eller trække en dato fra en brugers tekstinput for at gøre den gyldig. For at gøre dette kan <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a>-funktionen give følgende syntaks:

```php
echo strtotime('nu');
echo strtotime('10. september 2000');
echo strtotime('+1 dag');
echo strtotime('+1 uge');
echo strtotime('+1 uge 2 dage 4 timer 2 sekunder');
echo strtotime('næste torsdag');
echo strtotime('sidste mandag');
```

Funktionens output er **timestamp** *(universaltid)* af det angivne tidspunkt som et heltal.

> Generelt anbefaler jeg at implementere denne funktion på alle formularer, hvor vi arbejder med brugerens tid på en eller anden måde. Det er bestemt ikke en god idé at tvinge brugeren til at skrive datoen i et bestemt format, men opret altid et sådant format automatisk, så brugeren kan skrive, hvad han/hun vil. For ofte er teksten f.eks. kopieret et sted fra, og det er for meget arbejde at omformatere den manuelt, når det kan gøres automatisk.

Antal sekunder fra tidens start
--------------------------

Siden den 1. januar 1970 er hvert sekund blevet lagt sammen med et for at få et stort heltal, der angiver den tid, der er gået siden da. Dette er især nyttigt til simpel beregning af tidsforskelle. Det er en ret pålidelig løsning, men den har sine risici (2038-problemet).

**Generelle egenskaber ved denne notation:**
- Det er et heltal, så det er nemt at arbejde med,
- Det er i decimalsystemet, så det er let for mennesker at læse (bortset fra at man ikke hurtigt kan se, hvad klokken er),
- Det kan ikke bruges til at repræsentere perioden før 1. januar 1970, fordi det ikke kan være negativt *(nogle nye versioner af algoritmen implementerer negative tider, men det kan man ikke altid stole på)*,
- I 2038 vil det ikke længere fungere på 32 bit-computere og vil få programmet til at gå ned (på grund af stackoverløb og maksimal størrelse af heltal).

Problemet i 2038
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">Jeg læser ikke...</h1>

<script>
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>

> Den aktuelle tidsværdi vises over denne tekst, som forhøjes med +1 hvert sekund. Den aktuelle værdi indlæses af javascript, så den er måske ikke altid nøjagtig (den angiver din computers systemtid).

Computere lagrer heltal i binær form, og hvis det er et heltal, har det ofte 32 bit (32 enere og nuller). Det højest mulige 32-bit-tal er: `011 111 111 111 111 111 111 111 111 111 111 111 111 111 111 111 11`.

Men hvis vi tilføjer +1 til denne konstant hvert sekund, vil vi en dag få et sådant tal *(det vil være den 19. januar 2038 kl. 03:14:07)*. Men når vi forsøger at tilføje endnu en 1, vil bitområdet ikke længere være tilstrækkeligt (tallet vil være større end det, der kan lagres i 32 bits), så der vil ske et **stack overflow**, dvs. der vil blive tilføjet en 1 i begyndelsen af tallet, hvilket i binær form betyder et **negativt tal** (så dette vil sandsynligvis få programmet til at gå ned), hvilket vil føre os et sted i år 1901 (ugh!).

Der er to løsninger på dette problem:

- Udvidelse af det hele talområde fra 32 bit til 64 bit, hvilket giver os et tidsrum på flere tusinde år
- Brug datatypen `\DateTime` (almindeligvis tilgængelig i PHP), som giver en objektorienteret tilgang til datostyring, er godt kompatibel med databasen og tilbyder en række årstal fra `0001` til `9999`, hvilket er tilstrækkeligt til behovene i de fleste virkelige applikationer.

Personligt anbefaler jeg at bruge datatypen `\DateTime` og slet ikke at bruge heltalslagring.

Forskellige tidszoner
-----------------

PHP er intelligent, så det forsøger altid at bruge den bedste tidszone, hvor serveren er placeret. Nogle gange kan det dog ske, at tidszonen er forkert angivet, eller vi programmerer et globalt program, hvor brugere fra hele verden har adgang til det, og derfor skal vi ændre tidszonen manuelt.

Dette kan gøres globalt for hele PHP-scriptet ved at kalde en funktion:

```php
date_default_timezone_set('UTC');
```

Lejlighedsvis kan du også støde på denne løsning (der registrerer, at serveren har en anden tidszone end UTC):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Ændre dato og klokkeslæt
--------------------------

Det er ikke PHP selv, der er ansvarlig for at få den aktuelle dato og klokkeslæt, men operativsystemet, som det kører på. Så hvis du har brug for at ændre tiden manuelt, skal du ændre den der.
