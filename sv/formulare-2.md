Formulär, formulärbehandling i PHP
==================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	sv: formulaer-formulaerbehandling-i-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

Jag antar att vi har skapat ett HTML-formulär som vi skickar och nu vill vi behandla uppgifterna. Det finns en <a href="/formulare">separat artikel</a> om hur man skapar ett HTML-formulär.

Mottagande av data - olika sätt
----------------------------

Sättet som formuläret skickas på fastställs direkt i HTML

Det finns 2 alternativ:

- **GET** - Det syns i adressfältet efter frågetecknet.
 Till exempel: `php.baraja.cz/search.php?query=formulare`.
- **POST** - Dold (inte synlig), de flesta formulär skickas via post.

Vi måste sedan läsa dem i PHP med samma metod.

Hämta data från användaren och överföra dem till skriptet
------------------------------------------------------

Grunden är ett HTML-formulär, hur man gör det kan du läsa <a href="/formulare">i en separat artikel</a>.

Låt oss till att börja med utgå från ett enkelt formulär för att skriva in användarens namn:

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

En textruta visas där du kan ange ett namn och klicka på skicka. När du klickar på knappen skickas fältets innehåll till skriptet `welcome.php`.

Nu till själva bearbetningen i filen `welcome.php`:

```php
$username = $_GET['användarnamn'];

echo 'Det angivna namnet är:' . $username;
```

Observera den speciella variabeln `$_GET`. Detta är en **superglobal variabel** som innehåller data från formuläret och kan nås som en array.

Problemet med denna lösning är dock att de mottagna uppgifterna inte är **säkra** och att ett liknande formulär lätt kan attackeras. En potentiell angripare kan t.ex. skriva in javascriptkod i fältet i stället för ett namn, som skrivs till sidan och körs.

Därför måste vi alltid rensa alla användardata innan vi lägger ut dem i HTML-koden:

```php
$username = $_GET['användarnamn'] ?? 'Okänd';

echo 'Det angivna namnet är:' . htmlspecialchars($username);
```

Vidareförädling
----------------

Vi kan göra vad som helst med de mottagna uppgifterna och behandla dem som vanliga variabler.

Lägg till exempel till värdet i två fält:

```php
echo $_GET['x'] + $_GET['y'];
```

Eller spara till en fil, databas, e-post, ...

Följande funktioner är användbara för detta:

- <a href="/file-put-contents">file_put_contents</a> - funktion för att spara data i en fil
- <a href="/hashovani">MD5</a> - beräkning av kontrollsumma, till exempel för lösenord.
- <a href="/cookies">Cookies</a> - sparar data i **cookies** (små filer i webbläsaren).
