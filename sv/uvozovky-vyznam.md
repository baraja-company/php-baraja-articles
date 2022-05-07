Särskild innebörd av citattecken
================================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	sv: saerskild-inneboerd-av-citattecken
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

I PHP kan du ofta ersätta citationstecken med apostrofer för att uppnå samma effekt. Ibland använder vi till och med en kombination av citationstecken och apostrofer avsiktligt för att uppnå olika resultat utan att behöva använda escaping.

**TLDR: Det kan vara farligt att använda citationstecken i vissa sammanhang, så använd apostrofer överallt i stället. Angivelsetecken är endast lämpliga för speciella fall.**

| Tecken | Namn |
|------|-----------
| "`"` | Angivenhetstecken |
| `'` | Apostrof |

Exempel ett - samma resultat
-----------------------------

```php
echo "Hej, världen!"; // detta är samma sak
echo 'Hej, världen!'; // så här
```

I detta fall spelar det ingen roll om du använder citationstecken eller apostrofer. På ytan kan det alltså se ut som om det inte finns någon skillnad mellan citationstecken och apostrofer.

Kombination av citationstecken och apostrofer
------------------------------

Tänk på följande kod:

```php
echo 'Min favoritfärg är "röd"!';
```

Om jag använde `carriage returns` för att avgränsa den sträng som skrivs ut skulle det vara **tvetydigt** var strängen **börjar** och var den slutar**, så jag använde `apostrophes` för att avgränsa strängen, vilket löser detta problem.

Infoga citationstecken i en sträng avgränsad av citationstecken
---------------------------------------------------

Ibland kan det finnas situationer där jag behöver lista både `quote` och `apostrophe` i samma sträng. Du kan inte bara använda concatenation av två strängar, utan även så kallad **escaping** av tecken.

```php
echo "Denna sträng \"innehåller\" citationstecken.";
```

Backslash har i det här fallet betydelsen "exakt det här tecknet" och därför kommer citattecknet inte att ses som slutet på en sträng (eller något annat) och därför kommer det alltid att vara ett citattecken. Andra märkliga tecken kan markeras på detta sätt när deras hållbarhet inte kan garanteras och den korrekta betydelsen kan missförstås.

Variabel inom en sträng
-----------------------

Anföringstecken kan vara extremt knepiga eftersom de gör det möjligt att infoga variabler direkt i en sträng:

```php
$color = 'Röd';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Mycket säkrare är apostrofer, som inte tillåter detta och du måste vika strängen:

```php
$color = 'Röd';

echo 'Min favoritfärg är' . $color . 'och din?';
```

Goda vanor
--------------------------

I allmänhet rekommenderar jag att du använder apostrofer (om möjligt) för att avgränsa något, eftersom de är mycket mindre vanliga i strängar än citationstecken.

Dessutom är PHP ett webbspråk, dvs. det används för att generera HTML-dokument, där citationstecken är mycket vanliga just för att de också används för att generera delar av HTML-koden. Personligen rekommenderar jag att du vänjer dig vid att använda apostrofer överallt, för då behöver du inte komma ihåg vad du omsluter.

Beteende för citationstecken
--------------------------

Se upp! Släng inte citattecknen helt och hållet! De har vissa speciella fördelar som kan vara användbara för avancerat PHP-arbete - men nybörjare betraktar dem som fel och förstår dem inte.

```php
$x = 10; // ange en variabel
echo "Hodnota proměnné je: $x, přesně."; // och en förteckning
```

Inom citationstecken kan du också ange värden för variabler, eller så gör dollartecknet att allt efter det blir en variabel. Så om du inte vill visa värdet av en variabel, utan dollartecknet, måste du använda escaped.

```php
$price = 25; // pris i dollar
echo "Cena produktu: $price\$"; // skriver ut "Produktpris: $25"
```

Fördelen med citationstecken kan ifrågasättas i detta fall och det kan vara bättre att använda apostrofer och helt enkelt länka samman strängarna.

```php
$price = 25;
echo 'Produktpris:' . $price . '$'; // skriver ut samma sak som i det föregående exemplet
```

> **Note:** Priset i dollar är korrekt skrivet i formatet `$25`, vilket skulle skapa ännu mer förvirring, eftersom notationen `$$pris` faktiskt kallar något som kallas `variabelvariabel` (i variabelnamnet kallar vi namnet på den variabel som ska kallas - bara förvirring).

**TIP:** Du kanske är intresserad av <a href="/promenna-variable">variabel variabel</a>.

Strängbeteckning
--------------------------

I allmänhet behandlas allt som står inom citationstecken eller apostrofer som en sträng. Således:

```php
$x = 5;   // detta är något annat,
$x = "5"; // än så här.
```

I det första fallet lagras **talet** 5 i variabeln **$x**, i det andra fallet lagras **strängen** "5" i samma variabel. Som tur är spelar detta ingen roll i PHP, och du kan arbeta med båda varianterna (nästan) på samma sätt, eftersom PHP automatiskt **omvandlar** variabler enligt deras innehåll. Det rekommenderas dock i allmänhet att inte skriva siffror inom citationstecken, särskilt när det gäller beräkningsoperationer där avrundningsfel kan uppstå.

Ibland vill jag lagra resultatet av en funktion i en variabel:

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // detta är korrekt
$zaokrouhleni = "round($pi)"; // detta är "fel" (frågan är vilken utdata jag förväntar mig).
```

Felet i det andra fallet är bara i citattecken, då resultatet av funktionen **round()** inte lagras i variabeln **$round**, utan i strängen som anropar funktionen.
> **TIP:** Värdet `$pi` behöver inte skrivas in direkt i skriptet på det här sättet utan vi kan använda funktionen `pi()`, som är mer exakt för mer komplexa beräkningar.

Särskild innebörd av "fly".
--------------------------

> **Varning:** Följande exempel fungerar endast inom citationstecken, om du omsluter dem med apostrofer kommer de att bete sig som vanliga tecken utan speciell betydelse (utom `\'` som undviker apostrofen).
Escaping är till för när jag vill skriva ut ett specialtecken inom citationstecken eller en apostrof som kan tolkas som ett språkuttryck och därför bearbetas, även om programmeraren inte hade tänkt sig det. Vi har redan visat ett exempel, men i det här avsnittet beskrivs möjliga beteendemässiga undantag.

Detta beror på att ibland har själva utbrytningen i sig en speciell betydelse. Exempel:

```php
echo "Lång text, uppdelad på två rader.";
```

Det föregående exemplet skrivs ut:

```php
Dlouhý text, rozdělený na
dva řádky.
```

Så om vi vill skriva ut ett snedstreck måste vi också undvika det (det är inte nödvändigt att undvika "n"-tecknet i det här fallet, eftersom det skulle uppfattas som ett omslag igen, eller i det här fallet måste vi inte undvika det alls):

```php
echo "Lång text, uppdelad på två rader.";
```

Det finns fler specialtecken som detta, till exempel `\t` som en tabb. Den fullständiga listan finns i den officiella dokumentationen.

Verklig användning av specialtecken vid escaping
-----------------------------------------------

Till att börja med vill jag påpeka att nästan vad som helst kan göras i PHP på flera olika sätt, och de exempel som ges här är bara för att illustrera hur problemet kan angripas.

Om vi till exempel vill analysera text rad för rad kan vi använda funktionen <a href="/explode">explode</a>.

```php
$parser = explode("\n", $retezec); // analyserar texten rad för rad
```

I det här fallet är det vettigt att använda specialtecknet `\n`, eftersom vi på ett mycket effektivt sätt kan säga att vi vill analysera efter radbrytningar.

```php
$parser = explode('
', $retezec);
```

> **Varning:** På Unix- och Windows-system förväxlas de tecken som används för att markera slutet av rader. Windows använder till exempel `CRLF` (ett par `\r\n`-tecken), medan Linux endast använder `LF` (ett enda `\n`-tecken). När du analyserar radvis bör du tänka på detta. Vanligtvis löses problemet genom att normalisera tecknen till endast `LF`.

Sammanfattning
-------

Om du kan, använd **apostrofer** överallt.

Det är bra att känna till användningen av citationstecken och bara använda dem när det är nödvändigt (eller allmänt bra). Om du skriver ut text som kan innehålla citationstecken, omsluta den med apostrofer (som då beter sig mer förutsägbart). Personligen använder jag citattecken för att uttrycka olika specialtecken som är onödigt komplicerade att ange i apostrofer och som skulle kräva komplicerad escaping.
