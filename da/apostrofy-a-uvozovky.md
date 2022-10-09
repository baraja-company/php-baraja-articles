Apostroffer og anførselstegn
============================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	da: apostroffer-og-anforselstegn
> 
> perex:
> 	- Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> 	- Brug af anførselstegn og apostroffer til afgrænsning af strenge i PHP. String escaping og kontekstopløsning.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

Du kan bruge enten anførselstegn eller apostroffer til at afgrænse strenge. Personligt foretrækker jeg kun **apostroffer**, medmindre der er tale om et særligt linjeskift eller en tabulator.

Der er en række grunde til dette, og lad os gennemgå dem konstruktivt.

> Hovedårsagen til ikke at bruge anførselstegn er sikkerhed og uhensigtsmæssig håndtering af datatyper.

Brug af HTML-tags i en streng
--------------------------------

Hvis vi skal returnere HTML-kode i en streng, og vi pakker strengen ind i anførselstegn, er vi nødt til at undvige ret besværligt:

```php
return "<a href=\"{$link}\">{$text}</a>";
```

Du kan også gøre det samme på en mere læsbar måde, hvor du beholder anførselstegnene uden at undslippe dem:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Større visuel afstand mellem strengen og variablen
---------------------------------------------

Der er ikke noget værre end at sætte variabler i en streng direkte oven på hinanden, hvor man ikke hurtigt kan se, hvad der er en variabel og hvad der er en streng:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Denne notation har ingen negativ indvirkning på funktionaliteten, men de enkelte variabler og faste tegn fra strengen er stablet for tæt sammen, hvilket forringer læsbarheden.

Lad os prøve igen og gøre det mere læsbart:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

Den største fordel, jeg ser, er den renhed omkring variablerne, hvor ingen unødvendige tegn i navnet er i vejen.

Desuden vil det være lettere at ombryde, og der vil ikke være nogen ombrydende tegn i strengen ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

Det er ikke muligt at kalde en funktion inden for anførselstegn
---------------------------------------

Men det kan en variabel, hvilket er grunden til, at "noget" tilføjes uden for strengen (især funktioner), men at variabler skrives tilbage indeni. Og nogle gange tilføjer programmøren endda variablen efter strengen. Kort sagt, forvirring over forvirring.

Hvorfor kan vi ikke gøre tingene ensartet?

Uhensigtsmæssig afbildning af en anden datatype til en streng
---------------------------------------------------

Overvej følgende funktionsopkald:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... implementering ...
}
```

Det er muligt at indsætte variabler i anførselstegn, hvilket medfører, at de bliver omskrevet som en streng. Hvis variablen imidlertid er i selve strengen og har en anden datatype end string, kan oplysningerne om den oprindelige datatype blive ødelagt. Hvis konstruktionen `$user->name` f.eks. returnerede `false` eller `null`, ville vi ikke kunne se, at der var tale om en fejl.

Et program bør altid genkende en fejltilstand og fejle korrekt i stedet for at ignorere den. Korrekt fejlrapportering fører til bedre fejlfinding i fremtiden.

Det kan ske, at du støder på overstyringer, især fordi en funktion kræver en bestemt datatype:

```php
trim("{$returnText}");
```

I så fald er jeg mere tilbøjelig til at skrive det ned:

```php
trim ((string) $returnText);
```

hvilket ikke er så "magisk", og det er tydeligt selv for mindre erfarne brugere, hvad der er sket med variablen.

Fjernelse af værdien `null` fra databasen
----------------------------------

Lad os antage, at vi henter navnet på et hotel fra en database:

```php
return "{$row->hotel->name}";
```

Men hvad hvis navnet ikke findes og er `null`? Ved at bruge anførselstegn har vi skabt en fejl, der kan være svær at opdage, fordi den vil blive omskrevet til en tom streng. Vi vil dog gerne returnere en rigtig `null`. Det gør en forskel, om posten ikke findes, eller om den findes, men er tom.

Så den korrekte måde at gøre dette på ville være at angive ingenting:

```php
return $row->hotel->name;
```

Kræver funktionen, at der returneres en bestemt datatype, og er den streng med hensyn til dette? Hvis vi ikke kan opfylde specifikationen, skal vi smide en undtagelse.

```php
function getHotelName(int $hotelId): string
{
   // implementering

   if ($row->hotel->name === null) {
      throw new \Exception('Hotellets navn findes ikke.');
   }

   return $row->hotel->name;
}
```

Uoverensstemmelser i kodning af tegn og datatyper
--------------------------------------------

Det er ikke ualmindeligt at støde på konstruktioner i stil med:

```php
echo "{$returnCode}" . self::CRLF;
```

Hvilket bedre kan skrives som:

```php
echo $returnCode . "\n";
```

Brugen af anførselstegn omkring variablen er unødvendig i dette tilfælde og gør det bare sværere at læse, fordi det er ekstra unødvendige tegn. Samtidig er linjeombrydning af typen `CRLF` ikke moderne, og det er bedre kun at bruge `\n`, som er indbygget i `UTF-8`-kodningen.

Vi kan stadig validere datatypen for koden på korrekt vis. F.eks. at det er et tal, og eventuelt returnere en erstatning. Dette kan gøres med <a href="/ternary-operator">ternary-operator</a>:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Bemærk: Brug af funktionen `is_int()` indikerer dårligt programdesign, fordi det aldrig bør ske, at vi ikke kender datatypen for en variabel.

Indpakning af outputstrengen i apostroffer
---------------------------------------

Det er ofte nødvendigt at omslutte outputstrengen fra en variabel med anførselstegn eller apostroffer i undtagelsesteksten. Dette skaber dog unødvendigt "payoff" for begge typer af strengafgrænsere, og hvis strengen starter og slutter med det samme tegn, er det ikke muligt hurtigt at afgøre entydigt, hvor strengen startede og sluttede i tilfælde af flere strenge, hvis der er en apostrof inde i strengen:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Personligt løser jeg problemet ved at omslutte den i en anden tegntype (firkantede parenteser fungerer godt for mig), så begyndelsen og slutningen af strengen kan ses på en elegant måde:

```php
throw new \Exception(__METHOD__ . ': Datatype, der ikke understøttes [' . $fileType . ']');
```

Eller:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Kan ombyttes til:

```php
throw new \Exception('Status [' . $status . '] er ugyldig');
```

Parsing efter linjer
--------------------

Anførselstegn er f.eks. nyttige til at analysere en ny linje:

```php
foreach(explode("\n", $text) as $line) {
	// implementering
}
```

De blev specielt skabt til dette formål.

Men hvis du skal behandle et mere komplekst dokument (f.eks. kildekode til et programmeringssprog eller en HTML-side), anbefaler jeg at bruge **Tokenizer** sammen med <a href="/regex">regulære udtryk</a>.

Du må ikke kombinere to metoder til indkapsling af
-----------------------------------

Hvis du af en eller anden grund alligevel skal bruge citater, så hold dem i det mindste konsekvente hele vejen igennem.

Der er tale om en afskrækkende sag:

```php
throw new \Exception("Billedet på URL findes ikke. ResponseSize:" . strlen($result) . ')');
```

Hvor begyndelsen af strengen er afgrænset af anførselstegn og slutningen af apostroffer.

Denne fejl opstår især ved brug af automatiske formateringsværktøjer (såsom **Code Sniffer**), som forsøger at ensrette konventioner, men som ikke altid lykkes.
