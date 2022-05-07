Sesje - pliki cookie serwera w PHP
==================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	pl: sesje---pliki-cookie-serwera-w-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Często musimy przechowywać więcej informacji w plikach <a href="/cookies">cookies</a>, ale maksymalny limit dla plików cookie wynosi 4 kB, czyli niewiele. Sesje rozwiązują ten problem, ponieważ przechowują dane na serwerze WWW, a w przeglądarce klienta przechowują jedynie krótki identyfikator pozwalający stwierdzić, które dane należą do którego klienta.

Rozpoczynanie sesji
---------------------

Przed przystąpieniem do pracy z sesjami należy je najpierw uruchomić. Odbywa się to przez wywołanie funkcji `session_start()` na samym początku skryptu:

```php
session_start();
```

> Silne ostrzeżenie: przed wywołaniem funkcji `session_start()` nie może być wykonywany żaden kod wyjściowy do kodu HTML!

Bezpieczeństwo sesji
-------------------

Zawartość sesji jest przechowywana na serwerze, a do przeglądarki klienta wysyłany jest tylko identyfikator, więc użytkownik nie ma możliwości sprawdzenia, co jest przechowywane w sesji. Jedynym sposobem, w jaki skrypt może wpłynąć na użytkownika, jest usunięcie identyfikatora (wówczas skrypt wygeneruje nowy).

Pobieranie danych z sesji
----------------------

Wszystkie sesje są przechowywane w zmiennej superglobalnej `$_SESSION` i mogą być przeglądane jako tablica.

Na przykład nazwę aktualnie zalogowanego użytkownika można uzyskać, pisząc:

```php
echo $_SESSION['użytkownik'];
```

Uwaga: Sesja może nie zawsze istnieć (np. jeśli jesteś nowym użytkownikiem). Dlatego przed wpisaniem na listę należy zawsze sprawdzić, czy istnieje i w razie potrzeby zaproponować alternatywny komunikat o błędzie.

```php
if (isset($_SESSION['użytkownik']) && $_SESSION['użytkownik']) {
    echo 'Zalogowany użytkownik:' . $_SESSION['użytkownik'];
} else {
    echo 'Nikt nie jest zalogowany.';
}
```

Zapisywanie danych w sesji
----------------------

Zapisywanie odbywa się jako zwykłe zapisywanie danych do zmiennej:

```php
$_SESSION['użytkownik'] = 'Honzik';
```

Serwer WWW zajmie się technicznym zapewnieniem prawidłowego przechowywania danych na serwerze i przesyłania identyfikatora do użytkownika.

Usuwanie sesji
----------------

Poszczególne wartości można usuwać oddzielnie, zgodnie z kluczem:

```php
unset($_SESSION['użytkownik']);
```

Można też wybrać wszystkie dostępne sesje:

```php
unset($_SESSION);
```

> Uwaga: Usunięcie określonej sesji nie powoduje opróżnienia wartości klucza, ale powoduje całkowite usunięcie klucza. Dlatego przy próbie odczytania nieistniejącego klucza zostanie wyświetlone ostrzeżenie o błędzie. Zawsze możemy łatwo sprawdzić istnienie klucza za pomocą funkcji `isset()`.

Maksymalna ważność sesji
---------------------------------

Każda zapisana sesja ma limit czasu, przez jaki będzie przechowywana na serwerze. PHP bezpośrednio zawiera skrypt cron, który okresowo usuwa stare sesje.

Domyślnie jest to zwykle `1440 sekund`, czyli `24 minuty`.

Zwiększanie wartości należy przeprowadzić w dwóch miejscach:

- W <a href="/info">`php.ini`` ustawiana jest maksymalna długość ważności, jaką serwer będzie utrzymywał</a>. Wartość ta jest ustawiana przez dyrektywę `session.gc_maxlifetime`,
- Po stronie skryptu PHP należy określić bieżącą żądaną ważność.

Użycie w PHP:

```php
// serwer będzie teraz utrzymywał sesję przez maksymalnie 3600 sekund = 1 godzinę
ini_set('session.gc_maxlifetime', '3600');

// wszyscy klienci (przeglądarki) będą
// sesja wysłana z okresem ważności wynoszącym dokładnie 3600 sekund
session_set_cookie_params(3600);

session_start(); // możemy rozpocząć sesję!
```
