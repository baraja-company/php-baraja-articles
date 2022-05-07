Operatory trójdzielne w PHP (?:) - warunek w jednej linii
=========================================================

> id: '1191be9b-ff9d-4725-b0b4-17b60de2b935'
> slug:
> 	cs: ternarni-operator
> 	pl: operatory-trojdzielne-w-php---warunek-w-jednej-linii
> 
> perex:
> 	- Ternární operátor umožňuje zapsat jednoduchou podmínku do jednoho řádku jako výraz.
> 	- Operator trójskładnikowy umożliwia zapisanie prostego warunku w jednym wierszu jako wyrażenia.
> 
> publicationDate: '2019-11-26 11:59:18'
> mainCategoryId: f46a0d80-fbe4-4be8-a5e4-04a8d29b0afc
> sourceContentHash: '491dbbbaeceba356a030e2501f5c0e2b'

Operator trójskładnikowy pozwala skrócić prosty warunek do jednego wiersza w miejscu, w którym parsowanie jest niepotrzebne, skomplikowane lub wręcz niewłaściwe.

TL;DR
------

Operatory trójskładnikowe są skrótem dla warunków `jeśli` i `jeśli`, więc nie trzeba ich używać. Są one przydatne w przypadku stale powtarzających się fragmentów logiki weryfikacyjnej. Zawsze używaj formatu `(warunek ? logika pozytywna : logika negatywna)` z nawiasami. Użyj do krótkiej weryfikacji, aby kod był bardziej przejrzysty i krótszy.

Podstawowe definicje
------------------

Często mamy do czynienia z warunkiem w postaci np:

```php
$a = 5;
$b = 3;

if ($a > $b) {
     echo 'Jest większy';
} else {
     echo 'Jest mniejszy';
}
```

Jeśli więc chcemy napisać tylko jedno proste zdanie, musimy użyć 4 wierszy kodu, które można by zredukować do jednego.

```php
$a = 5;
$b = 3;

echo 'Jest to' . ($a > $b ? 'większa' : 'mniejszy');
```

Ogólnie rzecz biorąc, operator trójskładnikowy zapisuje się z trzech części (stąd jego nazwa "trójskładnikowy"):

```php
(podmínka ? pokud je pravda : pokud není pravda)
```

Operatory trójdzielne są bardzo często używane w praktyce, na przykład do oznaczania parzystych wierszy w tabeli:

```php
$pole = [3, 1, 4, 1, 5, 9, 2];

for ($i = 0; $pole[$i]; $i++) {
     echo '<tr class=""' . ($i % 2 ? 'z' : 'nawet') . '">';

          // To jest w jakiś sposób praca z arkuszem kalkulacyjnym...
          // na przykład: echo '<td>' . $field[$i] . '</td>';

     echo '</tr>.';
}
```

Przykład użycia operatora trójskładnikowego
------------------------------------

Często trzeba sprawdzić istnienie wartości zmiennej i w razie potrzeby natychmiast jej użyć. Jeśli nie istnieje, chcemy zwrócić wartość domyślną.

Klasyczne podejście polega na wykonaniu tej czynności w następujący sposób:

```php
$a = 5;
$b = 8;

$c = $a ? $a : $b;
```

Jeśli zmienna `$a` istnieje, jako wartości użyj `$a`, w przeciwnym razie `$b`.

Czasami jednak wartość jest pobierana z funkcji:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ? funkce($a, $b) : $default;
```

Ta metoda wywoływania jest bardzo nieefektywna pod względem wykorzystania zasobów systemowych. Najpierw musi zostać wywołana funkcja, a jeśli istnieje, to jest wywoływana ponownie, aby uzyskać wartość, która jest przechowywana w zmiennej `$c`.

Można to lepiej rozwiązać za pomocą zmiennej pomocniczej:

```php
$a = 5;
$b = 3;
$pomocnaPromenna = funkce($a, $b);
$default = 42;

$c = $pomocnaPromenna ? $pomocnaPromenna : $default;
```

Niewłaściwe użycie
------------------

Nie zawsze warto używać operatora trójdzielnego, ponieważ może on powodować zamieszanie podczas pisania skomplikowanych i zagnieżdżonych warunków.

Zobaczcie sami przykład:

```php
$valid = true;
$lang = 'Francuski';

$x = $valid
     ? ($lang === 'Francuski' ? 'oui' : 'tak')
     : ($lang === 'Francuski' ? 'nie' : 'z');
```

Czy od razu wiedziałbyś, że zmienna `$x` będzie zawierała wartość `oui`?

Po odrobinie wprawy można, ale to nie jest właściwa odpowiedź. :)

Sprawdzanie (nie)istnienia wartości i stosowanie wartości domyślnej
--------------------

Operatory trójskładnikowe są najbardziej wydajne w rutynowej weryfikacji (nie)istnienia wartości i stosowania innych wartości domyślnych.

Na przykład chcemy sprawdzić, czy dla danego artykułu istnieje kategoria główna, a jeśli nie, wyświetlić komunikat zastępczy:

```php
echo $mainCategory ?? 'Kategoria nie istnieje';
```

Operator `??` (dwa znaki zapytania) sprawdza, czy zmienna `$mainCategory` istnieje i nie jest `null`. Działa ona w taki sam sposób jak funkcja `isset()`.

Walidacja pustej wartości
-----------------------------

Bardzo często przydatna konstrukcja do sprawdzania, czy wartość wyjściowa nie jest pusta (tzn. nie jest `null`, `0`, `false` lub `''*(pusty łańcuch)*).

Można to zapisać w następujący sposób:

```php
$a = 5;
$b = 3;
$default = 42;

$c = funkce($a, $b) ?: $default;
```

Najpierw wywoływana jest `funkcja($a, $b)`, następnie testowana jest jej wartość i jeśli nie jest pusta, jest natychmiast przekazywana do zmiennej `$c`, w przeciwnym razie używana jest zmienna `$default`.

Operator `?:` działa jak operator `!=` w warunku (fałsz niezależnie od typu danych) i można go zapamiętać, wyglądając na przykład jak uśmiechnięta buźka z włosami Elvisa.

Wsparcie dla starszych wersji PHP
----------------------------

Operator `?:` działa od PHP 7. W starszych wersjach musimy zadowolić się warunkiem `if (...)`, który pozwala osiągnąć to samo zachowanie. Pamiętaj, że operatory trójdzielne są tak naprawdę skrótem do pisania tego samego, co jest obsługiwane przez warunki.

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

> **Ostrzeżenie:** Kolejność `isset()` i samej zmiennej ma znaczenie. Gdybyśmy odwrócili kolejność, a zmienna nie istniała, zostałby zgłoszony błąd dostępu do nieistniejącej zmiennej.
