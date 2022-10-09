Principper for skrivning af variabler
=====================================

> id: '4c8e631b-b305-4169-8885-4f9506155999'
> slug:
> 	cs: zasady-promennych
> 	da: principper-for-skrivning-af-variabler
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '5528d9b73c1d1c07c330a58e4aeaa06a'

Dette er anden del af en tutorial-serie om PHP. I denne episode vil vi se på de grundlæggende regler for at skrive variabler.

Denne side er blot et hurtigt overblik. Hvis du leder efter en detaljeret teknisk beskrivelse af alle funktionerne, har jeg skrevet en <a href="/variabel">separat artikel</a>.

Grundlæggende syntaks
--------------------------

Variabler i PHP starter med dollartegnet `$`, umiddelbart efterfulgt af navnet.

```php
$zvire = 'cat';
```

Strings (sekvenser af tegn) er omsluttet af anførselstegn eller apostroffer:

```php
$a = "Anførselstegn";
$b = 'apostroffer';
```

Cifre er ikke omgivet af anførselstegn:

```php
$a = 5;
$b = 10;
$c = 3.14159;
```

Variabelnavnet kan kun bestå af tegn i det engelske alfabet og tal. Navnet begynder altid med et bogstav.

Hvis navnet består af mere end ét ord, er det almindeligt at bruge syntaksen `camelCase` (første bogstav med små bogstaver og hvert andet ord starter med et stort bogstav):

```php
$kocka = 'Kitty';
$rychlyPocitac = 'Selvfølgelig er det min!';
$pocetRohuJednorozce = 1;
```

Navnet må ikke indeholde mellemrum, bindestreger, kroge, kommaer, anførselstegn, parenteser eller andre specialtegn. Det eneste tilladte specialtegn er `understreg`.

Decimaltal skal skrives med punktum:

```php
$pi = 3.14159;
```

Det kan ofte være nyttigt at udføre matematiske operationer direkte, når du definerer en variabel:

```php
$a = 5;
$b = 3;
$c = $a + $b;	// tilføj 5 + 3
echo $c;		// udskriver 8
```

Korrekt indsættelse af et anførselstegn eller en apostrof
--------------------------

Anførselstegn og apostroffer må ikke kombineres vilkårligt. Hvis vi f.eks. beslutter os for at bruge et anførselstegn, skal vi også afslutte strengen med et anførselstegn og ikke bruge det indeni.

Det er derfor forkert:

```php
echo "<img src="obrazek.gif">";
```

Fordi det ikke er klart, hvor kæden begynder og slutter. Anførselstegn og apostroffer kan ikke være indlejret i hinanden.

En mulig løsning kaldes <a href="/escapovani">escaping</a>, hvor det problematiske tegn indledes med en skråstreg.

```php
echo "<img src="image.gif">";
```

Skråstreg angiver, at det næste tegn vil være præcis det tegn, som vi ønsker at bruge.

Når der skal udskrives HTML-kode, er det dog bedre at omslutte hele strengen med apostroffer og derefter bruge anførselstegn på normal vis:

```php
echo '<img src="image.gif">';
```

Alternativt kan den også vendes:

```php
echo "<img src='picture.gif'>";
```

Udfyldning af en variabel fra en url-adresse eller fra en formular
--------------------------

Adresser, der indeholder et spørgsmålstegn, indeholder oplysninger om inputvariablerne, så f.eks. `index.php?page=contacts` angiver variablen `page` med værdien `contacts`. Værdien af denne variabel læses som `$_GET['page']`.

Spørgsmålstegnet er på ingen måde relateret til navnet på filen på disken. Det er altid den samme fil, som vi sender parametre til i adressen.

Jeg diskuterer dette spørgsmål i detaljer i min artikel om <a href="/methods-odesilani-dat">metoder til at sende data</a>.

Definition af indholdet af en variabel ud fra en adresse
--------------------------

Nogle variabler er tilgængelige på det tidspunkt, hvor scriptet køres (og kan derfor bruges med det samme), disse kaldes **superglobale variabler**. Hvis vi f.eks. ønsker at læse en værdi fra en URL-adresse, bruger vi variablen `$_GET`.
Brugen er som følger:

```php
$a = $_GET['a'];

echo $a;
```

Dette script udskriver det, der står i URL'en efter spørgsmålstegnet, i kildekoden.

> Advarsel, denne prøve er ikke sikker! Hvis en uærlig besøgende f.eks. indsender HTML-kode i URL-adressen, vil den blive indsat på siden og udført. Derfor skal vi altid behandle output; funktionen `htmlspecialchars()` bruges til dette.

```php
$a = $_GET['a'];

echo htmlspecialchars($a);
```

> Hvis vi tilgår siden uden at angive parameteren `?a=anything`, vil variablen `$_GET['a']` ikke eksistere, og PHP vil sende en fejlmeddelelse. Vi skal behandle denne betingelse med en betingelse og gøre ingenting, hvis variablen ikke findes (eller alternativt output alternativt indhold). Variablens eksistens kan verificeres med funktionen `isset()`.

```php
if (isset($_GET['a'])) {
    $a = $_GET['a'];

    echo htmlspecialchars($a);
} else {
    echo 'Variablen "a" findes ikke!';
}
```

Eksempel med optælling
--------------------------

Med variablerne fra URL-adressen kan vi f.eks. tilføje dem sammen og skrive resultatet direkte:

```php
echo $_GET['a'] + $_GET['b'];
```

Hvis vi ønsker at inkludere flere inputparametre i URL'en, skal vi adskille dem med et ampersand (`&`). Adressen kan se således ud: `index.php?a=5&b=3`.

Sammenkædning af tekstinput (strenge)
--------------------------

Vi kan også nemt linke 2 tekstinput (strenge). Dette gøres ved hjælp af punktoperatoren. Du kan oprette et link i en variabel eller i en liste.

```php
$a = 'hund';
$b = 'cat';

echo $a . 'a' . $b;
```

Der står "hund og kat".
