End()
=====

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	pl: end
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Obsługa PHP 4, PHP 5
- Krótki opis: ustawia wewnętrzny wskaźnik tablicy na jej ostatni element.
- Wymagania.

Opis
--------------------------

Funkcja `end()` ustawia wewnętrzny wskaźnik tablicy na ostatni element i zwraca jego wartość.

Podobne funkcje
--------------------------

- `current()`.
- `each()`
- `prev()`.
- <a href="/reset">reset()</a>.
- `next()`.

Przykład
--------------------------

```php
echo end([
    'jabłko',
    'banan',
    'żurawina',
]); // wypisuje żurawinę
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // drukuje 0
```

Parametry
--------------------------

| # | Typ | Opis |
| --- | ------- | ----- |
| 1 | `array` | Nazwa tablicy, z którą chcemy pracować. |

Wartości zwracane
--------------------------

Zwraca wartość ostatniego elementu lub `false` dla pustej tablicy.

Różnice w stosunku do wcześniejszych wersji
--------------------------

Brak
