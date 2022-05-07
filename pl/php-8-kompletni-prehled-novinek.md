Ukazał się PHP 8 - pełne omówienie
==================================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	pl: ukazal-sie-php-8---pelne-omowienie
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

Dzisiaj, 26 listopada 2020 roku, po kilku latach została wydana nowa, duża wersja PHP 8, która zawiera wiele nowych funkcji. Jest to jedna z największych aktualizacji od dłuższego czasu i zasługuje na osobny artykuł.

W tym artykule podsumujemy wszystkie najważniejsze nowe funkcje oraz różnice w składni i opcjach w porównaniu ze starszą wersją programu. Większość nowych funkcji jest kompatybilna wstecz i wprowadza ulepszenia w zakresie zachowania, które na pewno spodobają się użytkownikom.

> **Ważne informacje:** PHP 8 jest obecnie w fazie `zamrożenia funkcjonalności`, co oznacza, że nie można już dodawać nowych zachowań, a jedynie naprawiać błędy. Dzięki temu można liczyć na kompatybilność i pełne debugowanie aplikacji.

Rodzaje Unii
----------

W ostatnich latach PHP przeszedł transformację od języka czysto dynamicznego, w którym dowolna zmienna mogła zawierać dowolne dane, do języka ścisłego, w którym z góry wiadomo, jakiego typu dane znajdą się w danej zmiennej, parametrze, argumencie czy właściwości. Użycie [data-types](/datove-types) jest nadal opcjonalne, ale zalecam stosowanie silnego typowania i sam używam go we wszystkich projektach.


Typy unii wyrażają kolekcję wielu typów, akceptując dowolny argument lub właściwość w nich zawartą.

Na przykład:

```php
function validatePsc(string|int $psc): bool
{
	// implementacja
}
```

Funkcja `validatePsc()` w zmiennej `$psc` akceptuje typy danych `string` (łańcuch znaków) i `int` (liczba całkowita).

W poprzedniej wersji PHP 7.4, notacja ta nie była możliwa i zazwyczaj była omijana przez [comment](/document-comment):

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// implementacja
}
```

Jednak ten komentarz do adnotacji jest ignorowany przez PHP (w końcu jest to komentarz) i musieliśmy wykonać dodatkowe sprawdzenie za pomocą zewnętrznego narzędzia, takiego jak PhpStan, co wielu programistów zignorowało. Teraz sprawdzanie odbywa się bezpośrednio w czasie wykonywania (podczas działania aplikacji) i nie można go ominąć.

Jednak PHP zna pewien typ unii od wersji 7, kiedy to można było powiedzieć, że typ główny może być również `nullable`, tzn. akceptować główny typ danych plus wartość `null`.

Zostało to napisane na dwa sposoby, z których każdy ma inne znaczenie:

```php
function setPhone(?string $phone): void
{
	// implementacja
}

// lub

function setPhone(string $phone = null): void
{
	// implementacja
}

// lub kombinacja

function setPhone(?string $phone = null): void
{
	// implementacja
}
```

Wszystkie wpisy mówią, że telefon `int` (integer) jest albo `string` albo `null`.

- Pierwsza notacja zawsze wymaga przekazania wartości
- Druga notacja nie wymaga podania żadnej wartości; jeśli nic nie zostanie podane, domyślną wartością jest `null` (jest to argument opcjonalny)
- Trzeci wpis jest połączeniem opcji i zachowuje się podobnie jak drugi wpis

Podczas używania typów unii nie będziemy już mogli używać notacji ze znakiem zapytania i musimy ściśle zdefiniować na przykład typ danych `null`:

```php
function setPhone(string|int|null $phone = null): void
{
	// implementacja
}
```

Numer telefonu musi być teraz `string`, `int` lub `null`.

Typy unii nadal mają wiele zastosowań, o których zaawansowani programiści przeczytają w dokumentacji lub implementacji konkretnych bibliotek.


JIT - szybsze przetwarzanie skryptów
----------------------------------

Kompilator JIT (just in time) znacznie poprawia wydajność komplikowania skryptów (parsowania i rozumienia). Zachowanie to może się jednak różnić w kontekście żądań internetowych.

Możesz teraz sprawdzić, czy masz włączony JIT na pasku Tracy w frameworku Nette, a więcej szczegółów znajdziesz w [osobnym artykule](https://stitcher.io/blog/php-jit).

Ogólnie o kompilacji można powiedzieć, że PHP stara się przetwarzać kod z góry, tak aby przy przetwarzaniu konkretnego żądania nie musiał on przeglądać fizycznego pliku skryptu, parsować go i interpretować. W przeszłości było to obsługiwane za pomocą rozszerzenia OPCache (domyślnie dostępnego na serwerach i hostach), co zwiększało szybkość przetwarzania o około połowę.

Ogólna zasada mówi, że w przypadku powolnej aplikacji zawsze lepiej jest wybrać odpowiedni algorytm do obsługi danego zadania niż dokonywać mikrooptymalizacji kodu. Zazwyczaj duże opóźnienia są spowodowane oczekiwaniem na bazę danych i jej powolnymi zapytaniami, przechowywaniem sesji, oczekiwaniem na zwolnienie miejsca na dysku twardym oraz innymi operacjami sprzętowymi.

Operator nullsafe (opcjonalny łańcuch)
-------------------------------------

Bardzo często w rzeczywistych zastosowaniach konieczne jest sprawdzenie istnienia wartości zwracanej (czy nie jest ona `null`) z jednej metody, a następnie warunkowe wywołanie innej. Operatory [trójskładnikowe](/ternary-operator) świetnie się do tego nadają, ale działają tylko z jednym warunkiem i nie można ich zagnieżdżać. Operator nullsafe umożliwia zagnieżdżanie w sposób natywny.

> **TIP:** Praktycznie takie samo zachowanie jest już obsługiwane przez system szablonów Latte, ale nadpisuje on tego typu składnię w natywnym kodzie PHP, dzięki czemu można używać operatora nullsafe w starszych wersjach PHP (od PHP 7). Gratulacje dla Davida za tę modyfikację!

Dzięki temu jest łatwy w użyciu:

```php
$orderId = $order?->getId();
```

Zmienna `$orderId` zawiera albo wartość zwróconą przez metodę `getId()`, albo `null`, jeśli zmienna `$order` zawiera wartość `null`, a metoda `getId()` nie mogła zostać wywołana.

Tego typu problem został ominięty w PHP 7 dzięki następującej składni wykorzystującej operator trójskładnikowy:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Prawdopodobnie jest to warunek:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

Zapis można umieścić w dalszej części rozmowy. Próbkę zaczerpnąłem z [dokumentacji Latte] (https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), która doskonale ją opisuje:

```php
$orderName = $order->item?->name;

// to samo co:

$orderName = isset($order->item) ? $order->item->name : null;
```

Typowe zastosowanie to wyszczególnianie bardziej złożonych struktur w szablonie, na przykład w Latte wygląda to tak (przykład zaczerpnięty z dokumentacji):

```php
{$user?->address?->street}
// means approx ($user !== null) && ($user->address !== null) ? $user->adres->ulica : null

{$items[2]?->count}
// replace approx ($items[2] !== null) ? $items[2]->count : null

{$user->getIdentity()?->name}
// replace approx $user->getIdentity() !== null ? $user->getIdentity()->name : null
```

W prawdziwym kodzie może to wyglądać na przykład tak, że chcemy dowiedzieć się, z jakiego kraju pochodzi klient, czytając jego profil (a dane w bazie są ładnie przechowywane za pomocą sesji, tak jak powinno to wyglądać), to w starym PHP wyglądało to tak:

```php
$country =  null;
if ($session !== null) {
    $user = $session->user;
    if ($user !== null) {
        $address = $user->getAddress();
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

Teraz można je skrócić do jednego wiersza:

```php
$country = $session?->user?->getAddress()?->country;
```

Użycie operatora nullsafe zapobiega również różnym błędom, które nie byłyby łatwe do wykrycia przez niedoświadczonego programistę w PHP 7.

Na przykład ten wpis spowoduje wygenerowanie błędu krytycznego:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// return: fatal error: uncaught Error: call to a member function format() on null
```

Poprawna składnia jest następująca:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// zwraca: null
```

Argumenty nazwane
---------------------

W starym, dobrym PHP, wywołania funkcji z argumentami musiały być pisane poprzez przekazywanie argumentów w dokładnej kolejności określonej przez funkcję docelową. Nie ma w tym nic złego, jednak w przypadku stosowania wielu parametrów o podobnych wartościach może to spowodować pogorszenie czytelności. Albo jeśli chcieliśmy przekazać aż do n-tego parametru w kolejności, wszystkie parametry opcjonalne musiały być przekazane wcześniej, co mogło mieć negatywny wpływ na czytelność i kompatybilność w przyszłości.

Wyobraźmy sobie na przykład funkcję `setCookie()` w Nette, która ma wiele argumentów:

```php
public function setCookie(
	string $name,
	string $value,
	$time,
	string $path = null,
	string $domain = null,
	bool $secure = null,
	bool $httpOnly = null,
	string $sameSite = null
)
```

Pierwsze trzy argumenty (`$name`, `$value` i `$time`) są obowiązkowe, ale jeśli chcieliśmy przekazać argument `$httpOnly`, musieliśmy przekazać wszystkie poprzednie i poprawnie obliczyć ich kolejność:

```php
$http->setCookie(
	'myCookie',
	'David lubi konie',
	'teraz',
	null, // ścieżka
	null, // domena
	null, // bezpieczny
	true
);
```

Czego po prostu nie chcesz robić, jeśli nie musisz.

Eleganckie pismo wygląda wtedy tak:

```php
$http->setCookie(
    name: 'myCookie',
    value: 'David lubi konie',
    time: 'teraz',
    httpOnly: true
);
```

Ten typ składni wymaga, aby nazwy argumentów w funkcji docelowej nigdy się nie zmieniały, ponieważ w momencie wywołania będą one nadal wpisane. Przynajmniej programiści będą mogli je lepiej nazwać.

Jeśli chcemy użyć tylko jednego z argumentów, składnię można połączyć i skrócić do jednego wiersza:

```php
$http->setCookie('myCookie', 'David lubi konie', 'teraz', httpOnly: true);
```

Pierwsze 3 argumenty są przekazywane w oryginalny sposób, a następnie przekazywany jest opcjonalny argument `httpOnly` (ponieważ ma taką nazwę).

Atrybuty
---------

Większość głównych języków, takich jak Java czy C#, zawiera już natywnie tzw. adnotacje, czyli rodzimą składnię języka, która umożliwia dodawanie metainformacji do innych konstrukcji językowych.

W PHP od dawna nie ma tego typu składni i jest ona omijana poprzez używanie komentarzy DOC, które są klasycznym komentarzem do metody, tyle że z dwiema gwiazdkami `/**`.

Komentarze te są ignorowane podczas przetwarzania skryptu i należy dodać specjalną logikę użytkownika, która będzie je analizować i interpretować w czasie wykonywania skryptu za pomocą refleksji. Zapewne rozumiesz, jaki to może mieć wpływ na wydajność, a ponadto składnia komentarzy nie jest wymagana i bardzo trudno ją sprawdzić w czasie kompilacji (gdy skrypt jest przetwarzany przed uruchomieniem), a do tego trzeba użyć dodatkowych narzędzi spoza normalnego zestawu narzędzi PHP.

Aby zachować kompatybilność wsteczną, PHP udostępnia atrybuty o składni podobnej do alternatywnej notacji komentarzy, co nie przerywa działania skryptu w starszych wersjach PHP.

Oryginalna notacja (używana na przykład w przypadku zależności Inject w programie Nette Presenter):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

Teraz można usunąć komentarz i użyć atrybutu natywnego:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Bardzo ważne jest, aby atrybut nie był już tylko kawałkiem łańcucha w komentarzu, ale fizyczną klasą, która jest poprawnym kodem PHP.

Jest to świetne rozwiązanie, ponieważ można teraz bezpiecznie sprawdzać poprawność danych wejściowych do atrybutu, a użycie atrybutu staje się wywołaniem jego konstruktora, w którym można zastosować inną logikę. Z niecierpliwością czekam na natywną obsługę tego rozwiązania przez Doctrine, które wykorzystuje adnotacje do wszystkiego.

Implementacja samego atrybutu może wyglądać następująco:

```php
#[Attribute]
class Inject
{
    public string $value;

    public function __construct(string $value)
    {
        $this->value = $value;
    }
}
```

Ścisła logika może być ponownie zastosowana w atrybutach, np. do sprawdzania typów danych argumentów, typów unii i innych właściwości języka.

Dopasowanie wyrażenia
----------------

Nowa konstrukcja językowa `match()` jest unowocześnionym ulepszeniem starej dobrej `switch()` (której staram się nie używać) i przynosi wiele fajnych funkcji (które sprawią, że znów zacznę jej używać).

Na przykład chcemy zmodyfikować wartość zmiennej na podstawie danych wejściowych:

```php
$pozdrav = match(bool $formal) {
    true => 'Witaj',
    false => 'Cześć',
};
```

Ważną nową cechą tej składni jest to, że nie musimy używać `break` (jak w starym `switch`) i składnia jest ogólnie dużo bardziej ekonomiczna.

Jednocześnie możemy sprawdzać poprawność wielu danych wejściowych jednocześnie w ramach warunku (oddzielonych przecinkiem) i ewentualnie zwracać wartość domyślną (gdy żaden z nich nie spełnia warunków).

Przydaje się to na przykład podczas przepisywania kodu warunku HTTP na komunikat o błędzie (z pewnością docenisz to podczas obsługi kodów wyjątków):

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'nie znaleziono',
    500 => 'błąd serwera',
    default => 'nieznany kod stanu',
};
```

Porównywanie wartości odbywa się ściśle za pomocą operatora `==` (switch używa tylko `==`), co ponownie pokazuje, że PHP podąża ścisłą ścieżką projektowania. Dlatego dane wejściowe `'200'` (łańcuch zawierający liczbę) nie zostaną zaakceptowane w poprzednim przypadku.

Jeśli nie zostanie podana wartość dla `default` i nie ma dopasowania, zostanie wyrzucony błąd `UnhandledMatchError`.

Nowa składnia umożliwia również użycie wyrażenia lub wywołania funkcji do dopasowania (zachowuje się ono jak warunek). W przypadku wystąpienia błędu możemy wtedy rzucić wyjątek (ponieważ żeton `throw` jest teraz wyrażeniem i może być użyty w ten sposób):

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'nieznany kod stanu',
};
```

Propagacja właściwości w konstruktorze
-------------------------------------------------

Jest to tylko cukier syntaktyczny, który przyda się do szybkiego i łatwego definiowania encji i jej właściwości bezpośrednio w konstruktorze.


Na przykład, jednostka pierwotna:

```php
final class User
{
    public string $name;

    public function __construct(
        string $name,
    ) {
        $this->name = $name;
    }
}
```

Można ją skrócić tylko do:

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

Właściwość `$name` jest walidowana względem typu danych `string`, a jej wartość może być odczytana bezpośrednio z instancji, ponieważ jest to właściwość publiczna. Jeśli używasz dodatkowego obiektu SmartObject w Nette (czego raczej nie polecam w PHP 8), możesz także uzyskać dostęp do prywatnych właściwości, wywołując najpierw ich metodę getter, a taka składnia ponownie upraszcza sprawę.

Typ zwrotu statyczny
--------

W przeszłości można było używać typu danych `self` jako wartości zwracanej przez metodę, ale zwracała ona instancję klasy, w której została zdefiniowana. Typ danych `static` może to zrobić nawet w przypadku dziedziczenia i zwróci typ danych klasy, z której została wykonana instancja, a nie jej przodka.

Na przykład:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Typ danych mieszany
------------------

Typ `mieszany` może być teraz używany jako argument funkcji lub metody. Oznacza to, że metoda musi zawsze przyjmować jakieś dane wejściowe (a więc jest argumentem obowiązkowym).

Jeśli jest to choć trochę możliwe, zawsze używaj bezpośredniego typu danych lub przynajmniej unii. Mieszane jest przydatne tylko wtedy, gdy funkcja przyjmuje naprawdę wszystko. W praktyce jest to użyteczne, na przykład w różnych funkcjach dump, które przyjmują dowolne dane wejściowe i muszą być w stanie je wyświetlić.

Typ `mixed` akceptuje następujące typy: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

Następnie David użyje typu mieszanego do swojej funkcji:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Rzut tokenem jako wyrażeniem
------------------------

Żeton `throw` stał się teraz wyrażeniem, co w praktyce oznacza, że wyjątek może zostać rzucony, gdy funkcja lambda `fn()` jest obcięta lub gdy sprawdzany jest operator trójskładnikowy:

```php
$error = fn () => throw new \InvalidArgumentException('To zawsze powoduje wyświetlenie błędu.');

$userName = $user['nazwa'] ?? throw new \LogicException('Użytkownik musi mieć nazwę.');
```

Funkcja str_contains()
-----------------------

PHP zawiera w końcu natywną funkcję sprawdzającą, czy domyślny łańcuch zawiera podłańcuch.

Na przykład:

```php
if (str_contains('Honzik lubi koty.', 'koty')) {
	echo 'Funkcja ta obsługuje koty.';
}
```

W przeszłości występowanie podłańcucha było sprawdzane za pomocą funkcji [strpos](/strpos-function):

```php
if (strpos('Honzik lubi koty.', 'koty') !== false) {
	echo 'Funkcja ta obsługuje koty.';
}
```

Funkcje str_starts_with() i str_ends_with()
----------------------------------------------

Para nowych funkcji do sprawdzania, czy łańcuch zaczyna się lub kończy podłańcuchem:

```php
str_starts_with('Honzik lubi koty.', 'Honzik'); // true

str_ends_with('Honzik lubi koty.', 'koty.'); // true
```

Funkcja get_debug_type()
-------------------------

Rozszerzenie danych wyjściowych istniejącej funkcji [gettype](/function-gettype), która zwracała tylko typ ogólny przekazanej zmiennej. Funkcja ta jest używana na przykład podczas rzucania wyjątku, gdy otrzymujemy nieważne dane wejściowe i chcemy poinformować użytkownika, co tak naprawdę przekazał.

Kiedy wywołujemy funkcję `gettype()` ze zmienną zawierającą instancję klasy `AppUser`, funkcja zwraca `object`, więc nie wiemy, jaka to jest klasa. Nowa funkcja `get_debug_type()` zwraca nazwę klasy.

Funkcja get_resource_id()
--------------------------

Ta funkcja zwraca identyfikator zasobu zewnętrznego ze zmiennej.

Na przykład, połączenie z bazą danych MySql jest obsługiwane przez PHP za pomocą specjalnego typu danych `resource`, dzięki czemu można dowiedzieć się, jaki identyfikator został jej przypisany.

> **Nota historyczna:**
>
> Typ `resource` w PHP został stworzony w czasie, gdy nie wiedziano jeszcze, jak używać obiektów, i trzeba było jakoś wymyślić, jak przekazywać referencje do czegoś takiego jak typ `data`. W przyszłości można się spodziewać, że `resource` zostanie całkowicie usunięty z języka, więc lepiej nie używać tej właściwości.

Rozszerzenie ext-json jest zawsze dostępne
-------------------------------------

W przeszłości PHP mogło być kompilowane bez obsługi json. Teraz json będzie zawsze dostępny, więc można usunąć zależność `ext-json` z plików `composer.json` i zawsze wiedzieć, że json może być używany.

Pierwszeństwo konkatenacji
-------------------------------------------------------

Wyobraźmy sobie coś takiego:

```php
echo 'Suma summarum:' . $a + $b;
```

Czy najpierw dodawane są liczby, czy też zmienna `$a` jest dołączana do łańcucha, a następnie cały nowy łańcuch jest dodawany do `$b`?

Można by się spodziewać, że najpierw zostanie wykonane dodawanie, ale to tylko założenie. PHP w rzeczywistości robi coś takiego:

```php
echo ('Suma summarum:' . $a) + $b;
```

PHP 8 będzie się teraz zachowywał w sposób przewidywalny:

```php
echo 'Suma summarum:' . ($a + $b);
```

Ogólnie jednak zawsze lepiej jest używać nawiasów do oddzielania wyrażeń.

Stabilne uporządkowanie
---------------

Przed PHP 8 sortowanie ciągów znaków odbywało się przy użyciu tzw. algorytmu niestabilnego, co oznacza, że PHP nie gwarantowało, że elementy o tej samej (lub równoważnej) wartości nie zostaną zamienione. W nowej wersji zmieniono zachowanie wszystkich funkcji sortowania na stabilne, dzięki czemu sortowanie jest zawsze wykonywane deterministycznie, a użytkownik zawsze otrzymuje to samo wyjście.

Rozwiązuje to na przykład problem, gdy oceny użytkowników były klasyfikowane według trafności, ale niektóre oceny miały ten sam wynik. Teraz przy każdym sortowaniu będą one wyświetlane w tej samej kolejności i nie będą ciągle pomijane.

Inne nowe funkcje
---------------

PHP posiada wiele innych, pomniejszych nowych funkcji i ulepszeń. Na przykład błędy będą rzucane inaczej (ale to nie przeszkadza nam, którzy piszemy bezbłędny kod, prawda?).

Pełną listę zmian można zawsze znaleźć w oficjalnej dokumentacji i we wpisie RFC.

Czego mi brakuje w nowym PZP
-----------------------

Chciałbym, żeby PHP w końcu obsługiwało złożone typy tablicowe, na przykład kiedy metoda zwraca tablicę identyfikatorów, wciąż musimy podawać tylko `getIds(): array`, a coś takiego jak `getIds(): int[]` byłoby o wiele lepsze. Być może już wkrótce doczekamy się tego i silne sprawdzanie typów będzie kompletne.

Więcej zasobów
------------

David Grudl opowiedział o nowościach w firmie Posobot. Polecam obejrzenie nagrania:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

W ten sposób chcę podziękować Davidowi za jego wykład, ponieważ zaczerpnąłem z niego pewne informacje do tego artykułu. W szczególności informacje o przejściu Nette na PHP 8 i inne zakulisowe wskazówki dotyczące PHP.
