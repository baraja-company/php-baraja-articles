Formularer, formularbehandling i PHP
====================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	da: formularer-formularbehandling-i-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

Jeg antager, at vi har oprettet en HTML-formular, som vi sender, og nu ønsker vi at behandle dataene. Der findes en <a href="/formulare">separat artikel</a> om oprettelse af en HTML-formular.

Modtagelse af data - forskellige måder
----------------------------

Måden, hvorpå formularen sendes, angives direkte i HTML

Der er 2 muligheder:

- **GET** - Det er synligt i adresselinjen efter spørgsmålstegnet
 For eksempel: `php.baraja.cz/search.php?query=formulare`
- **POST** - Skjult (ikke synlig), de fleste formularer sendes med posten

Vi skal derefter læse dem i PHP ved hjælp af samme metode.

Få dataene fra brugeren og overføre dem til scriptet
------------------------------------------------------

Grundlaget er en HTML-formular, og hvordan du laver den, kan du læse <a href="/formulare">i en separat artikel</a>.

Lad os starte med en simpel formular til indtastning af brugerens navn:

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

Der vises et tekstfelt, hvor du skal indtaste et navn og klikke på Send. Når der klikkes på knappen, sendes indholdet af feltet til scriptet `welcome.php`.

Nu til den egentlige behandling i filen `welcome.php`:

```php
$username = $_GET['brugernavn'];

echo 'Det indtastede navn er:' . $username;
```

Bemærk den særlige variabel `$_GET`. Dette er en **superglobal variabel**, der indeholder data fra formularen og kan tilgås som et array.

Problemet med denne løsning er imidlertid, at de modtagne data **ikke er sikre**, og at en lignende formular let kan angribes. En potentiel angriber kan f.eks. indtaste javascript-kode i feltet i stedet for et navn, som vil blive skrevet til siden og udført.

Derfor skal vi altid rense alle brugerdata, før vi udsender dem i HTML-kode:

```php
$username = $_GET['brugernavn'] ?? 'Ukendt';

echo 'Det indtastede navn er:' . htmlspecialchars($username);
```

Yderligere behandling
----------------

Vi kan gøre hvad som helst med de modtagne data og behandle dem som alle andre almindelige variabler.

Tilføj f.eks. værdien i to felter:

```php
echo $_GET['x'] + $_GET['y'];
```

Du kan også gemme til en fil, database, e-mail, ...

Følgende funktioner er nyttige til dette formål:

- <a href="/file-put-contents">file_put_contents</a> - funktion til at gemme data til en fil
- <a href="/hashovani">MD5</a> - beregning af checksummen, f.eks. for adgangskoder
- <a href="/cookies">Cookies</a> - gemmer data i **cookies** (små filer i webbrowseren)
