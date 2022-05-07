Hur du skriver ditt första PHP-skript
=====================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	sv: hur-du-skriver-ditt-foersta-php-skript
> 
> perex:
> 	- Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> 	- Hur skriver du ditt första PHP-skript? Introduktion till PHP för nybörjare.
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Innan vi skriver vårt första PHP-skript måste vi först teoretiskt förklara hur man laddar en sida med PHP.

1. Först ringer användaren upp en specifik URL i sin webbläsare, till exempel `https://baraja.cz`.
2. Sedan skapar webbläsaren en "begäran", som är en särskild begäran till webbservern som skickas ut på Internet. Den innehåller information om den begärda sidan, grundläggande webbläsaridentifiering och inställningar, information om cookies och så vidare.
3. Denna "begäran" skickas över Internet till en webbserver (oftast Apache) som läser begäran och börjar sammanställa ett svar.
4. Eftersom den uppringda webbadressen är ett PHP-skript och begäran gäller en fil som heter `index.php`, läser `Apache` filen `index.php` från rotkatalogen på disken och skickar den till `PHP-tolkaren`, som är ett program som kan bearbeta PHP-koden och bygga `HTML-kod` baserat på den, som sedan skickas tillbaka till användaren.
5. När HTML-koden är kompilerad skickas svaret tillbaka till användaren (detta kallas för ett "svar") och webbläsaren visar sidan på normalt sätt, som om den vore ren HTML.

> Observera att webbläsaren inte får reda på något om innehållet i PHP-skriptet, utan endast bearbetar den genererade HTML:en, så dina skript och ditt serverinnehåll förblir säkra.

Låt oss skapa det första skriptet
----------------------------

> För att skriva ditt första skript förutsätter du att du har en webbserver som körs på din dator. För Windows är <a href="https://www.apachefriends.org/index.html">XAMPP</a> bäst (ladda ner PHP version 7.0 eller senare), och <a href="https://www.apachefriends.org/index.html">XAMPP</a> fungerar på exakt samma sätt på Mac som på Windows. För Linux rekommenderar jag <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">LAMP-server</a> (den här webbplatsen körs också på Lamp-server).

Namnet på PHP-skriptfilen måste sluta med tillägget `.php` så att webbservern vet att vi vill behandla den enligt PHP-reglerna. Låt oss till exempel skapa en fil `index.php` som kommer att innehålla koden för huvudsidan på vår webbplats.

Öppna filen
--------------------------

Öppna filen i en lämplig textredigerare för att skriva källkod.

I Windows är till exempel <a href="https://www.sublimetext.com">Sublime Text</a> ett bra ställe att börja, eftersom det färgar syntaxen (språkreglerna) och gör koden lättare att läsa. Senare rekommenderar jag att du köper <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, som används mycket på företag och ger möjlighet att programmera in flera personer.

Skriva den grundläggande strukturen för en HTML-sida
--------------------------

Du känner förmodligen redan till den grundläggande strukturen för en HTML-sida:

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

All HTML-kod kommer att hanteras på normalt sätt och kommer att vara till stor hjälp vid utformningen av webbplatsen. PHP använder i stor utsträckning principerna för HTML och CSS.

Separering av PHP-skript från HTML-kod
--------------------------

PHP är huvudsakligen ett mallspråk som genererar anpassat innehåll på lämpliga ställen i koden. För att tydligt kunna skilja på vad som är HTML och vad som är PHP måste vi använda en separatortagg.

För närvarande är det bäst att använda notationen med `<?php` och `?>`.

```php
// här är PHP-koden
?>
```

> Vi använder terminatorn `?>` om vi vill använda någon annan HTML-kod. Om det inte finns någon mer HTML-kod i slutet av PHP-skriptet är det bättre att inte inkludera taggen `?>`, så att det inte finns några onödiga vitrymder och radbrytningar i slutet av sidan som textredigeraren kan infoga.

Tidigare har taggen `<?` använts ofta i stället för `<?php`, men den stöds kanske inte alltid.

Wrapper-taggar kan placeras var som helst i HTML-koden, t.ex. i sidans huvuddel:

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

Grundläggande konstruktioner
--------------------------

Bland de mycket grundläggande byggstenarna finns:

- <a href="/echo">Lista innehåll i källkoden</a>
- <a href="/variabel">Variabler</a>
- <a href="/if">Villkor</a>

I det här avsnittet visar vi en enkel listning av innehåll till källkod med hjälp av variabler.

Principen för uppbyggnad av källkod
--------------------------------

Alla konstruktioner (språkuttryck), uttalanden och funktioner är separerade med semikolon för att göra det otvetydigt var den aktuella konstruktionen är giltig från och till.

Ett semikolon följs vanligen av ett radbyte.

Symboliskt skrivet:

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

Utdrag till källkoden
--------------------------

Konstruktionen <a href="/echo">echo</a> används för att lista innehållet.

Den är mycket lätt att använda:

```php
echo 'Hej, världen!';
```

Därefter skrivs texten "Hello world!" ut i HTML-koden. Prova provet.

> Alla andra demonstrationer kommer endast att innehålla PHP-koden. Den omgivande HTML-koden är fri att använda (använd till exempel exemplet i början av den här artikeln).

Variabler
--------------------------

Variabler är virtuella minnesplatser som lagrar data och används för att flytta runt dem. Namnet på en variabel börjar alltid med en `dollar`, följt av själva `namnet` och sedan dess `värde`.

> Jag har sammanfattat en detaljerad beskrivning av hur variabler fungerar i <a href="/variabel">en separat artikel om variabler</a>.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>';
echo $jmenoAutora;
```

> Variabelnamnet bör uttrycka vad variabeln faktiskt innehåller för att göra koden tydligare. Observera också att HTML-tagget `<br>` har satts in för att förskjuta texten. Du bör redan känna till denna tagg från HTML.

Det som skrivs ut i konstruktionen `echo` kallas en sträng (en sekvens av tecken). Enskilda strängar kan sammanfogas med en punkt (`.`) för att minska utmatningen till en enda rad:

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>' . $jmenoAutora;
```

> När strängarna är sammanlänkade med en punkt kan man se det hela som en enda stor sträng.

Variabel verksamhet
--------------------------

Mellan variabler fungerar alla grundläggande <a href="/matematik">matematiska operationer</a> helt intuitivt som förväntat.

Vi definierar två variabler och anger siffror i dem:

```php
$x = 5; // definierar variabeln x med värdet 5
$y = 3; // definierar en variabel y med värdet 3

echo $x + $y; // lägger till variablerna och skriver ut 8
```

> Observera att likhetstecknet (`=`) inte används för att utföra en matematisk operation, så du kan inte skriva ekvationer, till exempel. PHP fungerar som en kalkylator i detta avseende.

Om vi inte vill använda variabler kan vi utföra operationerna direkt. Det spelar alltså ingen roll var verksamheten är belägen och de kan utvärderas var som helst.

```php
echo 5 + 3; // skriver ut 8
```

Alternativt kan vi addera variablerna och lagra resultatet i en annan variabel:

```php
$x = 5;
$y = 3;
$z = $x + $y; // variabeln $z innehåller 8

echo $z; // skriver ut 8
```

I nästa del kommer vi att lära oss grunderna för <a href="/principer för variabler">definition och användning av variabler</a>.
