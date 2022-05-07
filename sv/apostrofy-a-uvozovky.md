Apostrofer och citationstecken
==============================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	sv: apostrofer-och-citationstecken
> 
> perex:
> 	- Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> 	- Användning av citationstecken och apostrofer för att avgränsa strängar i PHP. Strängar som undviks och kontextupplösning.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

Du kan använda antingen citationstecken eller apostrofer för att avgränsa strängar. Personligen föredrar jag endast **apostrofer** om det inte är ett särskilt radbrytningstecken eller en tabb.

Det finns ett antal skäl till detta, låt oss gå igenom dem på ett konstruktivt sätt.

> Huvudskälet till att inte använda citationstecken är säkerhet och olämplig hantering av datatyper.

Användning av HTML-taggar i en sträng
--------------------------------

Om vi behöver returnera HTML-kod i en sträng och vi omsluter strängen med citattecken måste vi använda oss av en ganska besvärlig escapingmetod:

```php
return "<a href=\"{$link}\">{$text}</a>";
```

Du kan också göra samma sak på ett mer lättläst sätt, genom att behålla citattecken utan escaping:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Längre visuellt avstånd mellan strängen och variabeln.
---------------------------------------------

Det finns inget värre än att lägga variabler i en sträng direkt ovanpå varandra, så att du inte snabbt kan se vad som är en variabel och vad som är en sträng:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Denna notation har ingen negativ effekt på funktionaliteten, bara de enskilda variablerna och de fasta tecknen i strängen staplas för nära varandra, vilket försämrar läsbarheten.

Låt oss försöka igen och göra det mer lättläst:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

Den största fördelen jag ser är att variablerna är rena och att inga onödiga tecken i namnet är i vägen.

Dessutom blir det lättare att slå in den och det kommer inte att finnas några omslagstecken i strängen ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

Det är inte möjligt att anropa en funktion inom citationstecken
---------------------------------------

Men en variabel kan göra det, vilket är anledningen till att "något" läggs utanför strängen (särskilt i funktioner), medan variabler skrivs tillbaka inuti. Och ibland lägger programmeraren till och med till variabeln efter strängen. Kort sagt, förvirring över förvirring.

Varför kan vi inte göra saker och ting enhetligt?

Olämplig mappning av en annan datatyp till en sträng.
---------------------------------------------------

Tänk på följande funktionsanrop:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... genomförande ...
}
```

Det är möjligt att sätta in variabler inom citationstecken, vilket gör att de skrivs om som en sträng. Men om variabeln finns i själva strängen och har en annan datatyp än string kan informationen om den ursprungliga datatypen skadas. Om konstruktionen `$user->name` till exempel returnerar `false` eller `null` kan vi inte se att det är ett fel.

Ett program bör alltid känna igen ett feltillstånd och misslyckas på rätt sätt, i stället för att ignorera det. Korrekt felrapportering leder till bättre felsökning i framtiden.

Ibland kan du stöta på överordningar, särskilt om en funktion kräver en viss datatyp:

```php
trim("{$returnText}");
```

I så fall är jag mer benägen att skriva ner det:

```php
trim ((string) $returnText);
```

vilket inte är så "magiskt" och det är uppenbart även för mindre erfarna användare vad som hände med variabeln.

Slopa värdet `null` från databasen
----------------------------------

Anta att vi hämtar namnet på ett hotell från en databas:

```php
return "{$row->hotel->name}";
```

Men vad händer om namnet inte finns och är `null`? Genom att använda citationstecken har vi skapat ett fel som kan vara svårt att upptäcka, eftersom det kommer att skrivas om till en tom sträng. Vi skulle dock vilja returnera en riktig `null`. Det gör skillnad om posten inte finns eller om den finns men är tom.

Det korrekta sättet att göra detta är alltså att inte ange något:

```php
return $row->hotel->name;
```

Kräver funktionen att en viss datatyp ska returneras och är den sträng när det gäller detta? Om vi inte kan uppfylla specifikationen ska vi kasta ett undantag.

```php
function getHotelName(int $hotelId): string
{
   // genomförande

   if ($row->hotel->name === null) {
      throw new \Exception('Hotellnamnet finns inte.');
   }

   return $row->hotel->name;
}
```

Inkonsekvenser i tecken- och datatypkodning
--------------------------------------------

Det är inte ovanligt att man stöter på konstruktioner i stil med:

```php
echo "{$returnCode}" . self::CRLF;
```

Det kan bättre skrivas som:

```php
echo $returnCode . "\n";
```

Användningen av citationstecken runt variabeln är onödig i det här fallet och gör det bara svårare att läsa eftersom det är extra onödiga tecken. Samtidigt är radbrytning av typen `CRLF` inte modern och det är bättre att använda endast `\n`, som är en del av `UTF-8`-kodningen.

På rätt sätt kan vi fortfarande validera datatypen för koden. Till exempel att det är ett nummer och eventuellt returnera en ersättning. Detta kan göras med <a href="/ternary-operator">ternary operator</a>:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Obs: Att använda funktionen `is_int()` tyder på dålig programdesign, eftersom det aldrig får hända att vi inte vet vilken datatyp en variabel har.

Inlindning av utdatasträngen i apostrofer
---------------------------------------

Det är ofta nödvändigt att omsluta utdatasträngen från en variabel med citationstecken eller apostrofer i undantagstexten. Detta skapar dock i onödan "payoff" för båda typerna av strängavgränsare, och om strängen börjar och slutar med samma tecken är det inte möjligt att snabbt och otvetydigt avgöra var strängen började och slutade om det finns flera strängar med en apostrof inuti:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Själv löser jag detta genom att omsluta den i en annan teckentyp (fyrkantiga parenteser fungerar bra för mig), så att strängens början och slut kan ses på ett elegant sätt:

```php
throw new \Exception(__METHOD__ . ': Datatyp som inte stöds [' . $fileType . ']');
```

Eller:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Kan bytas ut mot:

```php
throw new \Exception('Status [' . $status . '] är ogiltig.');
```

Analysering av rader
--------------------

Anföringstecken är till exempel användbara för att analysera en ny rad:

```php
foreach(explode("\n", $text) as $line) {
	// genomförande
}
```

De har skapats särskilt för detta ändamål.

Om du behöver bearbeta ett mer komplext dokument (t.ex. källkod till ett programmeringsspråk eller en HTML-sida) rekommenderar jag att du använder **Tokenizer** tillsammans med <a href="/regex">regelbundna uttryck</a>.

Kombinera inte två metoder för att innesluta
-----------------------------------

Om du av någon anledning ändå ska använda citat, använd åtminstone konsekventa citat genomgående.

Detta är ett avskräckande fall:

```php
throw new \Exception("Bilden på URL-adressen finns inte. Svarsstorlek:" . strlen($result) . ')');
```

Där början av strängen avgränsas av citattecken och slutet av apostrofer.

Detta fel uppstår särskilt när man använder automatiska formateringsverktyg (som **Code Sniffer**) som försöker förenhetliga konventioner, men som inte alltid lyckas.
