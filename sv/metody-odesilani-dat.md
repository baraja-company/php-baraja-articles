Metoder för dataöverföring (GET och POST)
=========================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	sv: metoder-foer-dataoeverfoering-get-och-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- 'GET- och POST-metoden, hämta data från formulär och URL. API-kommunikation och databehandling.'
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '81b5f92d7ee05563b6ece295ed5958d3'

Förutom de vanliga variablerna har vi också så kallade **superglobala variabler** i PHP, som innehåller information om den aktuella sidan och de data vi skickar.

Vanligtvis har vi ett formulär på en sida som användaren fyller i och vi vill överföra dessa data till webbservern där vi bearbetar dem i PHP.

Det finns tre metoder som oftast används för att göra detta:

- `GET` ~ data skickas i URL:en som parametrar
- `POST` ~ data skickas i hemlighet tillsammans med sidans begäran.
- <a href="/ajax-post">Ajax POST</a> ~ asynkron javascriptbehandling

GET-metoden - `$_GET`.
--------------------

Data som skickas med GET-metoden är synliga i URL:en (som parametrar efter frågetecknet), den maximala längden är 1024 tecken i Internet Explorer (andra webbläsare har *nästan* inga begränsningar, men större texter bör inte skickas på detta sätt). Fördelen med denna metod är framför allt enkelheten (du kan se vad du skickar) och möjligheten att tillhandahålla en länk till resultatet av behandlingen. Uppgifterna skickas till en variabel.

Adressen till den mottagande sidan kan se ut så här:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`.

I PHP kan vi till exempel skriva värdet för parametern `variable` på följande sätt:

```php
echo $_GET['Promenad'];	// skriver ut "innehåll"
```

> **Stark varning:** Den här metoden att skriva data direkt till HTML-sidan är inte säker, eftersom vi till exempel kan skicka HTML-kod i URL:en som skrivs till sidan och sedan utförs.
>
> Vi måste **alltid** behandla data innan vi skickar ut dem till sidan, funktionen `htmlspecialchars()` används för detta.
>
> Till exempel: `echo htmlspecialchars($_GET['variable']);`

POST-metoden - `$_POST`.
----------------------

De data som skickas med POST-metoden syns inte i URL:en, vilket löser problemet med den maximala längden på de data som skickas. POST-metoden bör alltid användas för att skicka formulärfält, eftersom detta säkerställer att t.ex. lösenord inte är synliga och att en länk inte kan ges till den sida som behandlar resultatet av en viss inmatning.

Uppgifterna finns i variabeln `$_POST` och användningen är densamma som för GET-metoden.

Kontroll av att de skickade uppgifterna finns.
--------------------------------

Innan vi behandlar uppgifter bör vi först kontrollera att uppgifterna faktiskt har skickats, annars skulle vi få tillgång till
 till en icke-existerande variabel, vilket skulle ge ett felmeddelande.

Funktionen `isset()` används för att kontrollera att en variabel finns.

```php
if (isset($_GET['Namn'])) {
    echo 'Ditt namn:' . htmlspecialchars($_GET['Namn']);
} else {
    echo 'Inget namn har angetts.';
}
```

Formulär för inmatning av uppgifter
------------------------

Formuläret är gjort i HTML, inte i PHP. Det kan vara på en vanlig HTML-sida. All "magi" sköts av det PHP-skript som tar emot uppgifterna.

Som exempel kan vi använda ett formulär för att ta emot två nummer som skickas med GET-metoden:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

I den första raden kan du se var data kommer att skickas och med vilken metod.

De två följande raderna är enkla formulärelement, notera attributet **name=""**, det är namnet på den variabel som kommer att innehålla det som nu finns i formuläret.

Därefter kommer knappen för att skicka uppgifterna (obligatorisk) och formulärets avslutande HTML-tagg (obligatorisk för att webbläsaren ska veta vad som ska skickas och vad som inte ska skickas).

> Vi kan ha hur många formulär som helst på en sida, men de kan inte vara inbäddade i varandra. Om det förekommer häckningar skickas alltid det mest häckade formuläret och resten ignoreras.

Behandling av formulär på servern
-------------------------------

Nu har vi ett färdigt HTML-formulär och skickar det till `script.php`, som tar emot data med hjälp av GET-metoden. Adressen till sidbegäran kan se ut så här:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// skriver ut 8
```

Det är korrekt att först kontrollera att båda formulärfälten har fyllts i. Detta görs med funktionen `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// skriver ut 8
} else {
    echo 'Formuläret har inte fyllts i korrekt.';
}
```

> **TIP:** Du kan skicka flera parametrar till konstruktionen `isset()` för att kontrollera att alla finns.
>
> Därför kan du istället för `isset($_GET['x']) && isset($_GET['y'])` bara ange:
>
> `isset($_GET['x'], $_GET['y'])`.

Behandling av data som tas emot med POST-metoden
--------------------------------------

Om data tas emot med POST-metoden kommer URL:en för det skript som ska bearbetas alltid att se ut så här:

`https://________.com/script.php`.

Och aldrig på annat sätt. Nej, helt enkelt inte. Uppgifterna är dolda i HTTP-förfrågan och vi kan inte se dem.

> Den dolda POST-metoden krävs av säkerhetsskäl för att skicka användarnamn och lösenord.
>
> **Säkerhet:** Om du arbetar med lösenord på din webbplats bör inloggnings- och registreringsformuläret vara hyllat på HTTPS och du måste hasha lösenordet på lämpligt sätt (t.ex. med BCrypt).

Hantering av ajax-förfrågningar
------------------------------

I vissa fall kan det vara svårt att hämta data när Ajax-begäranden behandlas. Anledningen är att ajax-bibliotek vanligtvis skickar data som `json payload`, medan den superglobala variabeln `$_POST` endast innehåller formulärdata.

Det går fortfarande att få tillgång till data, jag beskrev detaljerna i artikeln <a href="/ajax-post">Bearbetning av ajax POST-förfrågningar</a>.

Att få in råmaterial
-----------------------------

Ibland kan det hända att en användare skickar en begäran med en olämplig HTTP-metod och lägger till sin egen inmatning ovanpå den. Eller, till exempel, skickar en binär fil eller dåliga HTTP-rubriker.

I ett sådant fall är det bra att använda inhemsk indata, som erhålls i PHP på följande sätt:

```php
$input = file_get_contents('php://input');
```

När jag implementerade REST API-biblioteket stötte jag också på ett antal specialfall där olika typer av webbservrar felaktigt beslutade om HTTP-huvuden för inmatning, eller där användaren felaktigt skickade in formulärdata osv.

I det här fallet kunde jag implementera den här funktionen som löser nästan alla fall (implementationen är beroende av `Nette\Http\RequestFactory`, men du kan ersätta detta beroende med något annat i ditt specifika projekt):

```php
/**
 * Hämtar POST-data direkt från HTTP-huvudet eller försöker analysera data från strängen.
 * Vissa äldre klienter skickar data som json, som är i bassträngformat, så det är obligatoriskt att kasta fältet till en array.
 *
 * @return array<string|int, mixed>
 */
private function getBodyParams(string $method): array
{
	if ($method === 'GET' || $method === 'DELETE') {
		return [];
	}

	$request = (new RequestFactory())->fromGlobals();
	$return = array_merge((array) $request->getPost(), $request->getFiles());
	try {
		$post = array_keys($_POST)[0] ?? '';
		if (str_starts_with($post, '{') && str_ends_with($post, '}')) { // stöd för äldre klienter
			$json = json_decode($post, true, 512, JSON_THROW_ON_ERROR);
			if (is_array($json) === false) {
				throw new LogicException('Json är inte en giltig matris.');
			}
			unset($_POST[$post]);
			foreach ($json as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// Tystnad är guld.
	}
	try {
		$input = (string) file_get_contents('php://input');
		if ($input !== '') {
			$phpInputArgs = (array) json_decode($input, true, 512, JSON_THROW_ON_ERROR);
			foreach ($phpInputArgs as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// Tystnad är guld.
	}

	return $return;
}
```
