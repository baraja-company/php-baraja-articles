Kompozytor - pełny przegląd zaawansowanych funkcji
==================================================

> id: a74d8d59-91ce-4602-ad52-80cf89a647bd
> slug:
> 	cs: composer
> 	pl: kompozytor---pelny-przeglad-zaawansowanych-funkcji
> 
> perex:
> 	- Composer je pokročilý správce balíků a závislostí pro vaše PHP aplikace. Článek popisuje jeho všechny výhody a možnosti použití.
> 	- Composer jest zaawansowanym menedżerem pakietów i zależności dla Twoich aplikacji PHP. W tym artykule opisano wszystkie jego zalety i zastosowania.
> 
> publicationDate: '2020-03-10 20:18:19'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '68340d4b4d3c8a6ed143ede176fbf04e'

Jak już wiesz, [Composer](https://getcomposer.org/) to zaawansowany menedżer pakietów i zależności dla PHP, dzięki któremu możesz w elegancki sposób zarządzać setkami projektów jednocześnie i rozprowadzać raz napisany kod do wszystkich aplikacji jednocześnie.

Niniejszy poradnik stanowi szczegółowy i wyczerpujący przewodnik dla programistów. Omówimy wszystkie ważne zaawansowane techniki pracy z programem Composer, a także wyjaśnimy szczegóły techniczne i odpowiednie zależności.

Instalacja programu Composer
-------------------

Niezależnie od platformy, pobieramy je z [oficjalnej strony Composera](https://getcomposer.org/).

Wewnętrznie pobierany jest plik PHP `composer-setup.php`, który po uruchomieniu w trybie CLI może zainstalować Composera. Ważne jest również, aby wiedzieć, że Composer nie działa bez PHP, dlatego najpierw należy sprawdzić, czy na komputerze jest uruchomione PHP (musi być dostępne tylko dla Terminala).

W systemach Mac i Linux Composer działa zaraz po instalacji i wystarczy wywołać polecenie `composer -v`, aby szybko sprawdzić, czy Composer został zainstalowany poprawnie.

W systemie Linux do instalacji można użyć następującego polecenia: `/usr/bin/php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"`.

W systemie Windows warto zainstalować narzędzie [Git Bash for Windows](https://gitforwindows.org/), które pozwala otworzyć specjalny Terminal zachowujący się prawie jak Linux i umożliwiający pracę w takim samym środowisku jak w systemie Linux.

Instalacja na serwerach przebiega tak samo jak w środowisku lokalnym. Upewnij się tylko, że masz odpowiednią wersję PHP, z której wewnętrznie korzysta Composer.

Dostępne polecenia
----------------

W programie Composer zaimplementowano wiele poleceń.

Używa się polecenia: `composer <polecenie>`, na przykład: `composer update`.

Przegląd dla `1.10.0`:

| Polecenie | Opis / Znaczenie |
|-----------------------|----------------|
| `about` | Wyświetla krótkie informacje o Composerze. |
| `archiwum` | Tworzy archiwum z zawartością wybranego pakietu Composer. |
| `przeglądaj` | Otwiera stronę główną wybranego pakietu, autora lub inną powiązaną stronę w przeglądarce internetowej. Często może zawierać dokumentację dotyczącą korzystania z niego. |.
| `cc` | Czyści wewnętrzną pamięć podręczną Composera z wersjami pakietów pobranych w przeszłości. |
| `check-platform-reqs` | Sprawdź, czy wymagania instalacyjne są spełnione dla bieżącej platformy. |
| Czyszczenie pamięci podręcznej wewnętrznej Composera. |
| Czyszczenie wewnętrznej pamięci podręcznej kompozytora. |
| | `config` | Ustawia dyrektywę konfiguracyjną.
| | Tworzy nowy projekt na podstawie wybranego pakietu i automatycznie tworzy folder, w którym zostanie umieszczony projekt.
| `depends` | Pokazuje, które pakiety spowodowały, że wybrany pakiet został zainstalowany. |
| `diagnose` | Diagnozuje system w celu wykrycia typowych błędów. Przetwarzanie danych wyjściowych zależy od programisty, jest to tylko lista. |
| `dump-autoload` | Generuje nowy <a href="/autoloading-trid">autoloader</a>. |
| `dumpautoload` | Generuje nowy <a href="/autoloading-trid">autoloader</a>. |
| `exec` | Wykonuje pliki binarne i skrypty z Vendora. |
| Jak stworzyć/modyfikować swoje zależności?
| `global` | Umożliwia uruchamianie globalnych poleceń Composera ze zmiennej `$COMPOSER_HOME`. |
| | `help` | Drukuje pomoc dla poleceń. |
| `home` | Otwiera stronę główną określonego pakietu w przeglądarce. |
| `i` | Instaluje wszystkie zależności projektu zgodnie z plikiem `composer.lock`, jeśli istnieje i jest poprawny. Jeśli wystąpi problem, używane są informacje z pliku `composer.json`, a `composer.lock` jest przywracany do pierwotnego stanu. |
| `info` | Wyświetla informacje o aktualnie zainstalowanych pakietach w projekcie. Wyświetla nazwy wszystkich pakietów, ich aktualne wersje i krótki opis. |
| `init` | Tworzy podstawową funkcję `composer.json` w bieżącym katalogu. |
| `install` | Instaluje wszystkie zależności projektu zgodnie z plikiem `composer.lock`, jeśli istnieje i jest poprawny. Jeśli wystąpi problem, użyta zostanie informacja z pliku `composer.json`, a `composer.lock` zostanie przywrócony do pierwotnego stanu. |
| |licenses` | Wyświetla listę wszystkich pakietów, ich wersji i aktualnej licencji. |
| `list` | Wyświetla listę dostępnych poleceń. |
| | | Nieaktualne` | Wyświetla listę wszystkich pakietów, dla których dostępna jest nowsza wersja do zainstalowania i które spełniają określone zależności. Dla każdego pakietu zostanie wyświetlona najnowsza kompatybilna wersja, którą Composer proponuje do zainstalowania. |
| `zakazuje` | Pokazuje, które pakiety i zależności uniemożliwiają instalację żądanego pakietu lub wersji. |
| `remove` | Usuwa pakiet z sekcji konfiguracji `require` lub `require-dev`. |
| `require` | Dodaje żądany pakiet do pliku `composer.json` i instaluje go. Jeśli nie będzie można spełnić tych zależności, powróci do stanu pierwotnego. |
| `run` | Uruchamia skrypty zdefiniowane w pliku `composer.json`. |
| `run-script` | Uruchamia skrypty zdefiniowane w pliku `composer.json`. |
| Szukaj` | Wyszukuje pakiety według słowa kluczowego lub zapytania. |
| self-update` | Aktualizuje wewnętrzny plik `composer.phar` do najnowszej wersji. |
| selfupdate` | Aktualizuje wewnętrzny plik `composer.phar` do najnowszej wersji. |
| `show` | Wyświetla szczegółowe informacje o aktualnie zainstalowanych pakietach.
| `status` | Wyświetla podsumowanie lokalnych zmian dokonanych w pakietach ręcznie, w oparciu o porównanie ze źródłem pakietu, z którego został on pierwotnie zainstalowany. |
| `suggests` | Wyświetla sugestie dotyczące pakietów. Sugestie mogą obejmować różnego rodzaju działania, takie jak instalowanie aktualizacji zabezpieczeń itp.
| `update` | Aktualizuje cały projekt zgodnie z zależnościami, tak aby były one zawsze spełnione przez `composer.json`. Jeśli się powiedzie, aktualizuje on plik `composer.lock`, w którym zapisuje aktualnie zainstalowane wersje. |
| `upgrade` | Alias do `update`. |
| `u` | Alias do `update`. |
| `validate` | Sprawdza pliki `composer.json` i `composer.lock` pod kątem błędów składni. |
| `why` | Pokazuje, które pakiety spowodowały zainstalowanie aktualnie wybranego pakietu, łącznie ze wszystkimi zależnościami. |
|, `why-not` | Wyświetla, które pakiety i wersje uniemożliwiają instalację wybranego pakietu lub wersji. |

Tworzenie i definiowanie projektu
----------------------------------

Każdy projekt zarządzany przez Composera jest definiowany przez plik `composer.json` znajdujący się w jego korzeniu, który określa wszystkie zależności. Plik ten można utworzyć ręcznie dla istniejącego projektu lub automatycznie podczas tworzenia projektu.

Ponieważ wszystko w Composerze jest pakietem, sam projekt może być oparty na pakiecie. Jeśli więc na przykład tworzysz dziesiątki lub setki bardzo podobnych projektów, sensowne jest umieszczenie ich podstawowej konfiguracji i struktury w osobnym pakiecie, na którym zostanie oparta instalacja.

Przykładem takiego pakietu jest mój [Baraja Sandbox](https://github.com/baraja-core/sandbox), który jest oparty na czystym Nette 3.0 i dodaje podstawową zależność do mojego [Menedżera pakietów](https://github.com/baraja-core/package-manager), którego używam do zarządzania wszystkimi projektami i zależnościami w konfiguracji Nette.

Piaskownicę instaluje się wtedy po prostu za pomocą polecenia:

```shell
$ composer create-project baraja/sandbox <your-project-name>
```

Na podstawie nazwy projektu Composer automatycznie utworzy folder, w którym zostanie zainstalowany projekt (skopiuje zawartość pakietu i zainstaluje zależności).

W folderze `vendor` Composer zarządza wszystkimi pakietami (ich fizyczne pliki znajdują się tam) i generuje autoload klas, który najlepiej umieścić bezpośrednio w `index.php` jako linię:

```php
require __DIR__ . '/vendor/autoload.php';

// Sam kod aplikacji
```

Instalowanie dodatkowych pakietów i zależności
-------------------------------------

W ramach projektu funkcjonalnego możemy w bardzo prosty sposób instalować nowe pakiety i dodawać zależności.

Można to zrobić na dwa sposoby:

- Za pomocą polecenia `composer require ...`,
- Dodając zależność bezpośrednio do pliku `composer.json` w sekcji `require`, a następnie używając polecenia `composer update`.

Spróbuj zainstalować na przykład PackageManager: `composer require baraja-core/package-manager`, lub [Doctrine](https://github.com/baraja-core/doctrine): `composer require baraja-core/doctrine`.

Jeśli wybrany pakiet nie może zostać zainstalowany, można podać konkretne powody, a Composer wyświetli listę zależności, które to uniemożliwiają. Często wystarczy naprawić zależność od konkretnej wersji lub usunąć niekompatybilny kod. Aby uzyskać szczegółowe informacje, należy użyć polecenia: `composer why baraja-core/doctrine`.

Aktualizowanie projektów i pakietów
-----------------------------

Dobrze opracowany projekt jest tworzony w taki sposób, aby można było łatwo pobierać aktualizacje z biegiem czasu i zawsze mieć najnowsze wersje wszystkich pakietów. Główną zaletą jest to, że naprawiane są wszystkie błędy, często poprawiana jest wydajność i dodawanych jest wiele nowych funkcji. Ponadto stopniowe przechodzenie na nowy system sprawi, że po dłuższym czasie aktualizacja będzie mniej skomplikowana, ponieważ problemy będą usuwane na bieżąco na mniejszą skalę, co pozwoli uniknąć niezgodności.

Aby zaktualizować wszystkie pakiety i zależności, użyj polecenia `composer update`.

W niektórych przypadkach sam proces aktualizacji może zakończyć się niepowodzeniem. Powodem jest zazwyczaj uszkodzona zależność lub niekompatybilny pakiet.

Aby uzyskać szczegółowe informacje o tym, dlaczego pakiet nie może zostać zainstalowany, użyj polecenia: `composer why-not baraja-core/doctrine`. Jeśli mamy już pakiet, a tylko określona wersja nie działa (nie chce się zainstalować), możemy również poprosić o konkretną wersję: `composer why-not baraja-core/doctrine:v1.0.20`.

W pliku `composer.json` możemy również wyszczególnić zależność od konkretnego runtime'u. Jest to szczególnie przydatne, gdy projekt jest realizowany przez kilka osób i chcemy sprawdzić, czy mają one zainstalowane wszystkie rozszerzenia.

Zazwyczaj sprawdzana jest wersja PHP (musi być `7.1.0` lub nowsza):

```json
{
   "require": {
      "php": ">=7.1.0"
   }
}
```

Ewentualnie inne rozszerzenia systemu:

```json
{
   "require": {
      "php": ">=7.1.0",
      "ext-json": "*",
      "ext-session": "*",
      "ext-PDO": "*"
   }
}
```

Reguły te są następnie uwzględniane podczas instalowania pakietów lub uaktualnień. Pozwala to uniknąć problemów, które mogłyby się ujawnić w czasie pracy. Zazwyczaj, na przykład, pakiet bramy płatniczej musi komunikować się z API, więc musi przyznać zależności od rozszerzeń `curl` i `json`.

Rozwiązywanie problemów z zależnościami
-----------------------------

Często naruszenia zależności występują z powodu złej wersji PHP. W takim przypadku Composer wyrzuca komunikat, na przykład `twoja wersja PHP (7.3.11) nadpisana przez wersję "config.platform.php" (7.1) nie spełnia tego wymagania.`.

Bardzo często błąd jest spowodowany ustawieniami bezpośrednio w pliku projektu `composer.json`, w którym znajduje się następująca sekcja:

```json
"config": {
   "platform": {
      "php": "7.2"
   }
}
```

Zmiana **musi być wprowadzona bezpośrednio w pliku**. W przypadku projektu globalnego (przed instalacją lub z globalną zależnością), można wymusić wersję Composera za pomocą `composer config -g platform.php 7.2.14` (przełącznik `-g` oznacza `global`).

W niektórych przypadkach chcemy zainstalować najnowsze wersje pakietów i zignorować ustawienia środowiska lokalnego. W takim przypadku można skorzystać z polecenia zaawansowanego:

```shell
$ composer update --ignore-platform-reqs
```

**Używaj przełącznika `ignore-platform-reqs` na własne ryzyko, może on spowodować uszkodzenie projektu!** Nie używaj go, jeśli nie rozumiesz jego konsekwencji.

Ręczne wywołania kompozytora, parametry i zarządzanie pamięcią
------------------------------------------------------

Composer jest w rzeczywistości skryptem PHP opakowanym w tzw. PHAR, czyli skompilowaną wersję aplikacji PHP. Znając te informacje, można je dobrze wykorzystać, na przykład do lepszego sparametryzowania samego wywołania.

W naprawdę dużych projektach czasami zdarza się, że zabraknie nam pamięci RAM i musimy przeznaczyć na nią znacznie więcej lub zwiększyć czas przetwarzania skryptów.

Na przykład w systemie Windows można w tym celu użyć tego polecenia:

```shell
$ php -d memory_limit=-1 C:/xampp/htdocs/composer.phar update
```

Przełącznik `memory_limit=-1` powoduje, że Composer nie jest wrażliwy na limit pamięci RAM i zużywa tyle pamięci, ile potrzebuje.

Niestandardowe skrypty użytkownika po wykonaniu akcji kompozytora
--------------------------------------------

Po uruchomieniu akcji programu Composer można wywołać automatyczne wykonanie skryptów zdefiniowanych przez użytkownika, które wykonują określone czynności w projekcie lub na przykład generują konfigurację po wdrożeniu. Używam tego podejścia głównie na serwerze lokalnym, aby udostępnić narzędzie do automatycznej konfiguracji bazy danych, generowania schematu bazy danych itp.

Wymagane skrypty rejestrujemy w sekcji `scripts` pliku `composer.json`:

```json
"scripts": {
   "post-autoload-dump": "Baraja\\PackageManager\\PackageRegistrator::composerPostAutoloadDump"
}
```

W tym przypadku statyczna metoda `composerPostAutoloadDump` w klasie `BarajaPackageManager` jest wywoływana automatycznie. Composer ma tę klasę dostępną, ponieważ jako pierwszy wykonał generowanie <a href="/autoloading-trid">klasy autoloadera</a>.

Jeśli chcemy po prostu uruchomić skrypty i nie wykonywać zbędnych czynności w Composerze, bardzo przydatna jest komenda `composer dump`, która po prostu generuje nowy autoloader (co zalecam, aby zawsze był aktualny), a następnie od razu uruchamia skrypty. Jeśli chcesz spróbować użyć skryptów, przygotowałem gotowy pakiet [Baraja PackageManager](https://github.com/baraja-core/package-manager), który implementuje inteligentne skrypty i interaktywny interfejs dla frameworka Nette.

Wersjonowanie Vendora w Git?
------------------------

Pytanie, które często omawiamy z programistami, dotyczy tego, czy należy wersjonować zawartość folderu `vendor` do Git, czy też generować go na nowo przy każdej instalacji.

Ogólnie rzecz biorąc, czystszym rozwiązaniem wydaje się być **nie wersjonowanie** zawartości w ogóle i instalowanie wszystkiego za każdym razem. W rzeczywistości jednak od czasu do czasu programista przestaje rozwijać pakiet lub całkowicie go usuwa. Ciągłe pobieranie pakietów komplikuje również lokalne instalacje i aktualizacje, a także spowalnia wdrożenia i może czasami powodować krótkie przerwy w działaniu witryny, gdy nowe wersje pakietów są pobierane nieprawidłowo.

Zawieszenie Vendora postrzegam jako formę "bezpieczeństwa". Jeśli pliki znajdują się fizycznie w systemie wersjonowania, to na serwerze produkcyjnym mamy przynajmniej podstawową pewność, że pakiety będą działać, a ich kod jest dokładnie taki sam, jak w lokalnie działającej instalacji. Ponadto Vendor często zajmuje tylko kilka MB, co przy dzisiejszych pojemnościach dysków z pewnością jest warte zapewnienia działającej witryny.

**Praktyczna uwaga:** Dla przeciętnej strony, którą utrzymuję, `vendor` zajmuje nie więcej niż `30 MB`, co jest akceptowalną pojemnością dla Git. Klonując repozytorium z juniorem, nie musimy go szkolić, jak uruchomić witrynę, i po prostu "od razu działa".

Niestandardowe pakiety kompozytora
-----------------------

W Kompozytorze można tworzyć własne pakiety, zarówno publicznie dostępne (zarejestrowane na stronie [Packagist](https://packagist.org/)), jak i prywatne (trzeba mieć własny serwer pakietów, na przykład [Satis](https://getcomposer.org/doc/articles/handling-private-packages.md)).

Kwestia tworzenia, utrzymywania, rozwijania i wersjonowania pakietów jest bardzo złożona i będzie tematem osobnego artykułu.

W międzyczasie można przeczytać artykuł [Semantic Versioning] (https://semver.org/lang/cs/), który dla Was przetłumaczyłem.
