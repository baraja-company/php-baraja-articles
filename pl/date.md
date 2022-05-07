Funkcja PHP date(), data i czas
===============================

> id: '9b0ec1c7-3431-4d7d-9bcc-6093285f14f1'
> slug:
> 	cs: date
> 	pl: funkcja-php-date-data-i-czas
> 
> perex:
> 	- 'Zjištění data a času, aktuální datum, formátování data a času a převod tvaru.'
> 	- 'Wykrywanie daty i czasu, bieżąca data, formatowanie daty i czasu oraz konwersja kształtu.'
> 
> publicationDate: '2019-09-11 10:14:16'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: d0323ba88fcdba84ef45439991301f6c

Funkcja `date()` jest narzędziem służącym do pracy z datą i czasem. Stosuje się go w dwóch przypadkach:

- **Znalezienie bieżącego stanu**, tj. bieżącej daty, godziny, ... i wyprowadzanie ich w określonym formacie,
- **Konwersja** określonej daty na inny format (np. numer miesiąca na nazwę, format zapisu roku, system 12- i 24-godzinny, ...).

Przykładowa strona
------

```html
Já jsem speciální stránka. Vím, že právě je
<?php
    echo date('H:i'); // Hodina:minuta
?>
```

> OSTRZEŻENIE: PHP nie drukuje czasu użytkownika, lecz czas na serwerze. W związku z tym może być wyświetlany inny czas niż ustawiony na komputerze.

Składnia danych wejściowych
--------------------------

Funkcja jest wywoływana w normalny sposób, a poszczególne żądania są podawane jako argumenty funkcji.

```php
echo date('znaki formatowania', atribut konkrétního času);
```

Znaki formatowania wskazują, w jakim formacie zostanie wydrukowana data. Znaki mogą zawierać spacje, kropki, dwukropki, myślniki i inne znaki, które same nie są znakami formatowania (jeśli chcesz użyć znaku formatowania, musisz <a href="/carriage-notes">escape it</a>). Poniżej znajduje się przegląd wszystkich znaczników.

Drugi (opcjonalny) atrybut wskazuje ręcznie wprowadzoną datę lub godzinę, która zostanie przekonwertowana i wyprowadzona w formacie zgodnym z pierwszym parametrem. Musi być podany jako *timestamp* (można go uzyskać za pomocą znacznika formatującego "U").

Przykład:

```php
echo date('d. m. Y', 1405856605); // do 20. 07. 2014 r.
```

Tabela dozwolonych znaczników formatowania
--------------------------

| Znak | Opis
|------|---------------------
|Y` | Rok w postaci czterech cyfr (np. 1998)
| `y` | Rok w postaci podwójnej cyfry (np. 98)
| `M` | angielski skrót nazwy miesiąca (np. Jan)
| `m` | Numer miesiąca (01-12)
| F` | angielska nazwa miesiąca (np. styczeń)
| `D` | angielski skrót oznaczający dzień tygodnia (np. piątek)
| `l` | angielska nazwa dnia tygodnia (np. piątek)
| `N` | Numer dnia tygodnia (1 - poniedziałek, 7 - niedziela)
| `w` | Numer dnia tygodnia (0 - niedziela, 1 - poniedziałek, 6 - sobota)
| `d` | Dzień miesiąca (01-31)
| j` | Numer dnia miesiąca (1-31)
| `z` | Dzień roku (001-365)
| `H` | Godzina (00-23)
| `h` | Godzina (01-12)
| i` | Minuta (00-59)
| `s` | Drugi (00-59)
| | *Timestamp:* Liczba sekund od początku czasu (od 1 stycznia 1970 r.)
| S` | angielska końcówka liczby porządkowej dnia miesiąca
| `A` | Wskaźnik AM/PM
| Wskaźnik poranka/popołudnia (am/pm)
| `P` | Różnica w stosunku do czasu Greenwich (GMT) z separatorem między godzinami i minutami (dodano w PHP 5.1.3), na przykład: `+02:00`.
|g` | Godzina w formacie 12-godzinnym (1-12)
| G` | Godzina w formacie 24-godzinnym (0-23)

Formatowanie czasu dla mapy strony
---------------------------------

Bardzo często zachodzi potrzeba sformatowania czasu dla pliku `sitemap.xml`, który zawiera datę i czas ostatniej zmiany w znaczniku `<lastmod>`.

Od wersji PHP 5.1.3 można do tego celu użyć następującej składni:

```php
date('Y-m-dTH:i:sP');
```

Data i godzina zostaną wyświetlone z dokładnością do jednej sekundy i będą zawierać informacje o strefie czasowej, w której znajduje się serwer (strefa czasowa jest określana przez system operacyjny serwera).

Uzyskiwanie czeskich nazw dni i miesięcy
----------------------------------

W PHP nie można uzyskać czeskich nazw dni i miesięcy w normalny sposób, dlatego musimy sami wpisać takie wartości. Najlepszym sposobem jest przechowywanie wpisów w tablicy i pobieranie ich za pomocą wywołania indeksu.

```php
$mesice = [
    1 => 'styczeń', 'Luty', 'Marzec', 'Kwiecień', 'Maj',
    'Czerwiec', 'Lipiec', 'Sierpień', 'Wrzesień', 'październik',
    'Listopad', 'Grudzień'
];

$dny = ['Niedziela', 'Poniedziałek', 'Wtorek', 'Środa', 'Czwartek', 'Piątek', 'Sobota'];
```

Ten przykład jest uproszczonym przykładem z artykułu <a href="https://php.vrana.cz">**Jakub Vrana**</a> z artykułu <a href="https://php.vrana.cz/ceske-nazvy-mesicu-a-dnu-v-tydnu.php">Czeskie nazwy miesięcy i dni tygodnia</a>.

Tłumaczenie dat z języka angielskiego na język angielski
--------------------------------------

Często mamy datę w formacie `Piątek, 13 września` i chcemy ją przetłumaczyć na język angielski, ale jak to zrobić? Najlepszym sposobem jest wywołanie funkcji, na przykład `datumcesky()`, do której przekażemy angielską datę, a ona ją przetłumaczy.

```php
function datumCesky(string $date): string
{
    $men = [
        'styczeń', 'Luty', 'Marzec', 'Kwiecień', 'Maj',
        'Czerwiec', 'Lipiec', 'Sierpień', 'Wrzesień', 'październik',
        'Listopad', 'Grudzień'
    ];

    $mcz = [
        'styczeń', 'Luty', 'Marzec', 'Kwiecień', 'Maj',
        'Czerwiec', 'Lipiec', 'Sierpień', 'Wrzesień', 'październik',
        'Listopad', 'Grudzień'
    ];

    $date = str_replace($men, $mcz, $date);

    $den = [
        'Poniedziałek', 'Wtorek', 'Środa', 'Czwartek',
        'Piątek', 'Sobota', 'Niedziela'
    ];

    $dcz = [
        'Poniedziałek', 'Wtorek', 'Środa', 'Czwartek',
        'Piątek', 'Sobota', 'Niedziela'
    ];

    return str_replace($den, $dcz, $date);
}
```

Przykład zastosowania:

```php
echo datumCesky('Piątek, 13 września'); // Piątek, 13 grudnia
```

Funkcja ta zdecydowanie nie jest idealna do tłumaczenia, ponieważ zastępuje jedynie słowa angielskie słowami czeskimi, ale może być wystarczająca w przypadku wielu wdrożeń. W przypadku bardziej zaawansowanych tłumaczeń należy zawsze zagwarantować dokładną składnię tłumaczenia w jednolitym stylu, np. `Piątek, 13 grudnia`.

Znajdowanie pierwszego dnia miesiąca
-----------------------------

Pierwszy dzień kwietnia 2018 r. wypadł w niedzielę, ale jak to łatwo sprawdzić?

Od wersji PHP 5.1.0 istnieje proste rozwiązanie:

```php
echo date('N', strtotime('2018-04-01')); // 1 (poniedziałek), 7 (niedziela)
```

W starszych wersjach musimy sami zaimplementować tę funkcję:

```php
/**
 * @autor Jan Barášek
 */
function getFirstDayPosition(?int $year = null, ?int $month = null): int
{
    $day = (int) date('w', strtotime($year . '-' . $month . '-1')) - 1;

    return $day < 0 ? 7 : $day + 1;
}
```

Ponieważ modyfikator `w` zwraca dane wyjściowe w formacie amerykańskim, nadal dostosowuję numer dnia za pomocą prostego obliczenia. Funkcja zwraca liczbę całkowitą z przedziału od 1 (poniedziałek) do 7 (niedziela).

Przesunięcia czasowe / konwersja formatowania i sprawdzanie poprawności daty
--------------------------------------------------

Często musimy przeskoczyć o względny czas (powiedzmy +5 dni) lub pobrać datę z tekstu wprowadzonego przez użytkownika, aby była ona ważna. Aby to zrobić, należy użyć funkcji <a href="https://www.php.net/manual/en/function.strtotime.php">strtotime()</a> o następującej składni:

```php
echo strtotime('teraz');
echo strtotime('10 września 2000 r.');
echo strtotime('+1 dzień');
echo strtotime('+1 tydzień');
echo strtotime('+1 tydzień 2 dni 4 godziny 2 sekundy');
echo strtotime('najbliższy czwartek');
echo strtotime('w ostatni poniedziałek');
```

Wynikiem działania funkcji jest **timestamp** *(czas uniwersalny)* podanego czasu w postaci liczby całkowitej.

> Ogólnie rzecz biorąc, zalecam wdrożenie tej funkcji na wszystkich formularzach, w których w jakiś sposób pracujemy z czasem użytkownika. Z pewnością nie jest dobrym pomysłem zmuszanie użytkownika do zapisywania daty w jakimś konkretnym formacie, ale zawsze należy tworzyć taki format automatycznie, aby użytkownik mógł wpisać dowolną datę. Często bowiem tekst jest na przykład kopiowany skądś i ręczne formatowanie go jest zbyt pracochłonne, gdy można to zrobić automatycznie.

Liczba sekund od początku czasu
--------------------------

Od 1 stycznia 1970 r. każda sekunda jest dodawana do jedynki, co daje ogromną liczbę całkowitą oznaczającą czas, jaki upłynął od tego momentu. Jest to szczególnie przydatne do zwykłego obliczania różnic czasowych. Jest to dość niezawodne rozwiązanie, ale niesie ze sobą pewne ryzyko (problem roku 2038).

**Ogólne właściwości tej notacji:**
- Jest to liczba całkowita, więc łatwo się z nią pracuje,
- Jest on podawany w systemie dziesiętnym, więc jest łatwy do odczytania przez człowieka (z wyjątkiem tego, że nie można szybko określić dokładnego czasu),
- Nie można go użyć do reprezentowania okresu przed 1 stycznia 1970 roku, ponieważ nie może być ujemny *(niektóre nowe wersje algorytmu wprowadzają ujemne czasy, ale nie zawsze można na tym polegać)*,
- W roku 2038 przestanie on działać na komputerach 32-bitowych i spowoduje awarię programu (z powodu przepełnienia stosu i maksymalnego rozmiaru liczby całkowitej).

Problem roku 2038
--------------------------

<h2 id="universalTime" style="background: #222; margin-top: 32px; padding: 32px; color: white; text-align: center; border-radius: 3px;">Nie czytam...</h1>.

<script>.
	setInterval(function() {
		window.document.getElementById('universalTime').innerHTML = Math.floor(Date.now() / 1000);
	}, 250);
</script>.

> Nad tym tekstem jest wyświetlana bieżąca wartość czasu, która co sekundę jest zwiększana o +1. Bieżąca wartość jest wczytywana przez javascript, więc może nie zawsze być dokładna (wskazuje czas systemowy komputera).

Komputery przechowują liczby całkowite w postaci binarnej, a jeśli jest to liczba całkowita, to często ma ona 32 bity (32 jedynki i zera). Najwyższą możliwą liczbą 32-bitową jest: `011 111 111 111 111 111 111 111 11`.

Jeśli jednak co sekundę będziemy dodawać +1 do tej stałej, to pewnego dnia otrzymamy taką liczbę *(będzie to 19 stycznia 2038 roku o godzinie 03:14:07)*. Gdy jednak spróbujemy dodać kolejną 1, zakres bitów przestanie wystarczać (liczba będzie większa, niż można zmieścić w 32 bitach), więc nastąpi **przepełnienie stosu**, tzn. na początku liczby zostanie dodana 1, co w postaci binarnej oznacza **liczbę ujemną**, (więc prawdopodobnie spowoduje to awarię programu), przez co znajdziemy się gdzieś w roku 1901 (ugh!).

Istnieją dwa rozwiązania tego problemu:

- Rozszerzenie zakresu liczb całkowitych z 32 bitów do 64 bitów, co daje nam okres kilku tysięcy lat
- Należy użyć typu danych `DateTime` (powszechnie dostępnego w PHP), który zapewnia obiektowe podejście do zarządzania datami, jest dobrze kompatybilny z bazą danych i oferuje zakres lat od `0001` do `9999`, co jest wystarczające dla potrzeb większości aplikacji w świecie rzeczywistym.

Osobiście jestem zwolennikiem używania typu danych `DateTime` i nieużywania w ogóle zapisu liczb całkowitych.

Różne strefy czasowe
-----------------

PHP jest inteligentne, więc zawsze stara się używać najlepszej strefy czasowej w miejscu, w którym znajduje się serwer. Czasami jednak może się zdarzyć, że strefa czasowa jest niepoprawnie określona lub programujemy aplikację globalną, do której mają dostęp użytkownicy z całego świata, i dlatego musimy ręcznie zmienić strefę czasową.

Można to zrobić globalnie dla całego skryptu PHP, wywołując funkcję:

```php
date_default_timezone_set('UTC');
```

Czasami można spotkać się również z tym rozwiązaniem (wykrycie, że serwer ma strefę czasową inną niż UTC):

```php
$defaultTimeZone = 'UTC';

if (date_default_timezone_get() != $defaultTimeZone) {
    date_default_timezone_set($defaultTimeZone);
}
```

Zmień datę i godzinę
--------------------------

To nie PHP jest odpowiedzialne za pobieranie aktualnej daty i godziny, ale system operacyjny, na którym działa. Jeśli więc konieczna jest ręczna zmiana czasu, należy ją wykonać w tym miejscu.
