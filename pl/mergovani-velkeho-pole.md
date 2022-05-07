Łączenie dużych tablic w PHP
============================

> id: ccfb3f47-c8a0-4d0f-aa29-18d4c1775725
> slug:
> 	cs: mergovani-velkeho-pole
> 	pl: laczenie-duzych-tablic-w-php
> 
> publicationDate: '2020-02-06 09:32:08'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3bfc3b53d26a4c9b675e851a643fc71a'

Często potrzebujemy połączyć wiele tablic razem, można to zrobić w bardzo elegancki sposób za pomocą funkcji `array_merge`:

```php
$userIdsA = [1, 2, 3];
$userIdsB = [5, 6, 7];

// zwraca [1, 2, 3, 5, 6, 7].
$finalIds = array_merge($userIdsA, $userIdsB);
```

Funkcja `array_merge` łączy dwie tablice w jedną dużą tablicę. Jeśli dojdzie do kolizji kluczy, wygrywa wartość tablicy najbardziej wysuniętej na prawo.

Wielokrotne łączenie w pętli
---------------------------

Często jednak otrzymujemy tablicę tablic, która jest tworzona dopiero w pętli (na przykład z bazy danych, a następnie przepuszczana przez foreach), a więc nie znamy z góry liczby łączeń.

Naiwne rozwiązanie może wyglądać następująco:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds = array_merge($finalIds, $user->someIds);
}
```

Takie rozwiązanie jest jednak bardzo nieefektywne z punktu widzenia procesora, ponieważ przy każdej iteracji musimy scalać tablice i iterować nad całą dużą tablicą.

Istnieje jednak proste rozwiązanie polegające na zmodyfikowaniu algorytmu scalania w taki sposób, aby dane były przeglądane tylko raz:

```php
$finalIds = [];

foreach ($users as $user) {
    $finalIds[] = $user->someIds;
}
```

W tym przypadku pole `$finalIds` wygeneruje nieco więcej danych, ale jest to wciąż mniejszy problem niż korzyść związana z oszczędnością czasu.

Samo scalanie zależy od wersji PHP, której używasz, a rozwiązuje się je za pomocą eleganckiego triku:

```php
/* PHP 5.6 i starsze */
$finalIds = call_user_func_array('array_merge', $finalIds + [[]]);

/* PHP 5.6+ i nowsze */
$finalIds = array_merge([], ...$finalIds);

/* PHP 7.4+ i nowsze dla niepustych pól */
$finalIds = array_merge(...$finalIds);
```

W szczególności rozwiązanie `array_merge(...$finalIds)` wygląda bardzo interesująco, ponieważ wykorzystuje nową koncepcję PHP 7, w której można przekazać dynamiczną liczbę argumentów do funkcji, używając znaku trójki na początku. Proces scalania jest wtedy maksymalnie wydajny, a cała logika jest automatycznie obsługiwana wewnętrznie przez PHP.

Skrótowa notacja `array_merge(...$finalIds)` może być używana tylko dla niepustych tablic. Jeśli jest to tablica pusta, do funkcji nie jest przekazywany żaden argument i funkcja wyrzuca błąd `Funkcja array_merge wywołana z 0 parametrami, wymagany co najmniej 1.`.
