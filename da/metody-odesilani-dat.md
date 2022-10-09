Dataoverførselsmetoder (GET og POST)
====================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	da: dataoverforselsmetoder-get-og-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- 'GET- og POST-metoden, hentning af data fra formular og URL. API-kommunikation og databehandling.'
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '2282538597ebfe95877ae0e005ddd352'

Ud over de almindelige variabler har vi også såkaldte **superglobale variabler** i PHP, som indeholder oplysninger om den aktuelle side og de data, som vi videregiver.

Typisk har vi en formular på en side, som brugeren udfylder, og vi ønsker at overføre disse data til webserveren, hvor vi behandler dem i PHP.

Der er 3 metoder, der oftest anvendes til dette:

- `GET` ~ dataene sendes i URL'en som parametre
- `POST` ~ dataene sendes skjult sammen med sideanmodningen
- <a href="/ajax-post">Ajax POST</a> ~ asynkron javascript-behandling

GET-metode - `$_GET`
--------------------

De data, der sendes med GET-metoden, er synlige i URL'en (som parametre efter spørgsmålstegnet), og den maksimale længde er 1024 tegn i Internet Explorer (andre browsere begrænser det *næsten* ikke, men større tekster bør ikke sendes på denne måde). Fordelen ved denne metode er især enkelheden (du kan se, hvad du sender) og muligheden for at give et link til resultatet af behandlingen. Dataene sendes til en variabel.

Adressen på den modtagende side kan se således ud:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`

I PHP kan vi så f.eks. skrive værdien af parameteren `variable` på følgende måde:

```php
echo $_GET['promenade'];	// udskriver "indhold"
```

> **Stærk advarsel:** Denne metode til at skrive data direkte til HTML-siden er ikke sikker, fordi vi f.eks. kan sende HTML-kode i URL'en, som vil blive skrevet til siden og derefter udført.
>
> Vi skal **altid** behandle dataene før output til siden, funktionen `htmlspecialchars()` bruges til dette.
>
> For eksempel: `echo htmlspecialchars($_GET['variable']);`

POST-metode - `$_POST`
----------------------

De data, der sendes med POST-metoden, er ikke synlige i URL'en, hvilket løser problemet med den maksimale længde af de sendte data. POST-metoden bør altid anvendes til at sende formularfelter, da dette sikrer, at f.eks. adgangskoder ikke er synlige, og at der ikke kan gives et link til den side, der behandler resultatet af et bestemt input.

Dataene er tilgængelige i variablen `$_POST`, og brugen er den samme som for GET-metoden.

Kontrol af, at de indsendte data findes
--------------------------------

Før vi behandler data, bør vi først kontrollere, at dataene faktisk er blevet sendt, ellers ville vi få adgang til
 til en ikke-eksisterende variabel, hvilket ville give en fejlmeddelelse.

Funktionen `isset()` bruges til at verificere eksistensen af en variabel.

```php
if (isset($_GET['Navn'])) {
    echo 'Dit navn:' . htmlspecialchars($_GET['Navn']);
} else {
    echo 'Der er ikke angivet noget navn.';
}
```

Formular til dataindtastning
------------------------

Formularen er lavet i HTML, ikke i PHP. Det kan være på en almindelig HTML-side. Alt "magi" håndteres af det PHP-script, der modtager dataene.

Vi kan f.eks. bruge en formular til at modtage 2 numre, der sendes med GET-metoden:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

I den første linje kan du se, hvor dataene vil blive sendt hen og med hvilken metode.

De næste 2 linjer er simple formularelementer, bemærk attributten **name=""**, der er navnet på den variabel, der skal indeholde det, der nu er i formularen.

Dernæst kommer knappen til at sende dataene (obligatorisk) og formularens afsluttende HTML-tag (obligatorisk, så browseren ved, hvad der ellers skal sendes og hvad der ikke skal sendes).

> Vi kan have et vilkårligt antal formularer på en side, og de kan ikke være indlejret i hinanden. Hvis der forekommer indlejring, sendes altid den mest indlejrede formular, og resten ignoreres.

Formularbehandling på serveren
-------------------------------

Nu har vi en færdig HTML-formular, og vi sender den til `script.php`, som modtager dataene ved hjælp af GET-metoden. Adressen på sideanmodningen kan se således ud:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// udskriver 8
```

Korrekt nok skal vi først kontrollere, at begge formularfelter er udfyldt, dette gøres med funktionen `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// udskriver 8
} else {
    echo 'Formularen var ikke udfyldt korrekt.';
}
```

> **TIP:** Du kan sende flere parametre til `isset()`-konstruktionen for at kontrollere, at de alle findes.
>
> Derfor kan du i stedet for `isset($_GET['x']) && isset($_GET['y'])` bare angive:
>
> `isset($_GET['x'], $_GET['y'])`.

Behandling af data, der er modtaget ved POST-metoden
--------------------------------------

Hvis data modtages via POST, ser URL'en for det script, der skal behandles, altid således ud:

`https://________.com/script.php`

Og aldrig på anden måde. Nej, bare nej. Dataene er skjult i HTTP-anmodningen, og vi kan ikke se dem.

> Den skjulte POST-metode er nødvendig for at sende brugernavne og adgangskoder af sikkerhedshensyn.
>
> **Sikkerhed:** Hvis du arbejder med adgangskoder på dit websted, skal login- og registreringsformularen være hostet på HTTPS, og du skal hashe adgangskoderne på passende vis (f.eks. med BCrypt).

Håndtering af ajax-forespørgsler
------------------------------

I nogle tilfælde er det ikke altid let at hente dataene, når der behandles Ajax-forespørgsler. Det skyldes, at Ajax-biblioteker normalt sender data som `json payload`, mens den superglobale variabel `$_POST` kun indeholder formulardata.

Dataene kan stadig tilgås, jeg beskrev detaljerne i artiklen <a href="/ajax-post">Håndtering af ajax POST-forespørgsler</a>.
