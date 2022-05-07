Dodajcslashes
=============

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	pl: dodajcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Obsługa PHP4, PHP5

`addcslashes` - ciąg ukośników w stylu C

Opis
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Zwraca łańcuch znaków z backslashem przed znakami określonymi w parametrze charlist.

Parametry
--------------------------

**str** Ciąg tekstowy

**charlist**

znaki, które mają zostać usunięte. Jeśli lista znaków zawiera znaki `n`, `r` i inne, to są one konwertowane na styl C. Inne niealfanumeryczne znaki ASCI o długości mniejszej niż 32 i większej niż 126 zmieniają się.

Podczas definiowania sekwencji znaków w argumencie charlist należy upewnić się, że wiadomo, jakie znaki należy umieścić na początku i na końcu zakresu.

```php
echo addcslashes('foo[ ]', 'A..z');

// Wartości: \\\\\\
// Usuwa wszystkie małe i wielkie litery
```

Wartości zwracane
--------------------------

Zwraca zmodyfikowany ciąg znaków.

Przykład
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `0..37!@177..77`, usuwa wszystkie znaki o kodzie ASCII między 0 a 31.
