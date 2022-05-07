Warunki i rozgałęzienia
=======================

> id: '2cea5541-6879-4763-a518-cb21bf9021dd'
> slug:
> 	cs: podminky
> 	pl: warunki-i-rozgalezienia
> 
> publicationDate: '2019-09-07 20:25:57'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: bc4140fc49413be3f2f2b2264b8ea5e6

> **Ostrzeżenie:** Ten artykuł został napisany wiele lat temu i niektóre informacje mogą być nieaktualne lub nieprawidłowe. Należy o tym pamiętać podczas lektury.

Koniec z programami liniowymi! Najbardziej podstawową zasadą każdego programu jest "co się dzieje, gdy....". Warunek można zapisać jako zdanie logiczne, które może być prawdziwe (warunek jest spełniony) lub fałszywe (warunek nie jest wykonywany lub jest wykonywany jego przeciwieństwo). Oba te pojęcia są łatwe do zdefiniowania.

Notacja ogólna
------------

Ogólnie rzecz biorąc, warunek można zapisać w postaci wyrażenia logicznego. Warunek ten może, ale nie musi być spełniony. Dobrym pomysłem jest uwzględnienie obu opcji. Jeśli istnieje wiele alternatyw, jest to **warunek zagnieżdżony**.

Przykład:

```php
if (hodnota   operace   hodnota) {
	// Zostanie to uruchomione, jeśli warunek zostanie spełniony
} else {
	// Jest to wyzwalane, jeśli warunek nie ma zastosowania.
}
```

Nie zawsze musimy definiować obie opcje (czasami jest to zupełnie niepotrzebne). W rzeczywistości możemy określić sytuację, jeśli tylko spełniony jest warunek. Odbywa się to w następujący sposób:

```php
if (hodnota   operace   hodnota) {
	// Zostanie to uruchomione, jeśli warunek zostanie spełniony
}
```

Operatory logiczne
--------------------------

| Operator | Znaczenie
|----------|---------
| `=` | Równa się
| `==` | równa się i ma ten sam typ danych (*cokolwiek można porównać do czegokolwiek, ale warunek jest spełniony tylko wtedy, gdy jest to wartość tego samego typu danych (np. liczba, tekst, ...)*)
| `!=` | Nie jest równy
| `<=` | Równe lub większe niż
| `>=` | równa się lub mniejsza niż
| | Większy
| `>` | Mniej

Przykład rzeczywisty
--------------------------

```php
$a = 5;
$b = 3;
if ($a === $b) {
	// blok, który jest drukowany, jeśli $a jest równe $b
} else {
	// blok, który jest drukowany, jeśli $a NIE jest równe $b
}
```

Warunki zagnieżdżone
--------------------------

Niestety, na wyjściu są tylko `true` (prawidłowe) i `false` (nieprawidłowe). Jeśli więc chcemy rozważyć wiele możliwości, musimy zagnieździć wiele warunków jeden w drugim. Jest to tak zwany **warunek zagnieżdżony**. Jest on zagnieżdżony, ponieważ jedno z rozwiązań warunku jest kolejnym warunkiem.

```php
$a = 5;         // lewa kieszeń
$b = 3;         // prawa kieszeń
$kapsa = true;  // Czy mam kieszeń?

if ($kapsa === true) {

	if ($a > $b) {
		echo 'W lewej kieszeni znajduje się więcej';
	} else {
		echo 'W prawej kieszeni znajduje się więcej';
	}

} else {
	echo 'Nie masz kieszeni';
}
```
