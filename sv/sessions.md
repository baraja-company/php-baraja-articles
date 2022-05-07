Sessioner - serverkakor i PHP
=============================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	sv: sessioner---serverkakor-i-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Ofta behöver vi lagra mer information i <a href="/cookies">cookies</a>, men den maximala gränsen för cookies är 4 kB, vilket inte är mycket. Sessions löser detta problem genom att lagra data på webbservern och endast lagra en kort identifierare identifierare i klientens webbläsare för att avgöra vilka data som tillhör vilken klient.

Starta en session
---------------------

Innan vi kan arbeta med sessioner måste vi först starta dem. Detta görs genom att anropa funktionen `session_start()` direkt i början av skriptet:

```php
session_start();
```

> Stark varning: ingen HTML-kod får exekveras innan funktionen `session_start()` anropas!

Sessionssäkerhet
-------------------

Sessionens innehåll lagras på servern och endast identifieraren skickas till klientens webbläsare, så användaren kan inte veta vad som lagras i sessionen. Det enda sätt på vilket skriptet kan påverka användaren är genom att radera identifieraren (varpå skriptet genererar en ny).

Hämta data från en session
----------------------

Alla sessioner lagras i den superglobala variabeln `$_SESSION` och kan genomgås som en array.

Namnet på den inloggade användaren kan till exempel hämtas genom att skriva:

```php
echo $_SESSION['användare'];
```

Obs: Sessionen finns kanske inte alltid (till exempel om du är en ny användare). Därför bör vi alltid kontrollera om det finns en sådan innan vi tar upp en artikel och vid behov erbjuda ett alternativt felmeddelande.

```php
if (isset($_SESSION['användare']) && $_SESSION['användare']) {
    echo 'Inloggad användare:' . $_SESSION['användare'];
} else {
    echo 'Ingen har loggat in.';
}
```

Spara data i sessionen
----------------------

Sparandet sker som en enkel lagring av data i en variabel:

```php
$_SESSION['användare'] = 'Honzik';
```

Webbservern tar hand om det tekniska tillhandahållandet av korrekt lagring på servern och sändning av identifieraren till användaren.

Radera sessioner
----------------

Enskilda värden kan raderas separat enligt nyckeln:

```php
unset($_SESSION['användare']);
```

Eller alternativt alla tillgängliga sessioner:

```php
unset($_SESSION);
```

> Observera: Om du raderar en specifik session töms inte nyckelvärdet, utan nyckeln raderas helt och hållet. Därför kommer en felvarning att skickas ut om man försöker läsa en icke-existerande nyckel. Vi kan alltid enkelt verifiera att en nyckel finns med funktionen `isset()`.

Maximal giltighet för sessionen
---------------------------------

Varje sparad session har en gräns för hur länge den sparas på servern. PHP innehåller direkt ett cronskript som regelbundet raderar gamla sessioner.

Standardvärdet är vanligtvis `1440 sekunder`, vilket är `24 minuter`.

Värdet måste ökas på två ställen:

- I <a href="/info">`php.ini` anges den maximala giltighetslängden som servern kommer att behålla</a>. Värdet fastställs av direktivet `session.gc_maxlifetime`,
- På PHP-skriptets sida måste du ange den aktuella begärda giltigheten.

Användning i PHP:

```php
// servern kommer nu att hålla sessionen i upp till 3600 sekunder = 1 timme.
ini_set('session.gc_maxlifetime', '3600');

// alla klienter (webbläsare) kommer att vara
// sessionen skickas med en giltighet på exakt 3600 sekunder
session_set_cookie_params(3600);

session_start(); // vi kan starta sessionen!
```
