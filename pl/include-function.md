Konstrukcja obejmuje
====================

> id: '881cc6ef-b1dc-4a71-ab22-d1943ce8095b'
> slug:
> 	cs: include-function
> 	pl: konstrukcja-obejmuje
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '53294e511809a77c08883100fd7416c6'

| Obsługa | PHP 4, PHP 5, PHP 7
|---------------|---------
| Krótki opis | Dołącza do skryptu inny plik tekstowy lub skrypt.
| Wymagania | Inny plik tekstowy lub skrypt, który ma zostać wstawiony.
| Uwaga | Nie można załadować plików zewnętrznych.

Opis
--------------------------

Wstawia do strony inny plik tekstowy lub skrypt. Obsługuje pliki zwykłe/tekstowe.

Pliki osadzone zachowują się tak, jakby znajdowały się bezpośrednio na stronie.

Wbudowane skrypty są wykonywane automatycznie.

Wbudowany skrypt przekazuje wartości zmiennych.

> Nie można załadować z pamięci zewnętrznej. Może odczytywać tylko zwykły niesformatowany tekst i pliki PHP.

Dozwolone formaty ścieżek:

- `script.php` - plik w tym samym katalogu, zostanie wykonany
- `script.html` - plik w tym samym katalogu, nie zostanie wykonany
- `./file.php` - plik w tym samym katalogu, zostanie wykonany
- `../strona.html`.
- `folderfile.php` - zapis w systemie Windows
- `address/DalsiAddress/file.php` - zapis w systemach Unix

Ukośniki typu `****` i `**/**` mogą być konwertowane, więc nie trzeba się tym zajmować.

Niewłaściwy wpis w ścieżce: `https://domena.pripona/slozka/soubor.php`.

Podobne funkcje
--------------------------

- <a href="/file-get-contents">file_get_contents()</a>.

Przykład
--------------------------

```php
include 'file.php';
```

Wstawia on skrypt `file.php` na stronę i uruchamia go.

`vars.php`.

```php
$color = 'zielony';
$fruit = 'jabłko';
```

`test.php`.

```php
$color = '';
$fruit = '';

echo 'A' . $color . '' . $fruit; // A

include 'vars.php';

echo 'A' . $color . '' . $fruit; // Zielone jabłko
```

> UWAGA: Poniższa notacja nie jest możliwa, należy przekazywać zawartość zmiennych poprzez ich definiowanie!

```php
include 'file.php?parameter=neco';
```

Wartości zwracane
--------------------------

Brak, po prostu umieszcza plik.

**UWAGA:** Umożliwia porównywanie zawartości plików, ale stanowi to zagrożenie dla bezpieczeństwa. Kolejność nawiasów ma znaczenie! Przykład:

```php
if ((include 'file.php') == 'OK') {
    echo 'Wartością jest "OK".';
}
```

> Jest to potencjalne zagrożenie bezpieczeństwa!
>
> Można to rozwiązać za pomocą **file_get_contents()**, **readfile()** lub **fopen()**. Funkcja Fopen() powinna być używana tylko do plików .txt i .html.
>
> Bezpieczniejsze odczytywanie pliku można rozwiązać, definiując własną funkcję.

Przykład:

```php
$string = get_include_contents('somefile.php');

function get_include_contents(string $filename): ?string
{
    if (is_file($filename)) {
        ob_start();
        include (string) $filename;
        return ob_get_clean();
    }

    return null;
}
```

Uwagi i wskazówki od innych programistów
--------------------------

**Jakub Vrána** wysłał mi e-mail:

```php
include 'artykuły/' . $_GET['Artykuł'] . '.html';
```

Jest to bardzo niebezpieczne.

Atakujący może przekazać łącze do innego katalogu, używając `../` lub czegoś podobnego jako nazwy artykułu, a czasami możliwe jest pozbycie się końcówki przez przekazanie na końcu bajtu null.

Powinieneś przynajmniej użyć funkcji `basename()`, ale lepiej dopuścić tylko wartości z białej listy.
