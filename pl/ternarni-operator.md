Operatory trójdzielne w PHP (?:) - warunek w jednej linii
=========================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	pl: operatory-trojdzielne-w-php-warunek-w-jednej-linii
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- Operator trójdzielny pozwala na zapisanie prostego warunku w jednej linii jako wyrażenie.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '20ff690401289869eae2a2584fae7568'

Operator trójskładnikowy pozwala na skrócenie prostego warunku do jednej linii w miejscu, gdzie parsowanie jest niepotrzebne, skomplikowane lub wręcz niewłaściwe.

TL;DR
------

Operatory trójskładnikowe są skrótem dla warunków `if` i `else`, więc nie musisz ich używać. Są one przydatne do wiecznie powtarzających się fragmentów logiki weryfikacyjnej. Zawsze używaj formatu `(warunek ? logika pozytywna : logika negatywna)` łącznie z nawiasami. Użyj do krótkiej weryfikacji, aby kod był bardziej przejrzysty i krótszy.

Podstawowe definicje
------------------

Często mamy warunek w postaci np:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'To jest większe';
} else {
     echo 'Jest mniejszy';
}
```

Jeśli więc chcemy napisać tylko jedno proste zdanie, musimy użyć 4 linii kodu, które można by zredukować do jednej.

```php
$a = 5;
$b = 3;

echo 'To jest' . ($a > $b ? 'większa' : 'mniejszy');
```

Generalnie operator trójskładnikowy zapisuje się za pomocą 3 części (dlatego nazywa się "trójskładnikowy"):

```php
(condition ? 'tak' : 'z')
```

Operatory trójdzielne są bardzo często używane w praktyce, na przykład do oznaczania parzystych wierszy w tabeli:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'z' : 'nawet') . '">';

          // To jest w jakiś sposób praca z arkuszem kalkulacyjnym...
          // na przykład: echo '<td>' . $field[$i] . '</td>';

     echo '</tr>';
}
```

Przykład użycia operatora trójdzielnego
------------------------------------

Dość często musimy sprawdzić istnienie wartości zmiennej i w razie potrzeby natychmiast jej użyć. Jeśli nie istnieje, chcemy zwrócić wartość domyślną.

Klasycznym podejściem jest zrobienie tego w ten sposób:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Jeśli zmienna `$a` istnieje, użyj `$a` jako wartości, w przeciwnym razie `$b`.

Czasami jednak otrzymujemy wartość z funkcji:

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ? my_function($a, $b) : $default;
```

Ten sposób wywoływania jest bardzo nieefektywny pod względem zasobów systemowych. Najpierw należy wywołać funkcję, a jeśli istnieje, to wywołuje się ją ponownie, aby uzyskać wartość, która jest przechowywana w zmiennej `$c`.

To może być lepiej obsługiwane przez zmienną pomocniczą:

```php
$a = 5;
$b = 3;
$helper = my_function($a, $b);
$default = 42;

$c = $helper ? $helper : $default;
```

Niewłaściwe użycie
------------------

Nie zawsze warto używać operatora trójdzielnego, ponieważ ma on tendencję do powodowania zamieszania podczas pisania skomplikowanych i zagnieżdżonych warunków.

Zobaczcie sami przykład:

```php
$valid = true;
$lang = 'Francuski';

$x = $valid
     ? ($lang === 'Francuski' ? 'oui' : 'tak')
     : ($lang === 'Francuski' ? 'nie' : 'z');
```

Czy na pierwszy rzut oka wiedziałbyś, że zmienna `$x` będzie zawierała wartość `oui`?

Po odrobinie praktyki możesz, ale to nie jest właściwa odpowiedź. :)

Sprawdzenie (nie)istnienia wartości i użycie domyślnej
--------------------

Operatory trójskładnikowe są najpotężniejsze w rutynowym sprawdzaniu (nie)istnienia wartości i używaniu innych domyślnych wartości.

Na przykład, chcemy sprawdzić, czy istnieje główna kategoria dla artykułu i jeśli nie, wyprowadzić komunikat zastępczy:

```php
echo $mainCategory ?? 'Kategoria nie istnieje';
```

Operator `??` (dwa znaki zapytania) sprawdza czy zmienna `$mainCategory` istnieje i nie jest `null`. Działa ona w taki sam sposób jak funkcja `isset()`.

Walidacja pustki wartości
-----------------------------

Bardzo często przydatna konstrukcja do sprawdzania, czy wartość wyjściowa nie jest pusta (tzn. nie jest `null`, `0`, `false` lub `''*(empty string)*).

Można to zapisać w następujący sposób:

```php
$a = 5;
$b = 3;
$default = 42;

$c = my_function($a, $b) ?: $default;
```

Najpierw wywoływana jest `funkcja($a, $b)`, następnie testowana jest jej wartość i jeśli nie jest pusta, to od razu przekazywana jest do zmiennej `$c`, w przeciwnym razie używana jest zmienna `$default`.

Operator `?:` działa jak operator `!=` w warunkowym (fałsz niezależnie od typu danych), a można go zapamiętać wyglądając np. jak uśmiechnięta buźka z włosami Elvisa.

Wsparcie dla starszych wersji PHP
----------------------------

Operator `?:` działa od PHP 7. W starszych wersjach musimy zadowolić się warunkiem `if (...)`, który pozwala osiągnąć to samo zachowanie. Pamiętaj, że operatory trójskładnikowe są tak naprawdę tylko skrótem do napisania tego samego, co jest obsługiwane przez warunki.

Nieistnienie wartości można sprawdzić za pomocą funkcji `isset()`, która sprawdza, czy zmienna istnieje i nie jest pusta (`null`).

Zamiast kodu:

```php
$a = 5;
$b = 3;

$c = $a ?? $b;
```

Następnie zapisujemy starszą alternatywę:

```php
$a = 5;
$b = 3;

$c = isset($a) && $a ?? $b;
```

> **Ostrzeżenie:** Kolejność `isset()` i samej zmiennej ma znaczenie. Gdybyśmy odwrócili kolejność i zmienna nie istniała, podniosłaby błąd dostępu do nieistniejącej zmiennej.
