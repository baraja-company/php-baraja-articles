Sådan skriver du dit første PHP-script
======================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	da: sadan-skriver-du-dit-forste-php-script
> 
> perex:
> 	- Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> 	- Hvordan skriver du dit første PHP-script? Introduktion til PHP for begyndere.
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Inden vi skriver vores første PHP-script, skal vi først teoretisk forklare, hvordan man indlæser en side med PHP.

1. Først kalder brugeren en bestemt URL i sin webbrowser, f.eks. `https://baraja.cz`.
2. Derefter opretter webbrowseren en `request`, som er en særlig anmodning til webserveren, der sendes til internettet. Den indeholder oplysninger om den ønskede side, grundlæggende browseridentifikation og -indstillinger, oplysninger om cookies osv.
3. Denne "forespørgsel" sendes over internettet til en webserver (oftest "Apache"), som læser forespørgslen og begynder at kompilere et svar.
4. Da den kaldte URL er et PHP-script, og anmodningen vedrører en fil med navnet `index.php`, læser `Apache` filen `index.php` fra rodmappen på disken og sender den videre til `PHP-fortolkeren`, som er et program, der kan behandle selve PHP-koden og bygge `HTML-kode` på grundlag af den, som sendes tilbage til brugeren.
5. Når HTML-koden er kompileret, sendes svaret tilbage til brugeren (dette kaldes et `response`), og webbrowseren gengiver siden på normal vis, som om det var ren HTML.

> Bemærk, at webbrowseren ikke får noget at vide om indholdet af PHP-scriptet, men kun behandler den genererede HTML, så dine scripts og dit serverindhold forbliver sikre.

Lad os oprette det første script
----------------------------

> Når du skriver dit første script, forudsætter det, at du har en webserver kørende på din computer. Til Windows er <a href="https://www.apachefriends.org/index.html">XAMPP</a> bedst (download PHP version 7.0 eller nyere), og <a href="https://www.apachefriends.org/index.html">XAMPP</a> fungerer nøjagtig på samme måde på Mac som på Windows. Til Linux anbefaler jeg <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">LAMP-server</a> (dette websted kører også på Lamp-server).

Navnet på PHP-scriptfilen skal slutte med udvidelsen `.php`, så webserveren ved, at vi ønsker at behandle den i overensstemmelse med PHP-reglerne. Så lad os f.eks. oprette en fil `index.php`, som skal indeholde koden til hovedsiden på vores websted.

Åbning af filen
--------------------------

Åbn denne fil i en passende teksteditor til at skrive kildekode.

I Windows er <a href="https://www.sublimetext.com">Sublime Text</a> f.eks. et godt sted at starte, da det farver syntaksen (sprogreglerne) flot ind og gør koden lettere at læse. Senere vil jeg anbefale at købe <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, som bruges meget i virksomheder og giver mulighed for at programmere flere personer ind.

Skriv den grundlæggende struktur for en HTML-side
--------------------------

Du kender sikkert allerede den grundlæggende opbygning af en HTML-side:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>
```

Al HTML-kode vil blive håndteret på normal vis og vil være en stor hjælp til at designe webstedet. PHP anvender i høj grad principperne i HTML og CSS.

Adskillelse af PHP-script fra HTML-kode
--------------------------

PHP er hovedsageligt et templating-sprog, der genererer brugerdefineret indhold på passende steder i koden. For at kunne skelne klart mellem HTML og PHP skal vi bruge et separatortag.

I øjeblikket er det bedst at bruge notationen med `<?php` og `?>`.

```php
// her er PHP-koden
?>
```

> Vi bruger terminatoren `?>`, hvis vi ønsker at bruge en anden HTML-kode. Hvis der ikke er mere HTML-kode i slutningen af PHP-scriptet, er det bedre ikke at inkludere `?>`-tagget, så der ikke er unødvendige hvidt mellemrum og linjeskift i slutningen af siden, som teksteditoren kan indsætte.

Tidligere er `<?`-tagget ofte blevet brugt i stedet for `<?php`, men det er ikke altid understøttet.

Wrapper-tags kan placeres hvor som helst i HTML-koden, f.eks. i sidens brødtekst:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // tady bude PHP kód
    ?>
    </body>
</html>
```

Grundlæggende konstruktioner
--------------------------

Blandt de helt grundlæggende byggesten er:

- <a href="/echo">Liste over indhold i kildekoden</a>
- <a href="/variabel">Variabler</a>
- <a href="/if">Betingelser</a>

I dette afsnit vil vi demonstrere en simpel oplistning af indhold til kildekode ved hjælp af variabler.

Princippet om opbygning af kildekode
--------------------------------

Alle konstruktioner (sprogudtryk), udsagn og funktioner er adskilt af semikolon for at gøre det utvetydigt, hvor den aktuelle konstruktion er gyldig fra og til.

Et semikolon efterfølges normalt af et linjeskift.

Symbolsk skrevet:

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

Uddrag til kildekode
--------------------------

<a href="/echo">echo</a>-konstruktionen bruges til at liste indholdet.

Den er meget nem at bruge:

```php
echo 'Hej, verden!';
```

Derefter udskriver den teksten "Hello world!" i HTML-koden. Prøv prøven.

> Alle andre demoer vil kun indeholde PHP-koden indeni. Den omgivende HTML-kode kan du selv bestemme (du kan f.eks. bruge eksemplet i begyndelsen af denne artikel).

Variabler
--------------------------

Variabler er virtuelle hukommelsesplaceringer, der gemmer data og bruges til at flytte dem rundt. Navnet på en variabel starter altid med en `dollar`, efterfulgt af selve `navnet` og derefter dens `værdi`.

> Jeg har sammenfattet en detaljeret beskrivelse af, hvordan variabler fungerer, i <a href="/variable">en separat artikel om variabler</a>.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```

> Variabelnavnet bør udtrykke, hvad variablen rent faktisk indeholder, for at gøre koden klarere. Bemærk også, at der er indsat HTML-tag `<br>` for at indrykke teksten. Du bør allerede kende dette tag fra HTML.

Det, der udskrives i `echo`-konstruktionen, kaldes en streng (en sekvens af tegn). Individuelle strenge kan sammenkædes med et punktum (`.`) for at reducere output til en enkelt linje:

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```

> Når strengene er forbundet med et punkt, vil det hele blive opfattet som én stor streng.

Variable operationer
--------------------------

Mellem variabler fungerer alle grundlæggende <a href="/matematik">matematiske operationer</a> helt intuitivt som forventet.

Lad os definere 2 variabler og sætte tal ind i dem:

```php
$x = 5; // definerer variabel x med værdien 5
$y = 3; // definerer en variabel y med værdien 3

echo $x + $y; // tilføjer variablerne og udskriver 8
```

> Bemærk, at lighedstegnet (`=`) ikke bruges til at udføre en matematisk operation, så du kan f.eks. ikke skrive ligninger. PHP fungerer bare som en lommeregner i denne henseende.

Hvis vi ikke ønsker at bruge variabler, kan vi udføre operationerne direkte. Så det er ligegyldigt, hvor operationen foregår, og de vil blive evalueret hvor som helst.

```php
echo 5 + 3; // udskriver 8
```

Alternativt kan vi lægge variablerne sammen og gemme resultatet i en anden variabel:

```php
$x = 5;
$y = 3;
$z = $x + $y; // variabel $z indeholder 8

echo $z; // udskriver 8
```

I næste del vil vi lære det grundlæggende om <a href="/principper-for-variabler">definition og brug af variabler</a>.
