Superglobala variabler
======================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	sv: superglobala-variabler
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Superglobala variabler används för att överföra globalt programtillstånd och HTTP-kommunikation.

Den största fördelen med dessa variabler är att de alltid och överallt är tillgängliga. I praktiken är de arrayer med värden där vi får tillgång till specifik information genom index. I olika sammanhang kan tillgången till nycklar variera (förklaras nedan).

Typer av superglobala variabler
--------------------------------

Alla superglobaler i PHP är matriser och betecknas med ett dollartecken följt av ett understreck (utom `$GLOBALS`) och versaler.

I `PHP 7` finns bland annat följande:

| Variabel | Beskrivning |
|-------------|-------|
| `$_GET` | URL-parametrar <a href="/methods-odesilani-dat">som skickas med GET-metoden</a>
| `$_POST` | Formulärdata <a href="/methods-odesilani-dat">som skickas via POST</a>. Observera att <a href="/ajax-post">kan bete sig annorlunda i ajax</a>.
| `$_REQUEST` | Formulärdata som skickas med någon metod (`$_GET`, `$_POST` och `$_REQUEST`).
| `$_FILES` | Teknisk information om de aktuella uppladdade filerna, till exempel via konstruktionen `<input type="file">`.
| `$_SERVER` | <a href="/info">webbserverinställningar</a>, IP-adress, konfiguration... det varierar beroende på miljön (när du anropar ett PHP-skript från Terminal kommer det att innehålla olika värden och t.ex. saknas information om den aktuella begäran).
| `$_COOKIE` | Konfigurerad <a href="/cookies">cookies</a>.
| `$_SESSION` | Sessionsdata (<a href="/sessions">session</a>), om den finns och har ställts in tidigare.
| `$GLOBALS` | **Varning, det finns inget understreck i namnet!** Detta är den så kallade <a href="global-variabel">global-variabeln</a> och en alternativ notation för nyckelordet `global`. Om du har en global variabel `$variable` i ditt program kan du också komma åt den med konstruktionen `$GLOBALS["variable"]`. Att använda globala variabler är dock en dålig och oren lösning, så det är bäst att du inte gör det.
| `$_ENV` | Information om den aktuella miljön där PHP körs.

Det är lätt att lista alla befintliga värden:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ':' . $value . '<br>';
}
```

> Observera: Alla index måste inte alltid finnas (om skriptet till exempel kör cron i CLI-läge kommer indexet med sidans URL eller IP-adressen för begäran inte att finnas).

Tillgång till variabler
-------------------

Jag rekommenderar att alla globala variabler (utom `$_SESSION`) är skrivskyddade. Detta beror på att de innehåller globala programdata och att annan kod kan ta hänsyn till detta (t.ex. ett annat installerat bibliotek).

En annan nackdel med global state är att du inte alltid kan lita på exakta värden, även om de finns, så du bör alltid kontrollera deras nycklar med konstruktionen `isset()`.

Om du vill spara en ny cookie använder du `setcookie()` och lägger inte in värdet direkt. Detta beror på att den är skrivskyddad.

Lärdomar som dragits
-------

Lita aldrig blint på värdena för superglobala variabler!

Användaren kan använda URL:en och de rubriker som skickas för att påverka hur värdena sätts. All inmatning bör alltid valideras noggrant.

Registrera globals - problemet med den gamla versionen av PHP
------------------------------------------

I den gamla versionen av PHP (fram till `5.4.0`) fanns det ett speciellt `register-globals`-direktiv (konfigurerbart i `php.ini`) som gjorde att alla passerade parametrar i en URL automatiskt registrerades som variabler.

Till exempel:

En användare kom till URL:n: `https://example.com/script.php?var=24`.

PHP skapade automatiskt en variabel `$var` med värdet `24` i skriptet.

Det fungerade alltså på ett klassiskt sätt:

```php
echo $var;
```

Vem som helst kan därför lägga in vilken variabel som helst i skriptet och ändra dess innehåll. Det är uppenbart att säkerheten inte alltid har varit en prioritet. Inte riktigt.

Andra källor
------------

En mer detaljerad beskrivning finns i <a href="https://www.php.net/manual/en/language.variables.superglobals.php">officiell handbok</a>.
