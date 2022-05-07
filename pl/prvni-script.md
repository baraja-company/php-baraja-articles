Jak napisać swój pierwszy skrypt PHP
====================================

> id: '2d6986dc-f24b-4ae5-b24c-d5bb9bf94512'
> slug:
> 	cs: prvni-script
> 	pl: jak-napisac-swoj-pierwszy-skrypt-php
> 
> perex:
> 	- Jak napsat první PHP script? Úvod do PHP pro začátečníky.
> 	- Jak napisać swój pierwszy skrypt PHP? Wprowadzenie do języka PHP dla początkujących.
> 
> publicationDate: '2020-02-16 16:26:08'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: a5fab0e5edf99323140e497a6fe52322

Zanim napiszemy nasz pierwszy skrypt PHP, musimy najpierw teoretycznie wyjaśnić, jak wczytać stronę za pomocą PHP.

1. najpierw użytkownik wywołuje w przeglądarce internetowej określony adres URL, na przykład `https://baraja.cz`.
2. Następnie przeglądarka internetowa tworzy `request`, czyli specjalne żądanie do serwera WWW, które jest wysyłane do Internetu. Zawiera on informacje o żądanej stronie, podstawowe dane identyfikacyjne i ustawienia przeglądarki, informacje o plikach cookie itd.
3. To `zapytanie` wędruje przez Internet do serwera WWW (najczęściej `Apache`), który odczytuje je i rozpoczyna kompilację odpowiedzi.
4. Ponieważ wywołany adres URL jest skryptem PHP, a żądanie dotyczy pliku o nazwie `index.php`, `Apache` odczytuje plik `index.php` z katalogu głównego na dysku i przekazuje go do `interpretera PHP`, czyli programu, który może przetwarzać sam kod PHP i na jego podstawie budować kod `HTML`, który jest odsyłany do użytkownika.
5. Po skompilowaniu kodu HTML odpowiedź jest wysyłana z powrotem do użytkownika (jest to tzw. "odpowiedź"), a przeglądarka internetowa renderuje stronę w normalny sposób, tak jakby był to czysty HTML.

> Zwróć uwagę, że przeglądarka internetowa nie dowiaduje się niczego o zawartości skryptu PHP, a jedynie przetwarza wygenerowany HTML, więc Twoje skrypty i zawartość serwera pozostają bezpieczne.

Utwórzmy pierwszy skrypt
----------------------------

> Pisząc pierwszy skrypt, zakłada się, że na komputerze działa serwer WWW. Dla systemu Windows najlepszy jest <a href="https://www.apachefriends.org/index.html">XAMPP</a> (pobierz PHP w wersji 7.0 lub nowszej), a <a href="https://www.apachefriends.org/index.html">XAMPP</a> działa dokładnie tak samo na Macu, jak i na Windowsie. Dla Linuksa polecam <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">serwer LAMP</a> (ta strona również działa na serwerze Lamp).

Nazwa pliku skryptu PHP musi kończyć się rozszerzeniem `.php`, aby serwer WWW wiedział, że chcemy go przetworzyć zgodnie z zasadami PHP. Utwórzmy więc na przykład plik `index.php`, który będzie zawierał kod strony głównej naszej witryny.

Otwieranie pliku
--------------------------

Otwórz ten plik w edytorze tekstu odpowiednim do pisania kodu źródłowego.

Na przykład w systemie Windows dobrym początkiem jest <a href="https://www.sublimetext.com">Sublime Text</a>, ponieważ ładnie koloruje składnię (reguły języka) i ułatwia czytanie kodu. Później polecam zakup <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, który jest często używany w firmach i oferuje możliwość programowania wielu osób.

Napisz podstawową strukturę strony HTML
--------------------------

Prawdopodobnie znasz już podstawową strukturę strony HTML:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>

    </body>
</html>
```

Cały kod HTML będzie obsługiwany w normalny sposób i będzie bardzo pomocny przy projektowaniu witryny. PHP w dużym stopniu wykorzystuje zasady HTML i CSS.

Oddzielenie skryptu PHP od kodu HTML
--------------------------

PHP to przede wszystkim język szablonów, który generuje własną treść w odpowiednich miejscach kodu. Aby wyraźnie odróżnić, co jest HTML-em, a co PHP, musimy użyć znacznika separatora.

Obecnie najlepiej jest używać notacji z `<?php` i `?>`.

```php
// tutaj znajduje się kod PHP
?>
```

> Terminatora `?>` używamy, jeśli chcemy użyć innego kodu HTML. Jeśli na końcu skryptu PHP nie ma więcej kodu HTML, lepiej jest nie umieszczać znacznika `?>`, aby na końcu strony nie było niepotrzebnych białych spacji i przerw w linii, które może wstawić edytor tekstu.

W przeszłości często używano znacznika `<?` zamiast `<?php`, ale nie zawsze jest on obsługiwany.

Znaczniki opakowujące można umieszczać w dowolnym miejscu kodu HTML, na przykład w treści strony:

```html
<!DOCTYPE HTML>
<html>
    <head>
    <title>Můj první PHP script</title>
    <meta charset="UTF-8">
    </head>
    <body>
    <?php
        // tady bude PHP kód
    ?>
    </body>
</html>
```

Podstawowe konstrukcje
--------------------------

Do podstawowych elementów konstrukcyjnych należą:

- <a href="/echo">Listowanie zawartości w kodzie źródłowym</a>.
- <a href="/zmienna">Zmienne</a>.
- <a href="/if">Warunki</a>.

W tym rozdziale zademonstrujemy proste listowanie zawartości do kodu źródłowego przy użyciu zmiennych.

Zasada budowy kodu źródłowego
--------------------------------

Wszystkie konstrukcje (wyrażenia językowe), instrukcje i funkcje są oddzielone średnikami, aby jednoznacznie określić, od czego i do czego dana konstrukcja jest ważna.

Po średniku zwykle następuje przerwa w wierszu.

Symbolicznie zapisane:

```php
příkaz;
další příkaz;
proměnná x = její hodnota;

vypsat proměnnou x;

uložit do souboru;
```

Wyciągnij kod źródłowy
--------------------------

Konstrukcja <a href="/echo">echo</a> jest używana do wypisywania zawartości.

Jest bardzo łatwy w użyciu:

```php
echo 'Witaj, świecie!';
```

Następnie wypisuje tekst "Witaj świecie!" w kodzie HTML. Wypróbuj próbkę.

> Wszystkie inne dema będą zawierały tylko kod PHP. Otaczający kod HTML można dowolnie modyfikować (na przykład skorzystać z przykładu zamieszczonego na początku tego artykułu).

Zmienne
--------------------------

Zmienne to wirtualne miejsca w pamięci, w których przechowywane są dane i które służą do ich przenoszenia. Nazwa zmiennej zawsze zaczyna się od `dolara`, po którym następuje sama `nazwa`, a następnie `wartość`.

> Szczegółowy opis działania zmiennych zamieściłem w <a href="/zmienna">odrębnym artykule na temat zmiennych</a>.

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo;
echo '<br>.';
echo $jmenoAutora;
```

> Nazwa zmiennej powinna wyrażać to, co zmienna faktycznie zawiera, aby kod był bardziej przejrzysty. Zwróć też uwagę na wstawienie znacznika HTML `<br>` w celu wcięcia tekstu. Znacznik ten powinien być już znany z języka HTML.

To, co jest wyprowadzane w konstrukcji `echo`, nazywane jest łańcuchem (ciągiem znaków). Poszczególne łańcuchy można łączyć kropką (`.`), aby zredukować dane wyjściowe do jednego wiersza:

```php
$oblibeneCislo = 1024;
$jmenoAutora = 'Jan Barášek';

echo $oblibeneCislo . '<br>.' . $jmenoAutora;
```

> Po połączeniu ciągów kropką całość będzie widoczna jako jeden duży ciąg.

Operacje zmienne
--------------------------

Pomiędzy zmiennymi wszystkie podstawowe <a href="/matematyka">operacje matematyczne</a> działają intuicyjnie tak, jak należy.

Zdefiniujmy dwie zmienne i wpiszmy do nich liczby:

```php
$x = 5; // definiuje zmienną x o wartości 5
$y = 3; // definiuje zmienną y o wartości 3

echo $x + $y; // dodaje zmienne i wypisuje 8
```

> Należy pamiętać, że znak równości (`=`) nie jest używany do wykonywania operacji matematycznych, więc nie można na przykład pisać równań. PHP pod tym względem zachowuje się jak kalkulator.

Jeśli nie chcemy używać zmiennych, możemy wykonywać operacje bezpośrednio. Nie ma więc znaczenia miejsce prowadzenia działalności, a oceny będą dokonywane w dowolnym miejscu.

```php
echo 5 + 3; // drukuje 8
```

Alternatywnie możemy dodać zmienne i zapisać wynik w innej zmiennej:

```php
$x = 5;
$y = 3;
$z = $x + $y; // zmienna $z zawiera 8

echo $z; // drukuje 8
```

W następnej części poznamy kompletne podstawy <a href="/principles-of-variables">definicji i użycia zmiennych</a>.
