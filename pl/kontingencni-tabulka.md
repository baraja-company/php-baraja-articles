Tabela awaryjna w PHP
=====================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	pl: tabela-awaryjna-w-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

Tablicę kontyngencji stosuje się na ogół w celu pokazania związku między dwoma zjawiskami statystycznymi. Podczas tworzenia aplikacji internetowej często zachodzi potrzeba zwizualizowania zależności między pewnym zjawiskiem w bazie danych a sekwencją czasu, zazwyczaj w administracji.

Na przykład mamy tabelę zamówień zawierającą poszczególne produkty i interesuje nas, w jaki sposób sprzedaż niektórych produktów o dużym wolumenie jest powiązana z czasem.

Przydałaby się do tego poniższa tabela:

| Daktyle | Jabłka | Truskawki | Gruszki |
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

Nie ma prostego sposobu na przygotowanie danych do tego formularza w PHP, a pobieranie ich do tego formularza bezpośrednio w SQL również nie jest eleganckie, ponieważ musimy uwzględnić dynamiczną liczbę kolumn.

Dlatego musimy być sprytni przy projektowaniu danych wyjściowych tej struktury danych.

Serializacja danych przy użyciu kluczy
----------------------------

Podczas tworzenia tabeli często korzystam z funkcji pobierania wszystkich rekordów spełniających dany warunek bezpośrednio z bazy danych, na przykład danych interwałowych.

Konkretnie:

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

Zapytanie pobiera wszystkie kolumny z tabeli zamówień (`order`), filtrując wszystkie rekordy od początku wieku do ``2019-05-01``, zwracając je posortowane od najnowszych do najstarszych.

Za pomocą prostego zapytania SQL uzyskujemy dane niemal natychmiast. Drugą zaletą jest możliwość efektywnego wykorzystania indeksów baz danych do kompilacji wyników. Ponieważ jednak mamy dane w postaci zwykłej tablicy, musimy je ręcznie serializować do struktury danych, którą można przekształcić w tablicę kontigową.

Ponieważ tabela kontyngencji opisuje związek dwóch lub więcej czynników, sensowne jest użycie klucza wielowymiarowego. Ponieważ jednak niektóre dane mogą nie istnieć dla wszystkich kombinacji, lepiej jest serializować klucz do pojedynczego łańcucha i przechowywać dane jako płaską tablicę.

Dane można zebrać w jednym przebiegu pętli (zmienna `$selection` zawiera dane wyjściowe z bazy danych):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Data rok-miesiąc
    foreach ($row->items as $product) { // przeglądamy produkty
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // istnieje, dodamy kolejny produkt
        } else {
            $data[$key] = 1; // nie istnieje, zaczniemy od pierwszego produktu
        }
    }
}
```

Gdybyśmy badali prostszą strukturę danych, pętla wewnętrzna nie byłaby potrzebna do przeglądania produktów. W takim przypadku cały proces tworzenia danych można rozwiązać w jednym cyklu.

Dzięki takiemu podejściu uzyskujemy tzw. płaską tablicę wartości, która wygląda jak tablica `klucz: wartość', przechowująca informacje dwuwymiarowe.

Wyjściem jest na przykład (w `maju 2019` produkt o identyfikatorze `10` sprzedał się w liczbie `6` sztuk):

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Renderowanie danych do tabeli - szablony
-------------------------------

Jeśli mamy dane w postaci tablicy płaskiej, możemy bardzo łatwo wyrenderować całą tabelę. Aby to zrobić, wystarczy znać pola wszystkich interesujących nas produktów oraz pola wszystkich dat, dla których chcemy wykreślić tabelę.

```php
$products = [ ... ]; // pole produktu: id => nazwa
$dates = [ ... ]; // według daty: data => etykieta

echo '<table>.';
foreach ($products as $productId => $productName) {
    echo '<tr>.';
    foreach ($dates as $date => $dateLabel) {
        echo '<td>.';
        echo htmlspecialchars(
            (string) ($data[$date . '_' . $productId] ?? '0')
        );
        echo '<td>.';
    }
    echo '</tr>.';
}
echo '</table>.';
```

Należy pamiętać, że podczas przeglądania danych wyszukiwane jest konkretne wystąpienie na podstawie złożenia kluczy łańcuchowych. Dzięki takiemu podejściu możemy dowolnie ograniczać lub rozszerzać renderowaną tabelę w zależności od tego, jakie dane przeglądamy. Jeśli dane nie istnieją, obliczany jest operator trójskładnikowy `??` i wyświetlane jest zero.

Tablicę dostępnych produktów i dat możemy utworzyć w ramach pierwszego cyklu przygotowującego dane. Wówczas będziemy mieć pewność, że wykreślamy tylko te dane, które rzeczywiście istnieją. W tym przypadku bardzo ważne jest, aby dane wyjściowe z bazy danych SQL były posortowane według daty utworzenia, ponieważ w przeciwnym razie wiersze mogą zostać przemieszane podczas ostatecznego renderowania tabeli.
