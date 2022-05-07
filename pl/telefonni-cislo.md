Walidacja i formatowanie numerów telefonów
==========================================

> id: bbfe35c3-3a8c-4fe5-a9bd-5bff0fc234e0
> slug:
> 	cs: telefonni-cisla
> 	pl: walidacja-i-formatowanie-numerow-telefonow
> 
> publicationDate: '2021-06-18 09:20:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '224b3857a6bb898d9d322499cd1d3757'

Nie ma prostego sposobu na sprawdzanie poprawności i formatowanie numerów telefonów w PHP, więc napisałem prostą bibliotekę, która nie ma żadnych zależności, a mimo to radzi sobie z tą rolą.

Celem jest sprawdzenie formatu numeru telefonu lub przekonwertowanie go na podstawową postać kanoniczną (która jest zawsze poprawna).

Instalacja strony
---------

Po prostu przez kompozytora:

```txt
$ composer require baraja-core/phone-number
```

Można też pobrać pakiet [Download on GitHub](https://github.com/baraja-core/phone-number).

Jak korzystać z biblioteki
----------

Zasada działania tego narzędzia opiera się na formatowaniu i walidacji numerów telefonów pochodzących od użytkownika lub ze źródeł, nad którymi użytkownik nie ma kontroli.

Najczęstszym zastosowaniem jest poprawianie formatowania numeru telefonu:

```php
$original = '+420 777123456';
$formatted = \Baraja\PhoneNumber\PhoneNumberFormatter::fix($original);

echo $original . '<br>.';
echo $formatted;
```

Funkcja koryguje formatowanie liczby i zwraca łańcuch `+420 777 123 456`.

Jeśli użytkownik nie określi prefiksu, przyjmowany jest prefiks `+420`. Za pomocą drugiego parametru można zmienić domyślne preferencje:

```php
// zwroty: +421 777 123 456
\Baraja\PhoneNumber\PhoneNumberFormatter::fix('+420 777123456', 421);
```

Kod telefonu jest nadpisywany tylko wtedy, gdy użytkownik go nie wprowadzi i nie zostanie on wykryty automatycznie.

Formatowanie wejścia i wyjścia
----------------------------

Ciąg wejściowy może wyglądać (prawie) dowolnie. Wbudowany algorytm może automatycznie usuwać nieprawidłowe znaki (np. niektórzy użytkownicy wpisują obok numeru telefonu dopisek, który zostanie automatycznie usunięty). Nie trzeba więc martwić się o formatowanie danych wejściowych, ale dane wyjściowe będą zawsze spójne.

Dane wyjściowe zawsze wyglądają tak samo (są normalizowane do formatu kanonicznego).

Format ogólny jest następujący:

```txt
   +420 777 123 456
     |  \_________/
  Prefix     |
      National number
```

Jeśli użytkownik wprowadzi nieprawidłowe dane (lub dane, których nie można automatycznie skorygować), zostanie zgłoszony wyjątek.

Wyłapywanie błędów
----------------

Jeśli liczba nie może być bezpiecznie znormalizowana do postaci bazowej lub nie istnieje, rzuca wyjątek `InvalidArgumentException`.

Jeśli chcesz przekonwertować wyjątek na wartość logiczną, użyj wbudowanego walidatora zasobów:

```php
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('123'); // fałsz
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('777123456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777123456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 777 123 456'); // true
\Baraja\PhoneNumber\PhoneNumberValidator::isValid('+420 77 712 34 56'); // true
```
