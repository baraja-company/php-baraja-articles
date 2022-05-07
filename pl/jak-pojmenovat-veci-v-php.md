Jak nazywać zmienne, funkcje, metody i klasy?
=============================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	pl: jak-nazywac-zmienne-funkcje-metody-i-klasy
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

Aby zachować porządek w kodeksie, ważne jest ustalenie jasnych zasad tworzenia nazw. Ta strona służy jako przegląd stosunkowo popularnych podejść stosowanych przez dużą liczbę programistów, w tym przeze mnie i osoby, z którymi współpracuję.

Jeśli pracujesz w zespole programistów, korzystaj z ich zasad, ale dla ogólnego rozwoju równie korzystne jest wyrobienie sobie kilku dobrych nawyków.

Ponieważ pojęcie całej składni PHP jest bardzo szerokie, podzieliłem ten przewodnik na wiele kategorii, które są wysoce wyspecjalizowane.

Jeśli szukasz szybkiego rozwiązania, polecam zapoznanie się z <a href="https://www.php-fig.org/psr/psr-4/">standardem PSR-4</a>.

Wstawianie skryptów, łączenie z HTML, ładowanie plików
---------------------------------------------------

Każdy skrypt musi zaczynać się od znacznika `<?php`.

Jeśli na końcu pliku nie ma **żadnego HTML**, nie należy go kończyć (aby zapobiec pojawieniu się białego znaku na końcu strony).

Wczytywanie innych plików powinno odbywać się zgodnie z następującymi zasadami:

- Jeśli plik nie jest krytyczny i można z niego zrezygnować podczas renderowania strony (na przykład dodatkowe informacje w stopce), powinien zostać załadowany przez include: `include 'file.php';`
- Pliki krytyczne tylko poprzez konstrukcję require: `require 'file.php';`
- Jeśli pracujemy z obiektami, używamy konstrukcji require do załadowania autoloadera, który sam zajmie się załadowaniem innych klas (większość frameworków implementuje takie zachowanie).


Jeśli jest to choć trochę możliwe, powinniśmy oddzielić logikę pobierania danych od renderowania do HTML, czyli zastosować model MVC (model - widok - kontroler).

Najpierw więc przygotowujemy dane w programie, na przykład, Presenter:

`Presenter.php`.

```php
$cisla = [1, 2, 3];

$data = [];
$data['numery'] = $cisla; // przekaż dane do szablonu
include 'renderCisel.php'; // wczytaj szablon
```

A następnie narysuj go w szablonie:

`renderCisel.php`.

```html
<table>
<?php
    foreach ($data['cisla'] as $cislo) {
        echo '<tr><td>' . $cislo . '</td></tr>';
    }
?>
</table>
```

Takie podejście jest stosowane przez większość frameworków i jest przydatne dla zwiększenia przejrzystości kodu. W dalszej części procesu tworzenia aplikacji podejście to powinien stosować każdy doświadczony programista, który chce tworzyć aplikacje o przejrzystej strukturze (w przypadku dużych aplikacji podejście to jest koniecznością).

Nazwy zmiennych
----------------

Jeśli zmienna zawiera tablicę wartości lub inne obiekty, powinna być nazwana w liczbie mnogiej:

```php
$numbers = [1, 2, 3];
```

Ponieważ wtedy możemy po prostu iterować wartości po jednej liczbie:

```php
foreach ($numbers as $number) {
    // przetwarzanie liczb
}
```

Nazwa składająca się z wielu słów jest łączona w jedno długie słowo za pomocą składni cameCase, tzn. pierwsze słowo zaczyna się małą literą, a każde następne wielką literą:

```php
$promenna = 'Hej! Hej!';
$seznamUzivatelu = [
   'Jan Barášek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // Skrót PHP został pomniejszony
```

Nazwy funkcji i metod
--------------------

Funkcje i metody powinny zawsze jasno określać w swojej nazwie, co robią. Często w nazwie można również zawrzeć oczekiwane parametry wejściowe i wartość zwracaną.

Spróbuj odgadnąć, co robią poniższe funkcje i jaka jest ich wartość zwracana:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

Cała sztuczka tkwi w pierwszym słowie w nazwie, które jasno określa, jakiej metody użyje dana funkcja. Zwykle stosuje się następującą konwencję:

- `get` - pobieranie danych w postaci tablicy lub obiektu, parametry wejściowe określają poszukiwaną encję
- `save` - zapis do pliku lub bazy danych
- `create` - tworzenie encji (na przykład tworzenie instancji obiektu)
- `set` - zapisywanie danych do predefiniowanej zmiennej (wewnątrz funkcji)

Nazwy klas
----------

Klasa jest dużą jednostką, która zawiera dużą liczbę właściwości i metod, dlatego też powinna zaczynać się od dużej litery. Klasa powinna również przenosić tylko jedną encję (i opisywać jej właściwości), dlatego powinna być nazwana nazwą pojedynczą. Jeśli musimy pracować z wieloma encjami, możemy po prostu przechowywać każdą instancję w tablicy.

Przykład:

```php
class User
{
    public string $username;

    public string $password;

    public string $role;
}

class Users
{
    /** @var Użytkownik[] */
    public array $users;

    public function addUser(User $user): void
    {
        $this->users = array_push($this->users, $user);
    }
}
```

Klasa **User** specjalizuje się w informacjach dotyczących tylko jednego konkretnego użytkownika. Jeśli chcemy pracować z wieloma użytkownikami, tworzymy kolejną klasę (envelope), która przenosi tablicę instancji określonej encji.

W tym celu często przydają się również fabryki, które pozwalają na łatwe tworzenie podobnych obiektów i recykling oryginalnych instancji, dzięki czemu kod jest bardziej przejrzysty i oszczędza zasoby systemowe.

Przestrzeń nazw - Przestrzenie nazw
---------------------------

Przestrzeń nazw jest niezależna od fizycznego katalogu, w którym dostępny jest skrypt, ale dobrą praktyką jest przynajmniej częściowe respektowanie układu projektu (co prowadzi do lepszego systemu tworzenia nowych nazw, który jest w ten sposób bardziej jednoznaczny).

Osobiście nazywam przestrzenie nazw zgodnie z podkatalogiem wspólnym dla klas tego typu.

Przykłady:

```php
App\Presenters; // Oto nazwiska wszystkich prezenterów
App\Model;      // Jest to nazwa modelu ogólnego
App\Model\Math; // To jest nazwa modelu, który pracuje z matematyką
```

Aby zapewnić prawidłowe autoloadowanie klas, warto stosować się do standardu <a href="https://jakpsatphp.cz/PSR4/">PSR-4</a>.

Niestandardowe konwencje (np. w zespole firmowym)
-----------------------------------------

A jak Ty nazywasz swoje? Będę wdzięczny za wskazówki, jak ulepszyć ten artykuł.

Ogólnie rzecz biorąc, niestandardowe konwencje w zespole nie mają większego sensu, ponieważ utrudniają przenoszenie kodu na inne frameworki, a kiedy zatrudnia się nowego pracownika, musi on nauczyć się aktualnych konwencji. Dlatego najlepiej jest stosować się do standardu `PSR-4`.
