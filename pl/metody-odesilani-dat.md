Metody wysyłania danych (GET i POST)
====================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	pl: metody-wysylania-danych-get-i-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- 'Metoda GET i POST, pobieranie danych z formularza i adresu URL. Komunikacja API i przetwarzanie danych.'
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '81b5f92d7ee05563b6ece295ed5958d3'

Oprócz zwykłych zmiennych, w PHP mamy również tzw. **superglobalne zmienne**, które przenoszą informacje o aktualnie wywoływanej stronie i przekazywanych przez nas danych.

Zazwyczaj na stronie mamy formularz, który użytkownik wypełnia i chcemy te dane przekazać na serwer www, gdzie przetwarzamy je w PHP.

Najczęściej stosuje się do tego 3 metody:

- `GET` ~ dane są przekazywane w URL jako parametry
- `POST` ~ dane są przekazywane niejawnie wraz z żądaniem strony
- <a href="/ajax-post">Ajax POST</a> ~ asynchroniczne przetwarzanie javascript

Metoda GET - `$_GET`.
--------------------

Dane wysyłane metodą GET są widoczne w adresie URL (jako parametry po znaku zapytania), maksymalna długość to 1024 znaki w Internet Explorerze (inne przeglądarki *prawie* nie ograniczają, ale większe teksty nie powinny być przekazywane w ten sposób). Zaletą tej metody jest przede wszystkim prostota (widać co wysyłamy) oraz możliwość podania linku do wyniku przetwarzania. Dane są przesyłane do zmiennej.

Adres strony odbiorczej może wyglądać tak:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`.

W PHP możemy wtedy np. zapisać wartość parametru `zmienna` w następujący sposób:

```php
echo $_GET['promenada'];	// drukuje "treść"
```

> **Strong warning:** Ta metoda zapisu danych bezpośrednio na stronie HTML nie jest bezpieczna, ponieważ możemy przekazać np. kod HTML w adresie URL, który zostałby zapisany na stronie, a następnie wykonany.
>
> Musimy **zawsze** traktować dane przed jakimkolwiek wyjściem na stronę, służy do tego funkcja `htmlspecialchars()`.
>
> Na przykład: `echo htmlspecialchars($_GET['variable']);`.

Metoda POST - `$_POST`
----------------------

Dane wysyłane metodą POST nie są widoczne w adresie URL, co rozwiązuje problem maksymalnej długości wysyłanych danych. Do przesyłania pól formularza należy zawsze używać metody POST, ponieważ dzięki temu np. hasła nie będą widoczne i nie będzie można podać linku do strony przetwarzającej wynik danego wejścia.

Dane są dostępne w zmiennej `$_POST`, a sposób użycia jest taki sam jak dla metody GET.

Weryfikacja istnienia przekazanych danych
--------------------------------

Przed przetwarzaniem jakichkolwiek danych powinniśmy najpierw sprawdzić, czy dane zostały rzeczywiście przesłane, w przeciwnym razie uzyskalibyśmy dostęp do
 do nieistniejącej zmiennej, co wyrzuciłoby komunikat o błędzie.

Funkcja `isset()` służy do sprawdzenia istnienia zmiennej.

```php
if (isset($_GET['Nazwa'])) {
    echo 'Twoje nazwisko:' . htmlspecialchars($_GET['Nazwa']);
} else {
    echo 'Nie wprowadzono żadnego nazwiska.';
}
```

Formularz do wprowadzania danych
------------------------

Formularz jest wykonany w HTML, a nie w PHP. Może być na zwykłej stronie HTML. Całą "magią" zajmuje się skrypt PHP, który przyjmuje dane.

Dla przykładu możemy użyć formularza do otrzymania 2 liczb, wysłanych metodą GET:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

W pierwszej linijce widać, gdzie zostaną wysłane dane i jaką metodą.

Następne 2 linie to proste elementy formularza, zwróć uwagę na atrybut **name=""**, jest tam nazwa zmiennej, która będzie przechowywała to, co jest teraz w formularzu.

Następnie pojawia się przycisk do przesłania danych (obowiązkowy) oraz zamykający formularz znacznik HTML (obowiązkowy, aby przeglądarka wiedziała, co jeszcze przesłać, a co nie).

> Na jednej stronie możemy mieć dowolną ilość formularzy i nie mogą być one zagnieżdżone. Jeśli występuje zagnieżdżanie, zawsze wysyłany jest najbardziej zagnieżdżony formularz, a pozostałe są ignorowane.

Przetwarzanie formularzy na serwerze
-------------------------------

Teraz mamy gotowy formularz HTML i wysyłamy go do `script.php`, który odbiera dane za pomocą metody GET. Adres żądania strony może wyglądać tak:

`https://________.com/script.php?x=5&y=3`

**script.php**

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// drukuje 8
```

Prawidłowo powinniśmy najpierw sprawdzić, czy oba pola formularza zostały wypełnione, służy do tego funkcja `isset()`:

```php
if (isset($_GET['x']) && isset($_GET['y'])) {
    $x = $_GET['x'];	// 5
    $y = $_GET['y'];	// 3

    echo $x + $y;		// drukuje 8
} else {
    echo 'Formularz nie został wypełniony prawidłowo.';
}
```

> **TIP:** Możesz przekazać wiele parametrów do konstrukcji `isset()`, aby sprawdzić, czy wszystkie one istnieją.
>
> Dlatego zamiast `isset($_GET['x']) && isset($_GET['y'])`, można po prostu określić:
>
> `isset($_GET['x'], $_GET['y'])`.

Przetwarzanie danych otrzymanych metodą POST
--------------------------------------

Jeśli dane są odbierane metodą POST, adres URL skryptu do przetworzenia zawsze będzie wyglądał tak:

`https://________.com/script.php`.

I nigdy inaczej. Po prostu nie. Dane są ukryte w żądaniu HTTP i nie możemy ich zobaczyć.

> Ukryta metoda POST jest wymagana do wysyłania nazw użytkowników i haseł ze względów bezpieczeństwa.
>
> **Bezpieczeństwo:** Jeśli pracujesz z hasłami na swojej stronie, formularz logowania i rejestracji powinien być hostowany na HTTPS i musisz odpowiednio hashować hasła (na przykład za pomocą BCrypt).

Obsługa żądań ajaxowych
------------------------------

W niektórych przypadkach, podczas przetwarzania żądań ajaxowych, pobieranie danych może nie być łatwe. Dzieje się tak dlatego, że biblioteki ajaxowe zazwyczaj wysyłają dane jako `json payload`, natomiast zmienna superglobalna `$_POST` zawiera tylko dane formularza.

Do danych nadal można się dostać, szczegóły opisałem w artykule <a href="/ajax-post">Przetwarzanie żądań ajax POST</a>.

Uzyskanie surowych danych wejściowych
-----------------------------

Czasami może się zdarzyć, że użytkownik wysyła żądanie przy użyciu niewłaściwej metody HTTP i dodaje do niego własne dane wejściowe. Albo np. wysyła plik binarny, albo złe nagłówki HTTP.

W takim przypadku dobrze jest użyć natywnego wejścia, które uzyskuje się w PHP w następujący sposób:

```php
$input = file_get_contents('php://wejście');
```

Podczas wdrażania biblioteki REST API napotkałem również wiele specjalnych przypadków, w których różne typy serwerów internetowych nieprawidłowo decydowałyby o wejściowych nagłówkach HTTP lub użytkownik nieprawidłowo przesyłałby dane formularza itp.

Dla tego przypadku udało mi się zaimplementować tę funkcję, która rozwiązuje prawie wszystkie przypadki (implementacja zależy od `NetteHttpRequestFactory`, ale możesz zastąpić tę zależność czymś innym w swoim konkretnym projekcie):

```php
/**
 * Pobiera dane POST bezpośrednio z nagłówka HTTP lub próbuje parsować dane z łańcucha.
 * Niektórzy klienci legacy wysyłają dane jako json, który jest w formacie ciągu podstawowego, więc rzutowanie pól na tablicę jest obowiązkowe.
 *
 * @return array<string|int, mixed>.
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
		if (str_starts_with($post, '{') && str_ends_with($post, '}')) { // wsparcie dla starszych klientów
			$json = json_decode($post, true, 512, JSON_THROW_ON_ERROR);
			if (is_array($json) === false) {
				throw new LogicException('Json nie jest poprawną tablicą.');
			}
			unset($_POST[$post]);
			foreach ($json as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// Milczenie jest złotem.
	}
	try {
		$input = (string) file_get_contents('php://wejście');
		if ($input !== '') {
			$phpInputArgs = (array) json_decode($input, true, 512, JSON_THROW_ON_ERROR);
			foreach ($phpInputArgs as $key => $value) {
				$return[$key] = $value;
			}
		}
	} catch (Throwable $e) {
		// Milczenie jest złotem.
	}

	return $return;
}
```
