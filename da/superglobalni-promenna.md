Superglobale variabler
======================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	da: superglobale-variabler
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Superglobale variabler bruges til at overføre global programtilstand og HTTP-kommunikation.

Den største fordel ved disse variabler er, at de altid og overalt er tilgængelige. I praksis er de arrays af værdier, hvor vi får adgang til specifikke oplysninger ved hjælp af indeks. I forskellige sammenhænge kan tilgængeligheden af nøgler variere (forklares nedenfor).

Typer af superglobale variabler
--------------------------------

Alle superglobaler i PHP er arrays og betegnes med et dollartegn efterfulgt af en understregning (undtagen `$GLOBALS`) og store bogstaver.

I `PHP 7` er der bl.a. følgende:

| Variabel | Beskrivelse |
|-------------|-------|
| `$_GET` | URL-parametre <a href="/methods-odesilani-dat">som sendes med GET-metoden</a>
| `$_POST` | Formdata <a href="/methods-odesilani-dat">sendes via POST</a>. Bemærk, at <a href="/ajax-post">kan opføre sig anderledes i ajax</a>.
| `$_REQUEST` | Formdata sendt med en hvilken som helst metode (`$_GET`, `$_POST` og `$_REQUEST`).
| `$_FILES` | Teknisk information om de aktuelt uploadede filer, f.eks. via konstruktionen `<input type="file">`.
| `$_SERVER` | <a href="/info">Webserverindstillinger</a>, IP-adresse, konfiguration... det varierer afhængigt af miljøet (når du kalder et PHP-script fra Terminal, vil det indeholde forskellige værdier, og f.eks. vil oplysninger om den aktuelle forespørgsel mangle).
| `$_COOKIE` | Konfigureret <a href="/cookies">cookies</a>.
| `$_SESSION` | Sessionsdata (<a href="/sessions">session</a>), hvis den findes og er blevet indstillet tidligere.
| `$GLOBALS` | **Varsling, den indeholder ikke en understregning i navnet!** Dette er den såkaldte <a href="global-variabel">global-variabel</a> og en alternativ notation for nøgleordet `global`. Hvis du har en global variabel `$variable` i dit program, kan du også få adgang til den med konstruktionen `$GLOBALS["variable"]`. Men at bruge globale variabler er en dårlig og uren løsning, så det er bedre ikke at gøre det.
| `$_ENV` | Oplysninger om det aktuelle miljø, hvor PHP kører.

Det er nemt at opregne alle eksisterende værdier:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ':' . $value . '<br>';
}
```

> Bemærk: Ikke alle indeks skal altid eksistere (hvis scriptet f.eks. kører cron i CLI-tilstand, vil indekset med sidens URL-adresse eller IP-adressen for anmodningen ikke eksistere).

Adgang til variabler
-------------------

Jeg anbefaler, at alle globale variabler (undtagen `$_SESSION`) er skrivebeskyttede. Det skyldes, at de indeholder globale programdata, og at anden kode kan tage hensyn til disse data (f.eks. et andet installeret bibliotek).

En anden ulempe ved global tilstand er, at du ikke altid kan stole på nøjagtige værdier, selv om de findes, så du bør altid kontrollere deres nøgler med `isset()`-konstruktionen.

Hvis du vil gemme en ny cookie, skal du bruge `setcookie()` og ikke indsætte værdien direkte. Det skyldes, at den er skrivebeskyttet.

Erfaringer
-------

Stol aldrig blindt på værdierne af superglobale variabler!

Brugeren kan bruge URL'en og de sendte overskrifter til at påvirke, hvordan værdierne indstilles. Alle input bør altid valideres omhyggeligt.

Register globals - problemet med den gamle version af PHP
------------------------------------------

I den gamle version af PHP (op til `5.4.0`) var der et specielt `register-globals`-direktiv (konfigurerbart i `php.ini`), som fik alle overførte parametre i en URL til automatisk at blive registreret som variabler.

For eksempel:

En bruger kom til URL'en: `https://example.com/script.php?var=24`.

Og PHP oprettede automatisk en variabel `$var` med værdien `24` i scriptet.

Så det fungerede på klassisk vis:

```php
echo $var;
```

Enhver kan derfor indsætte en hvilken som helst variabel i scriptet og ændre dets indhold. Det er klart, at sikkerhed ikke altid har været en prioritet. Ikke rigtig.

Andre kilder
------------

Du kan finde en mere detaljeret beskrivelse i <a href="https://www.php.net/manual/en/language.variables.superglobals.php">officielle manual</a>.
