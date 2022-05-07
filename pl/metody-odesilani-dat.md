Metody wysyłania danych (GET i POST)
====================================

> id: '32f9083f-7cb1-469f-911a-765df059123d'
> slug:
> 	cs: metody-odesilani-dat
> 	pl: metody-wysylania-danych-get-i-post
> 
> perex:
> 	- 'Metoda GET a POST, získávání dat z formuláře a URL. Komunikace přes API a zpracování dat.'
> 	- 'Metody GET i POST, pobieranie danych z formularza i adresu URL, komunikacja z interfejsem API i przetwarzanie danych.'
> 
> publicationDate: '2019-11-26 11:38:32'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '2282538597ebfe95877ae0e005ddd352'

Oprócz zwykłych zmiennych, w PHP mamy również tak zwane **zmienne superglobalne**, które przenoszą informacje o aktualnie wywoływanej stronie i przekazywanych przez nią danych.

Zazwyczaj na stronie znajduje się formularz, który wypełnia użytkownik, a my chcemy przesłać te dane na serwer WWW, gdzie zostaną one przetworzone w PHP.

W tym celu najczęściej stosuje się trzy metody:

- `GET` ~ dane są przekazywane w adresie URL jako parametry
- `POST` ~ dane są przekazywane niejawnie wraz z żądaniem strony
- <a href="/ajax-post">Ajax POST</a> ~ asynchroniczne przetwarzanie javascript

Metoda GET - `$_GET`.
--------------------

Dane wysyłane metodą GET są widoczne w adresie URL (jako parametry po znaku zapytania), maksymalna długość to 1024 znaki w Internet Explorerze (inne przeglądarki *prawie* tego nie ograniczają, ale większe teksty nie powinny być przekazywane w ten sposób). Zaletą tej metody jest przede wszystkim prostota (widać, co się wysyła) oraz możliwość zamieszczenia linku do wyniku przetwarzania. Dane są przesyłane do zmiennej.

Adres strony odbierającej może wyglądać następująco:

`https://____________.com/script.php?promenna=obsah&promenna2=obsah`.

W PHP możemy wtedy, na przykład, zapisać wartość parametru `zmienna` w następujący sposób:

```php
echo $_GET['promenada'];	// drukuje "treść"
```

> Ta metoda zapisu danych bezpośrednio na stronie HTML nie jest bezpieczna, ponieważ w adresie URL możemy przekazać np. kod HTML, który zostanie zapisany na stronie, a następnie wykonany.
>
> Musimy **zawsze** przetwarzać dane przed wysłaniem ich na stronę, służy do tego funkcja `htmlspecialchars()`.
>
> Na przykład: `echo htmlspecialchars($_GET['variable']);`.

Metoda POST - `$_POST`.
----------------------

Dane przesyłane metodą POST nie są widoczne w adresie URL, co rozwiązuje problem maksymalnej długości przesyłanych danych. Do przesyłania pól formularzy należy zawsze używać metody POST, ponieważ gwarantuje ona, że np. hasła nie będą widoczne i że nie będzie można podać łącza do strony przetwarzającej wynik określonego wprowadzania danych.

Dane są dostępne w zmiennej `$_POST`, a sposób użycia jest taki sam jak w przypadku metody GET.

Sprawdzenie istnienia przesłanych danych
--------------------------------

Przed przystąpieniem do przetwarzania danych należy sprawdzić, czy dane zostały rzeczywiście przesłane, w przeciwnym razie uzyskalibyśmy dostęp do
 do nieistniejącej zmiennej, co spowodowałoby wyświetlenie komunikatu o błędzie.

Funkcja `isset()` służy do sprawdzania istnienia zmiennej.

```php
if (isset($_GET['Nazwa'])) {
    echo 'Imię i nazwisko:' . htmlspecialchars($_GET['Nazwa']);
} else {
    echo 'Nie wprowadzono żadnej nazwy.';
}
```

Formularz wprowadzania danych
------------------------

Formularz jest napisany w języku HTML, a nie w PHP. Może to być zwykła strona HTML. Całą "magią" zajmuje się skrypt PHP, który przyjmuje dane.

Na przykład możemy użyć formularza do odebrania 2 liczb, wysłanych metodą GET:

```html
<form action="script.php" method="get">
    První číslo: <input type="text" name="x">
    Druhé číslo: <input type="text" name="y">

    <input type="submit" value="Sečíst čísla">
</form>
```

W pierwszym wierszu widać, gdzie zostaną wysłane dane i jaką metodą.

Następne 2 wiersze to proste elementy formularza, zwróć uwagę na atrybut **name=""**, w którym znajduje się nazwa zmiennej, która będzie przechowywała to, co znajduje się teraz w formularzu.

Następnie umieszcza się przycisk służący do przesyłania danych (obowiązkowy) oraz zamykający formularz znacznik HTML (obowiązkowy, aby przeglądarka wiedziała, co jeszcze należy przesłać, a co nie).

> Na jednej stronie możemy mieć dowolną liczbę formularzy i nie mogą one być zagnieżdżone. Jeśli występuje zagnieżdżanie, zawsze wysyłany jest najbardziej zagnieżdżony formularz, a pozostałe są ignorowane.

Przetwarzanie formularzy na serwerze
-------------------------------

Teraz mamy gotowy formularz HTML i wysyłamy go do pliku `script.php`, który odbiera dane za pomocą metody GET. Adres żądania strony może wyglądać następująco:

`https://________.com/script.php?x=5&y=3`

**script.php**.

```php
$x = $_GET['x'];	// 5
$y = $_GET['y'];	// 3

echo $x + $y;		// drukuje 8
```

Prawidłowo, najpierw powinniśmy sprawdzić, czy oba pola formularza zostały wypełnione, służy do tego funkcja `isset()`:

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

Przetwarzanie danych otrzymanych za pomocą metody POST
--------------------------------------

Jeśli dane są odbierane metodą POST, adres URL skryptu, który ma zostać przetworzony, będzie zawsze wyglądał tak:

`https://________.com/script.php`.

I nigdy inaczej. Po prostu nie. Dane są ukryte w żądaniu HTTP i nie możemy ich zobaczyć.

> Ze względów bezpieczeństwa do przesyłania nazw użytkowników i haseł wymagana jest ukryta metoda POST.
>
> **Bezpieczeństwo:** Jeśli na swojej stronie używasz haseł, formularz logowania i rejestracji powinien być umieszczony na HTTPS, a hasła muszą być odpowiednio szyfrowane (np. za pomocą BCrypt).

Obsługa żądań ajaxowych
------------------------------

W niektórych przypadkach, podczas przetwarzania żądań ajaxowych, pobranie danych może nie być łatwe. Dzieje się tak, ponieważ biblioteki ajaxowe zwykle wysyłają dane w postaci `json payload`, podczas gdy zmienna superglobalna `$_POST` zawiera tylko dane formularza.

Dostęp do danych nadal jest możliwy, szczegóły opisałem w artykule <a href="/ajax-post">Obsługa żądań ajax POST</a>.
