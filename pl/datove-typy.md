Typy danych w PHP
=================

> id: cd63818c-259a-484e-b006-86c35b362019
> slug:
> 	cs: datove-typy
> 	pl: typy-danych-w-php
> 
> perex:
> 	- 'Datové typy v PHP: String, integer, float, boolean, pole (array), objekt a další.'
> 	- 'Typy danych w PHP: string, integer, float, boolean, array, object i inne.'
> 
> publicationDate: '2019-08-23 15:44:28'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: fcb2c647973c5f94e601c789b0a882f1

Wszystkie dane przetwarzane w PHP są określonego typu. Na przykład liczba całkowita, ciąg znaków lub boolean (prawda/fałsz).

Podstawowe typy danych
--------------------

Typy podstawowe są również nazywane **prymitywnymi typami danych** lub <a href="/function-is-scalar">typami skalarnymi</a>.

| Typ | Nazwa | Opis |
|---------|-----------------|-------|
|int` | Integer | (`integer`) Zawiera tylko liczby całkowite. Maksymalna wartość jest określana przez liczbę bitów. W 32-bitowym PHP zakres ten wynosi od `-2 147 483 648` do `-2 147 483 647` (~ ± 2 miliardy), w 64-bitowym PHP zakres ten wynosi od `-9 223 372 036 854 775 808` do `-9 223 372 036 854 775 807` (~ ± 9 kwintylionów). Wartość maksymalną można zawsze uzyskać przez wywołanie stałej `PHP_INT_MAX`. Jeśli maksymalna wartość liczby całkowitej zostanie przekroczona, PHP przedstawi tę liczbę jako `float` i automatycznie ją nadpisze.
| Float` | Floating Point Decimal | Jest to odmiana liczby zmiennoprzecinkowej, dla której obowiązuje zasada *"im mniejsza, tym dokładniejsza "*. Liczba jest przechowywana wewnętrznie jako tak zwana **mantysa** i **eksponent**, więc w rzeczywistości przechowywane są dwie liczby, między którymi wykonywana jest operacja `mantysa * (2^eksponent)`, co umożliwia przechowywanie naprawdę ogromnego zakresu liczb. Wykorzystuje to zasadę, że w przypadku dużych liczb nie zawsze musimy znać ich dokładną wartość, ale chcemy zaoszczędzić jak najwięcej pamięci. Liczby typu `float` nie muszą być przechowywane dokładnie i nie powinny być używane do obliczania wartości pieniężnych.
| `string` | String | Zawiera sekwencję znaków, które są ograniczone cudzysłowami lub apostrofami. Maksymalna długość jest ograniczona jedynie pojemnością pamięci operacyjnej. Łańcuch może być zapisany w dowolnym kodowaniu, zawierać emoji lub dane binarne.
| `bool` | Boolean | (`boolean`) Wartość logiczna z algebry booleańskiej, może zawierać tylko `true` lub `false`.
| `null` | wartość pusta | Wartość pusta `null` jest przydatna w przypadkach, gdy chcemy wyrazić, że coś nie istnieje. Na przykład artykuł nie ma kategorii. Czasami `null` jest niepoprawnie zastępowany przez null (`0`) lub pusty łańcuch (`''`), ale nie jest to dobre rozwiązanie dla wyrażenia nieistnienia.

**Ostrzeżenie:** Typ `null` nie jest skalarny.

Zero (0) vs. null
----------------

Wielu programistów ma problem ze zrozumieniem różnicy między `0` (zero) a `null` (wartość nieistniejąca), gdy rozpoczynają pracę nad projektem.

To rozróżnienie można bardzo dobrze i humorystycznie wyjaśnić na poniższym obrazku:

<img src="{$baseUrl}/images/0-vs-null.jpg" alt="0 vs. null" class="w-100 mb-3">

Ręczne przepakowywanie
--------------------

Niektóre typy można konwertować między sobą. Docelowy typ danych jest określany za pomocą nawiasów okrągłych i można go określić w dowolnym miejscu aplikacji, na przykład podczas wyliczania wartości.

Na przykład:

```php
$pi = 3.14;

echo $pi;       // prints 3.14

echo (int) $pi; // drukuje 3
```

Dynamiczne uzupełnianie
---------------------

Rozważmy następujące dwie zmienne:

```php
$x = 10;
$y = '10';
```

Jaka jest różnica między zawartością zmiennych `$x` i `$y`?

Zmienna `$x` jest liczbą, `$y` jest łańcuchem (zawierającym "1" i "0"), więc jeśli tylko przechowamy zmienną w pamięci i nie wykonamy żadnej operacji, która wpłynie na jej wartość. Na przykład dwa poniższe wpisy dadzą ten sam wynik:

```php
echo $x + 5;	// drukuje 15
echo $y + 5;	// drukuje 15
```

W drugim przypadku następuje tak zwane **nadpisanie dynamiczne**, czyli zmienna zmienia swój typ danych tak, aby można było wykonać na niej operację obliczeniową. Nie zawsze można polegać na tym zachowaniu i jest ono raczej zachowaniem korekcyjnym, służącym do poprawiania źle napisanych skryptów dla początkujących. Jeśli to możliwe, zawsze zapisuj liczby z typem danych do ich przechowywania, ponieważ zwiększa to ich precyzję i ułatwia ich wykorzystanie w przyszłości.

> **Uwaga:** Należy pamiętać, że nie możemy konwertować typów danych w sposób całkowicie dowolny, więc nie zawsze jest to możliwe. W przypadku nadpisania typu danych na inny (niekompatybilny) typ, konwersja może w ogóle nie nastąpić, albo oryginalna zawartość może zostać uszkodzona lub całkowicie zniszczona i zastąpiona inną. Na przykład, jeśli łańcuch znaków zostanie przekształcony na liczbę całkowitą (a w zmiennej zostanie zapisany tekst, który nie jest liczbą), w zmiennej zostanie zapisana wartość `1` zamiast wartości liczbowej.

Porównywanie typów
----------------

Porównując wartości, należy brać pod uwagę różne typy danych.

Ogólnie rzecz biorąc, operator `==` jest używany do ogólnego porównywania dwóch wartości (niezależnie od typu), natomiast `===` porównuje zarówno wartości, jak i typ danych.

Na przykład:

```php
$a = '';
$b = null;

if ($a == $b) {
    // Zostanie ocenione jako TRUE, ponieważ
    // konwertowany jest typ danych.
}

if ($a === $b) {
    // Wykonuje znacznie bardziej rygorystyczną walidację
    // i nie przejdzie, ponieważ jest to inny
    // treść i inny typ danych, dlatego
    // ten kod nigdy nie zostanie uruchomiony.
}
```
