Principer för skrivande variabler
=================================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	sv: principer-foer-skrivande-variabler
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

Detta är den andra delen av en handledningsserie om PHP. I det här avsnittet tittar vi på de grundläggande reglerna för att skriva variabler.

Den här sidan är bara en snabb översikt. Om du vill ha en detaljerad teknisk beskrivning av alla funktioner har jag skrivit en <a href="/variabel">separat artikel</a>.

Grundläggande syntax
--------------------------

Variabler i PHP börjar med dollartecknet `$` följt av namnet.

```php
$zvire = 'cat';
```

Strängar (sekvenser av tecken) omges av citattecken eller apostrofer:

```php
$a = "Angivelsemärken";
$b = 'apostrofer';
```

Siffror får inte stå inom citationstecken:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Variabelns namn kan endast bestå av tecken i det engelska alfabetet och siffror. Namnet börjar alltid med en bokstav.

Om namnet består av mer än ett ord är det vanligt att använda syntaxen `camelCase` (första bokstaven är liten och alla andra ord börjar med en stor bokstav):

```php
$kocka = 'Kitty';
$rychlyPocitac = 'Självklart är det min!';
$pocetRohuJednorozce = 1;
```

Namnet får inte innehålla mellanslag, streck, krokar, kommatecken, citationstecken, parenteser eller andra specialtecken. Det enda specialtecken som tillåts är `underline`.

Decimaltal ska skrivas med en punkt:

```php
$pi = 3.14159;
```

Det kan ofta vara användbart att utföra matematiska operationer direkt när du definierar en variabel:

```php
$a = 5;
$b = 3;
$c = $a + $b;	// lägg till 5 + 3
echo $c;		// skriver ut 8
```

Korrekt införande av ett citationstecken eller en apostrof.
--------------------------

Angivelser och apostrofer får inte kombineras på ett godtyckligt sätt. Om vi till exempel bestämmer oss för att använda ett citationstecken måste vi också avsluta strängen med citationstecken och inte använda det inuti.

Detta är därför fel:

```php
echo "<img src="obrazek.gif">";
```

Eftersom det inte är tydligt var kedjan börjar och slutar. Angivelser och apostrofer kan inte placeras i varandra.

En möjlig lösning kallas <a href="/escapovani">escaping</a>, där det problematiska tecknet föregås av ett backslash.

```php
echo "<img src="image.gif">";
```

Backslash anger att nästa tecken kommer att vara exakt det som vi vill använda.

När det gäller HTML-kod är det dock bättre att omsluta hela strängen med apostrofer och sedan använda citattecken på vanligt sätt:

```php
echo '<img src="image.gif">';
```

Alternativt kan den också vändas om:

```php
echo "<img src='picture.gif'>";
```

Fyllning av en variabel från en url-adress eller från ett formulär
--------------------------

Adresser som innehåller ett frågetecken innehåller information om inmatningsvariablerna, så till exempel `index.php?page=contacts` anger variabeln `page` med värdet `contacts`. Värdet av denna variabel läses som `$_GET['page']`.

Frågetecknet är inte på något sätt relaterat till namnet på filen på disken. Det är alltid samma fil som vi skickar parametrar till i adressen.

Jag diskuterar den här frågan i detalj i min artikel om <a href="/methods-odesilani-dat">metoder för att skicka data</a>.

Definiera innehållet i en variabel från en adress
--------------------------

Vissa variabler är tillgängliga när skriptet körs (och kan därför användas direkt), dessa kallas **superglobala variabler**. Om vi till exempel vill läsa ett värde från en URL använder vi variabeln `$_GET`.
Användningen är följande:

```php
$a = $_GET['a'];

echo $a;
```

Det här skriptet skriver ut det som står i URL:en efter frågetecknet i källkoden.

> Varning, detta prov är inte säkert! Om en oseriös besökare skulle skicka in t.ex. HTML-kod i URL:en skulle den infogas på sidan och köras. Därför måste vi alltid behandla utdata; funktionen `htmlspecialchars()` används för detta.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```

> Om vi öppnar sidan utan att ange parametern `?a=anything` kommer variabeln `$_GET['a']` inte att existera och PHP kommer att skicka ett felmeddelande. Vi måste behandla detta villkor som ett villkor och göra ingenting om variabeln inte finns (eller alternativt ge ut alternativt innehåll). Variabelns existens kan verifieras med funktionen `isset()`.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'Variabeln "a" finns inte!';
}
```

Exempel med räkning
--------------------------

Med variablerna från URL-adressen kan vi till exempel lägga ihop dem och skriva resultatet direkt:

```php
echo $_GET['a'] + $_GET['b'];
```

Om vi vill inkludera fler inmatningsparametrar i URL:en måste vi separera dem med ett ampersand (`&`). Adressen kan se ut så här: `index.php?a=5&b=3`.

Länkning av textinmatningar (strängar)
--------------------------

Vi kan också enkelt koppla ihop två textinmatningar (strängar). Detta görs med hjälp av punktoperatorn. Du kan länka i en variabel eller när du listar.

```php
$a = 'hund';
$b = 'cat';

echo $a . 'a' . $b;
```

Det står "hund och katt".
