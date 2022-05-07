Warunki w PHP - IF() {...} - opcje rozgałęziania
================================================

> id: '3e4b81bb-a8bb-4e99-8842-e76f1658a371'
> slug:
> 	cs: if
> 	pl: warunki-w-php---if-...---opcje-rozgaleziania
> 
> perex:
> 	- 'Podmínky, logické výrazy, booleovská logika a možnosti větvení PHP scriptů. Vyhodnocování logických výrazů a operátory. Možnosti skládání výrazu.'
> 	- 'Warunki, logika Boolean, logika Boolean i opcje rozgałęziania skryptu PHP. Ocenianie wyrażeń i operatorów boole''owskich. Opcje składania wyrazów.'
> 
> publicationDate: '2019-11-26 11:55:09'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '759a244b4fe2dc119c475aee42259578'

> **Uwaga:** Ten artykuł może być nieco zawiły dla niektórych początkujących, ponieważ zakłada podstawową znajomość PHP. Jeśli interesuje Cię, jak działają warunki, przeczytaj <a href="/warunki">o warunkach w kursie dla początkujących</a>.

| Obsługa: | Wszystkie wersje: PHP 4, PHP 5, PHP 7
|-------------------|----------|
| Walidacja jednego lub więcej oświadczeń
| Typ: <a href="/oświadczenia-i-funkcje">Poświadczenie, konstrukt</a> (nie funkcja)

Opis
-----

Często trzeba określić, czy zachodzi równość lub czy dane stwierdzenie jest prawdziwe - do tego służą warunki. PHP używa następującej składni, jak wiele innych języków (zwłaszcza C):

```php
if (logický výrok) {
    // construct
}
```

Każde wyrażenie logiczne ma wartość `TRUE` (prawda) lub `FALSE` (fałsz), nie ma innych opcji.

Przykład porównywania, czy zmienna `$x` jest większa od zmiennej `$y`:

```php
$x = 10;
$y = 5;

if ($x > $y) {
    // Ta część skryptu zostanie uruchomiona, jeśli warunek jest prawdziwy
} else {
    // Ta część skryptu zostanie uruchomiona, jeśli warunek nie zostanie spełniony
}
```

Konstrukcja warunku ma obowiązkową treść w nawiasach okrągłych, w której podane jest testowane wyrażenie, składające się z operatorów (przegląd poniżej), wiele wyrażeń można łączyć za pomocą operatorów logicznych (przegląd poniżej).

Ponadto warunek zawiera dwa opcjonalne bloki instrukcji.

- Jeśli warunek jest spełniony
- Jeśli warunek nie jest spełniony

Ze względów praktycznych należy zawsze uwzględniać przynajmniej pierwszy blok instrukcji, gdy warunek jest prawdziwy, gdyż w przeciwnym razie testowanie wyrażenia nie miałoby sensu.

Użycie średników i nawiasów
--------------------------

Ogólnie rzecz biorąc:
- **Nawiasy okrągłe** służą do oddzielania wyrażeń logicznych (aby uzyskać bardziej złożone wyrażenia, można użyć innych nawiasów okrągłych).
- Nawias **złożony** jest używany do oddzielania bloku poleceń i funkcji.
- Środek** nie jest używany do wskazywania warunku (blok poleceń jest ograniczony nawiasem złożonym), lecz do oddzielania poszczególnych poleceń w ramach warunku).

Jedyny możliwy zapis ze średnikiem (poza przypadkami użycia konstrukcji **endif**):

```php
if ($x > $y);
```

Taki warunek jest jednak bezsensowny, ponieważ w obu przypadkach wynik porównania zostanie odrzucony i nie zostanie wykonana żadna instrukcja należąca do warunku.

Alternatywna notacja
--------------------------

W pewnych okolicznościach konstrukcja `jeśli` może być użyta z pominięciem nawiasów złożonych. Można to osiągnąć tylko w następujących przypadkach:

- Jeśli w warunku wykonamy tylko jedno polecenie.
- Jeśli zamiast nawiasów złożonych użyjemy dwukropka i endif;
- Jeśli użyjemy notacji "in-line".

Bardziej szczegółowe informacje znajdują się w kolejnych 3 rozdziałach.

> **`1. tylko jedno polecenie ~ skrócona składnia`**.

Jeśli tworzysz warunek, w którym chcesz wykonać tylko jedną konstrukcję (instrukcję), możesz użyć klasycznej notacji nawiasów złożonych:

```php
if ($x > 10) { $y = $x; }
```

Możemy też pominąć nawiasy:

```php
if ($x > 10) $y = $x;
```

Zachowanie to dotyczy jednak tylko jednego polecenia w bezpośrednim sąsiedztwie warunku.

Lepszy przykład (tylko konstrukcja `$y = $x` jest wykonywana warunkowo, reszta jest wykonywana zawsze):

```php
$x = 5;
$y = 3;
$z = 10;
if ($x > $y)
$y = $x;
$x = 3;
```

> **`2. dwukropek i endif;`**

```php
if (výraz):
    konstrukt;
    konstrukt;
    konstrukt;
endif;
```

Jednak zapis ten od dawna uważa się za przestarzały, ponieważ zmniejsza on orientację, gdy wiele warunków jest zanurzonych w sobie.

> **Uwaga:** Chciałbym zauważyć, że ten styl jest również lubiany przez niektórych ludzi, takich jak Yuh (<a href="https://www.jakpsatweb.cz/php/php-tahak.html#vetveni">zobacz jego artykuł</a>). Boże broń, żebyś tego gdzieś nie użył.

> **`3. Wyrażenie trójskładnikowe ~ notacja jednoliniowa "in-line"`**

Niekiedy warto wykonać proste porównanie w linii z inną czynnością (na przykład wraz z definiowaniem nowej zmiennej). Jeśli chcemy wykonać tylko jedną instrukcję, całą procedurę można zredukować do jednego wiersza, zachowując jej prostotę.

```php
$x = 5;
$xJeVetsiNezDva = ($x > 2 ? TRUE : FALSE);

// lub jeszcze krócej:

$xJeVetsiNezDva = ($x > 2);
```

Stoły operatorskie
-----------------

W ramach warunku stosowane są dwa rodzaje operatorów:

- **Porównawcze** ~ porównują konkretną relację,
- **Logiczne** ~ łączenie wielu wyrażeń w celu tworzenia złożonych warunków.

Operatorzy porównawczy
-----------------------

| Operator | Znaczenie |
|----------|----------|
| `=` | Równa się
| `==` | równa się i ma ten sam typ danych
| `!=` | Nie jest równy
| `>=` | równa się lub jest większa
| `<=` | równa się lub jest mniejsza
| `>` | Większy
| | Mniej

Przykład (ważny, gdy `$x nie jest 5`):

```php
if ($x != 5) { ... }
```

Operatory logiczne
--------------------

| Operator | Alternatywa | Znaczenie | Prawda, gdy:
|-----------|--------------|----------------|--------------------------------------------
| i w tym samym czasie | obie wartości są prawdziwe
| | OR | lub | co najmniej jedna wartość jest prawdziwa
| XOR | wyłączne OR | przynajmniej jedna z nich jest prawdziwa lub fałszywa, ale nigdy obie
| `!` | *doesn't* | negacja wyrażenia | `true` gdy `false` i odwrotnie
| `()` | *nie* | negacja wyrażenia| zależy od okoliczności

Bardziej złożony przykład:

```php
$x = 5;
$y = 3;
$z = 8;
if ($x > 0 && !($y != 2 && $z == $x) || $z > $y) { ... }
```

Pomijanie operatorów logicznych i porównawczych
---------------------------------------------

Często możemy sobie pozwolić na pominięcie któregoś z operatorów (lub nawet obu), ale nigdy nie wolno nam zapominać o zasadach prawidłowego stosowania, aby otrzymane wyrażenie działało.

Ogólnie rzecz biorąc, testując wyrażenie bez operatora, sprawdzamy, czy jego wartość jest `TRUE` lub niepusta (np. czy zawiera niezerową liczbę, niepusty łańcuch znaków, ...).

Przykłady:

```php
$x = 5;
$y = 3;
$z = 8;

if ($x) { ... }         // przechodzi, ponieważ $x nie jest pusty
if ($x && $y) { ... }   // przechodzi, ponieważ $x i $y nie są puste
if (!$x) { ... }        // nie powiedzie się, ponieważ TRUE jest zanegowane
if (isset($z)) { ... }  // przechodzi, ponieważ zmienna $z istnieje
```

Mogą jednak pojawić się trudne sytuacje, zwłaszcza gdy:
- Pytam o `jeśli ($x)`, a zmienna `$x` zawiera zero (`0`), to warunek nie jest spełniony.
- Albo zmienna `$x` zawiera łańcuch ``0`` (liczbę zero), ponieważ przepełnia się do zera, a zatem wyrażenie nie jest prawdziwe.
- W warunku znajduje się funkcja, która zawsze zwraca jakiś niepusty ciąg znaków, ponieważ wtedy warunek jest zawsze prawdziwy.
- Lub jeśli sprawdzamy dane wejściowe od użytkownika, a on zwraca `'false'` jako łańcuch, to ponownie warunek jest prawdziwy, ponieważ łańcuch jest niepusty.

Polecam jedno proste i skuteczne rozwiązanie tego problemu - poproś o podanie liczby zwracanych znaków. Jeśli łańcuch jest pusty (lub zmienna nie istnieje), to zwracane jest zero znaków i warunek nie jest spełniony. Prosty przykład:

```php
$x = '0';
if ($x) { ... }			// warunek zwykle nie ma zastosowania
if (strlen($x)) { ... }	// warunek jest ważny, ponieważ $x zawiera 1 znak
```

Następnie możemy sprawdzić, czy zmienna istnieje, używając funkcji `isset()`.

Porównanie ciągów znaków
-----------------

Stwierdzenie, że struny są identyczne, jest łatwe:

```php
$a = 'Kot';
$b = 'cat';

if ($a === $b) {
    // Jeśli ciągi są takie same
} else {
    // Jeśli ciągi są różne
}
```

Należy zwracać uwagę na typy danych, aby nie dopuścić do sytuacji, w której dany wpis będzie równoważny innemu.

Na przykład pusty łańcuch `$a = '';` jest różny od łańcucha `NULL`: `$b = NULL;`. Rozróżnienie to jest konieczne na przykład ze względu na bazy danych, w których istnieje różnica między wartością nieistniejącą a pustą.

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

Dobrym pomysłem jest również ignorowanie białych (niewidocznych) znaków, takich jak spacje, tabulatory i podziały wierszy podczas porównywania łańcuchów. Jest to przydatne na przykład podczas wprowadzania hasła i przekazywania go do funkcji haszującej:

```php
$password = '81dc9bdb52d04dc20036dbd8313ed055'; // 1234
$userPassword = '1234';

if (md5(trim($userPassword)) === $password) {
    // Funkcja trim() automatycznie usuwa spacje.
}
```

Nieznana (nieistniejąca) wartość?
--------------------------

Czasami może się zdarzyć, że wartość nie istnieje (nie jest ani `TRUE` ani `FALSE`), jest to głównie wartość uzyskana z bazy danych (na przykład pytamy o kolumnę, która nie istnieje), w tym przypadku zostanie zwrócony typ danych `NULL`.

Ogólnie rzecz biorąc, `NULL` jest obliczane jako `FALSE`, tzn. warunek nie ma zastosowania. Jednak takie zachowanie nie zawsze jest wygodne, ponieważ nieistniejąca wartość nie musi oznaczać, że nie ma żadnego rekordu.

> Przykład z praktyki: Mamy profil użytkownika i zadajemy mu pytania dotyczące jego strony internetowej. Nie wszyscy użytkownicy muszą mieć stronę WWW, więc w tym przypadku zwracany jest `NULL`, ale użytkownik nadal istnieje. Dlatego w tym przypadku powinniśmy raczej użyć funkcji `isset()`, aby sprawdzić (nie)istnienie zmiennej, a nie wyciągać wnioski na podstawie konkretnej wartości.
