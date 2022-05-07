Formularze, przetwarzanie formularzy w PHP
==========================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	pl: formularze-przetwarzanie-formularzy-w-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

Przypuszczam, że utworzyliśmy formularz HTML, który wysyłamy, a teraz chcemy przetworzyć dane. O tworzeniu formularza HTML można przeczytać w <a href="/formularz">oddzielnym artykule</a>.

Odbieranie danych - różne sposoby
----------------------------

Sposób wysyłania formularza jest określany bezpośrednio w kodzie HTML

Dostępne są 2 opcje:

- **GET** - jest widoczny w pasku adresu po znaku zapytania
 Na przykład: `php.baraja.cz/search.php?query=formulare`.
- **POST** - ukryty (niewidoczny), większość formularzy jest wysyłana pocztą

Następnie musimy je odczytać w PHP, stosując tę samą metodę.

Pobieranie danych od użytkownika i przekazywanie ich do skryptu
------------------------------------------------------

Podstawą jest formularz HTML, o tym, jak go stworzyć, można przeczytać <a href="/formularz">w osobnym artykule</a>.

Na początek załóżmy prosty formularz służący do wprowadzania imienia i nazwiska użytkownika:

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

Zostanie wyświetlone pole tekstowe, w którym należy wpisać nazwę i kliknąć przycisk Wyślij. Gdy przycisk zostanie kliknięty, zawartość pola zostanie wysłana do skryptu `welcome.php`.

Teraz zajmiemy się właściwym przetwarzaniem danych w pliku `welcome.php`:

```php
$username = $_GET['nazwa użytkownika'];

echo 'Wprowadzona nazwa to:' . $username;
```

Zwróć uwagę na specjalną zmienną `$_GET`. Jest to **zmienna globalna**, która zawiera dane z formularza i może być dostępna jako tablica.

Problem z tym rozwiązaniem polega jednak na tym, że otrzymane dane nie są **bezpieczne** i podobny formularz można łatwo zaatakować. Na przykład potencjalny napastnik może wpisać w polu zamiast nazwy kod javascript, który zostanie zapisany na stronie i wykonany.

Dlatego zawsze należy oczyszczać dane użytkownika przed umieszczeniem ich w kodzie HTML:

```php
$username = $_GET['nazwa użytkownika'] ?? 'Nieznany';

echo 'Wprowadzona nazwa to:' . htmlspecialchars($username);
```

Dalsze przetwarzanie
----------------

Z otrzymanymi danymi możemy zrobić wszystko i traktować je jak zwykłe zmienne.

Na przykład dodaj wartości w dwóch polach:

```php
echo $_GET['x'] + $_GET['y'];
```

Lub zapisać do pliku, bazy danych, wiadomości e-mail, ...

W tym celu można skorzystać z następujących funkcji:

- <a href="/file-put-contents">file_put_contents</a> - funkcja zapisująca dane do pliku
- <a href="/hashovani">MD5</a> - obliczanie sumy kontrolnej, na przykład dla haseł
- <a href="/cookies">Cookies</a> - zapisywanie danych do **cookies** (małych plików wewnątrz przeglądarki internetowej)
