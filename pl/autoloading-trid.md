Autoloading klas w PHP
======================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	pl: autoloading-klas-w-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

Jestem pewien, że to wiesz, kiedy programując skrypty PHP, dzielimy kod na wiele plików i aby mieć dostępne wszystkie części, ładujemy je za pomocą serii wywołań `include`, `require` lub najlepiej `require_once`, co gwarantuje załadowanie tylko raz.

W kodzie wygląda to następująco:

```php
require_once 'Router.php';
require_once 'Strona.php';
require_once 'Paginator.php';
```

Główną wadą tego podejścia jest to, że programista musi stale upewniać się, że wszystko jest zawsze załadowane. Jeśli jednak użytkownik dużo ładuje, niepotrzebnie traci wydajność i wielokrotnie trafia na dysk. Z rozwiązaniem podręcznikowym wiążą się więc same problemy.

Natywne automatyczne ładowanie
-------------------

Na szczęście w PHP istnieje natywne wsparcie dla tak zwanego **Class Autoloading**, czyli logiki w kodzie, która ładuje plik klasy tylko wtedy, gdy jest on po raz pierwszy potrzebny (zazwyczaj przy pierwszym utworzeniu instancji).

Prosta implementacja może wyglądać następująco:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

Podczas tworzenia instancji klasy `MyClass1`, funkcja `spl_autoload_register` odczytuje plik `MyClass1.php` z katalogu `src`. W tej implementacji przyjęto, że każda klasa znajduje się w osobnym pliku nazwanym według nazwy klasy lub interfejsu.

Bardziej złożone przypadki automatycznego załadunku
-------------------------------

W rzeczywistych zastosowaniach może wystąpić wiele nieprzyjemnych sytuacji, które komplikują na przykład pracę w trybie autoload:

- Klasa lub interfejs w ogóle nie istnieje
- Plik został już raz załadowany
- W tym samym pliku znajduje się wiele klas lub interfejsów
- Ścieżka do klasy lub interfejsu nie jest zgodna z nazwą
- Lokalizacja klasy lub interfejsu zmienia się w czasie

Nie musimy jednak w tym celu programować własnego rozwiązania, lecz możemy wykorzystać istniejące, raz zaprojektowane.

Jeśli używasz <a href="https://getcomposer.org/doc/01-basic-usage.md">Composera</a>, prawdopodobnie korzystasz również z jego natywnego autoloadingu. Dzieje się tak, ponieważ podczas instalacji dowolnego pakietu Composer automatycznie generuje `mapę klas`, która jest przeglądem klas i ich fizycznych lokalizacji.

Następnie na początku kodu (zwykle w `index.php`) wystarczy użyć:

```php
require __DIR__ . '/vendor/autoload.php';
```

Jednak autoloading jest generowany tylko raz, gdy wywoływana jest komenda `composer dump`, więc konieczne jest regenerowanie autoloadingu za każdym razem, gdy aplikacja się zmienia.

RobotLoader - eleganckie rozwiązanie dla deweloperów
----------------------------------------

Do lokalnego rozwoju bardzo lubię pakiet `nette/robot-loader`, który automatycznie przemierza strukturę katalogów i buforuje lokalizacje klas. Jeśli więc wczytujemy klasę, najpierw sprawdzana jest ona w pamięci podręcznej, a dopiero jeśli nie istnieje, następuje automatyczne przeindeksowanie projektu. W związku z tym programista nie musi w ogóle śledzić, gdzie znajduje się dany plik lub klasa, i po prostu programuje.

Instalacja za pomocą aplikacji Composer:

```txt
composer require nette/robot-loader
```

Podstawowe objaśnienia funkcji są opisane w samej <a href="https://doc.nette.org/cs/3.0/robotloader">dokumentacji</a>:

> Podobnie jak robot Google indeksuje strony internetowe, RobotLoader przeszukuje wszystkie skrypty PHP i notuje, jakie klasy, interfejsy i cechy w nich znalazł. Następnie buforuje wyniki swoich badań i wykorzystuje je w następnym żądaniu. Wystarczy więc określić, które katalogi mają być indeksowane i gdzie mają być buforowane.

Jest on wtedy niezwykle łatwy w użyciu:

```php
$loader = new Nette\Loaders\RobotLoader;

// dodaj katalogi, które RobotLoader ma indeksować
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// ustaw buforowanie na dysku w katalogu 'temp'.
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // uruchom RobotLoader
```

Ustawienie `$loader->setAutoRefresh(true lub false)` określa, czy RobotLoader powinien przeindeksować pliki, gdy napotka nową klasę. Ta funkcja powinna być wyłączona na serwerach produkcyjnych.

Teraz już nigdy więcej nie będziesz musiał zmagać się z automatycznym ładowaniem.

Rozwiązanie łączone
------------------

Podczas tworzenia prawdziwego projektu stosuję rozwiązanie łączone.

W praktyce wygląda to tak, że ładuję zainstalowane pakiety przez Composer autoload (który jest bardzo wydajny) i to rozwiązuje problem ładowania wszystkich klas w katalogu `vendor`.

Kod dla danego projektu jest następnie umieszczany w katalogu `app`, gdzie za pomocą RobotLoader automatycznie ładuję tylko kilka klas. Ważne jest, aby konkretna aplikacja była jak najmniejsza i w jak największym stopniu korzystała z gotowych pakietów - to bardzo ułatwia ponowne wykorzystanie.

Struktura projektu wygląda następująco:

```txt
/app
    Bootstrap.php <-- konfigurace
    /model
        UserForm.php <-- projektové třídy
        RegisterFactory.php
        ...
/vendor
    ... <-- knihovny
/www
    index.php <-- inicializace
```

Badanie z automatycznym ładowaniem
------------------------

Czasami może się zdarzyć, że nie wszystkie pliki zostaną załadowane i pojawią się problemy.

Do debugowania zalecam użycie funkcji <a href="/get-list-of-all-loaded-files">get_included_files()</a>.
