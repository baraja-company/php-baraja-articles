Jak zrozumieć kod PHP
=====================

> id: d8d452da-37b4-4502-a573-a27afe7ea37a
> slug:
> 	cs: jak-se-vyznat
> 	pl: jak-zrozumiec-kod-php
> 
> publicationDate: '2020-04-11 18:45:23'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c3012a19e6dd01770fad9ab456a5b6b3

Większość języków można zapisać na różne sposoby, uzyskując ten sam rezultat. Jednocześnie, po napisaniu kodu, prawdopodobnie kiedyś w przyszłości przeczytasz go i poprawisz lub dodasz nowe funkcje.

Dlatego, aby uniknąć konieczności ciągłego myślenia o kodzie i dobrze się nim poruszać, istnieje zestaw narzędzi i sposobów na "poprawne pisanie kodu" bezpośrednio w PHP, lub budowanie kodu w sposób, który bezpośrednio wspiera jego przyszłą czytelność (nawet przez innego człowieka).

> **Nota autora:**
>
> Doświadczenie pokazuje, że kod staje się przestarzały tak szybko, że nawet sam autor aplikacji po pół roku postrzega swój kod jako obcy. Jeśli więc od początku napiszemy go poprawnie, nie uniemożliwi to jego przyszłej rozszerzalności.
>
> W rzeczywistych projektach potajemnie wykazano, że jednolite formatowanie kodu i wprowadzenie ogólnych reguł zapobiega wielu błędom.

TL;DR
-----

Czytelność kodu jest często związana z formatowaniem i zasadami pisania.

W zespołach programistów sensowne jest ustalenie formalnych zasad formatowania i utrzymywania kodu.

Osobiście używam (w 2022 roku) standardu kodowania dla frameworka Nette, a reguły są sprawdzane automatycznie w każdym commit'cie. Więcej informacji na ten temat można znaleźć w artykule [Using GitHub CI](https://php.baraja.cz/github-actions-nejlepsi-ci-pro-rok-2021#hotove-priklady).

Instalację testu standardu kodowania i jego uruchomienie wykonuje się za pomocą kilku poleceń:

```shell
composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
php temp/coding-standard/ecs check src
```

Uwagi w kodzie
---------------

Uwagi te nie mają wpływu na przetwarzanie kodu i są przeznaczone wyłącznie do użytku programisty. W przypadku większych, bardziej kompletnych fragmentów kodu należy napisać notatkę wyjaśniającą, do czego dany kod służy i jak w zasadzie działa.

```php
// Definicje zmiennych
$a = 5;
$b = 3;
$c = 2;

// Suma wszystkich liczb
$sum = $a + $b + $c;

// Listing dla użytkowników
echo $sum;
```

Notatka zaczyna się od pary ukośników (`//`) i jest ważna do końca wiersza. Można go używać w dowolnym miejscu.

W notatce nie należy wyjaśniać konkretnej implementacji algorytmu, ale raczej jego ogólne zasady. Wynika to z faktu, że kod może z czasem ulec kilkukrotnej zmianie, w związku z czym należy również poprawić notatkę.

> **Nota autora:**
>
> Często zdarza się, że kod nie robi dokładnie tego, co wyjaśnia jego opis. Dzieje się tak głównie dlatego, że programista popełnił gdzieś błąd. W notatce należy więc opisać ogólne zasady, tak abyśmy mogli potem odpowiednio zmodyfikować kod. Nie należy jednak zapominać, że jedyną prawdę o tym, co faktycznie dzieje się w aplikacji, opisuje tylko sam kod, a notatka nie ma na to żadnego wpływu.

Graficzne oddzielenie części kodu
----------------------------

Podczas projektowania aplikacji ważne jest, aby oddzielić od siebie bloki logiczne. Zazwyczaj są one podzielone na funkcje, metody, a w przypadku kodu podstawowego przynajmniej na komentarze.

W dłuższych algorytmach zwykle na początku opisuję całą zasadę działania algorytmu, a następnie numeruję poszczególne miejsca w kodzie, aby programista mógł lepiej zrozumieć, na czym polega konkretna funkcjonalność oparta na tych miejscach.

```php
/**
 * Funkcja oblicza średnią arytmetyczną.
 *
 * 1. uzyskanie listy numerów
 * 2. Uzyskiwanie sumy i liczby liczb
 * 3. oblicz i wydrukuj średnią
 */

// 1.
$numbers = [1, 3, 8, 12];

// 2.
$sum = array_sum($numbers);
$count = count($numbers);

// 3.
echo 'Średnia wynosi:' . ($sum / $count);
```

Znaki `/**` rozpoczynają komentarz wielowierszowy, który obowiązuje aż do znacznika `*/`. Aby ułatwić czytanie, dobrze jest umieścić gwiazdkę na początku każdego wiersza.

Uwagi dotyczące dokumentacji
----------------------

Komentarze dokumentacyjne są zwykle używane do opisywania i dokumentowania funkcji (zachowanie, parametry, wartości zwracane, autor itp.).

We wcześniejszych wersjach PHP (przed `7.0`) typy danych nie były jeszcze używane, więc typ danej zmiennej był opisywany bezpośrednio w komentarzu.

```php
/**
 * @author Jan Barášek <jan@barasek.com>.
 * @licencja MIT
 * @link https://php.baraja.cz
 * @param float[] $numbers
 */
function average(array $numbers): float
{
    $sum = array_sum($numbers);
    $count = count($numbers);

    return $sum / $count;
}
```

Komentarze do dokumentacji są nazywane "dokumentacją" głównie dlatego, że mają z góry ustalony format, który jest zrozumiały dla określonych środowisk programistycznych (i edytorów), ale także dla zautomatyzowanych narzędzi do generowania dokumentacji lub sprawdzania kodu.

Pisać kod w języku czeskim czy angielskim?
-----------------------------

Cały kod piszę wyłącznie w języku angielskim (w tym nazwy funkcji, zmienne, komentarze, ...).

Ma to wiele zalet:

- Programista może od razu aktywnie szkolić swój angielski.
- Duża część aplikacji korzysta z bibliotek innych firm, które są w języku angielskim, więc automatycznie zachowuje spójność
- Większość zaawansowanych materiałów nie ma w ogóle tłumaczenia na język angielski
- Jestem pewien, że można podać wiele innych przykładów

PHP nie wymaga bezpośrednio znajomości języka angielskiego i możesz pisać wszystko po angielsku. Używanie języka angielskiego traktuję bardziej jako rodzaj inwestycji na przyszłość oraz możliwość łatwego rozszerzenia kodu przez inne osoby, dla których angielski nie jest językiem ojczystym.

Całkowicie zlokalizowany kod w języku angielskim jest również używany w firmach, dlatego dobrze jest od samego początku ćwiczyć język angielski.

Kolejność operacji numerycznych
------------------------

Zawsze pamiętaj, że PHP zaokrągla liczby podczas wykonywania operacji numerycznych. Często może to być uciążliwe, ponieważ każdemu wynikowi z liczbami dziesiętnymi towarzyszy pewna niedokładność.

Wydaje się, że dobrym rozwiązaniem jest najpierw inkrementacja liczb, a następnie obliczanie z użyciem największych możliwych liczb. W ten sposób zniekształcenia są statystycznie mniejsze.

Przykład:

```php
echo 10 / 3; // Pisze 3,3333333333333
```

W niektórych przypadkach można też zastosować sztuczkę polegającą na nieużywaniu w ogóle ułamków dziesiętnych i obliczaniu wszystkiego jako liczby całkowitej. W tym przypadku takie zniekształcenia nie występują:

```php
echo 1 / 2 * 2; // to jest gorsze, ponieważ 1/2 = 0,5*2 = 1
echo 2 * 1 / 2; // tak jest lepiej, bo 2*1 = 2/2 = 1
```

Podczas rozwiązywania dużych, złożonych działań liczbowych, do zapisu liczb należy używać ułamków.
