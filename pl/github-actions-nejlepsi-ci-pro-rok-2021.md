GitHub Actions - najlepsze CI na 2021 r.
========================================

> id: c1fe3b77-a506-4f20-bf1e-90ee82817c7d
> slug:
> 	cs: github-actions-nejlepsi-ci-pro-rok-2021
> 	pl: github-actions---najlepsze-ci-na-2021-r
> 
> perex:
> 	- 'Protože už nějaký čas vyvíjíte webové aplikace, určitě jste si všimli, že se pro vás celá řada věcí rutinně opakuje, i když by nemusela. Velmi často jde o technickou správu projektů, verzování souborů, automatická kontrola kódu, zpracování různých dat, nebo třeba deploy na server a bezpečnostní věci kolem toho.'
> 	- 'Jeśli tworzysz aplikacje internetowe już od jakiegoś czasu, zapewne zauważyłeś, że wiele rzeczy jest dla Ciebie rutynowo powtarzalnych, mimo że nie powinno tak być. Bardzo często jest to zarządzanie projektami technicznymi, wersjonowanie plików, automatyczna weryfikacja kodu, różne przetwarzanie danych, a także wdrażanie na serwer i kwestie związane z bezpieczeństwem.'
> 
> publicationDate: '2021-02-07 15:46:39'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: d3d151927a02dde744d1202449c904dc

Jeśli tworzysz aplikacje internetowe już od jakiegoś czasu, zapewne zauważyłeś, że wiele rzeczy jest dla Ciebie rutynowo powtarzalnych, mimo że nie powinno tak być. Bardzo często jest to zarządzanie projektami technicznymi, wersjonowanie plików, automatyczna weryfikacja kodu, różne przetwarzanie danych, a także wdrażanie na serwer i kwestie bezpieczeństwa.

Podczas konsultacji w firmach bardzo często spotykam się z problemem, że profilaktyka jest bardzo niedoceniana. Powodem jest często to, że programiści postrzegają niektóre rzeczy jako bardzo wymagające i że przysporzą im one dodatkowej pracy. Prawda jest jednak taka, że zazwyczaj wystarczy raz skonfigurować całą konfigurację, a następnie czerpać z niej długofalowe korzyści.

Co to jest CI (Continuous Integration)
----------

CI/CD to narzędzie, które pozwala zautomatyzować rutynowe zadania, które mają podobną podstawę i powtarzają się w projektach. Najczęściej CI wykorzystuje się do przeprowadzania testów automatycznych, przeglądu kodu, automatycznego wdrażania (wdrażania aplikacji na serwer WWW), zarządzania projektami lub audytów bezpieczeństwa.

CI należy traktować jako wirtualny system operacyjny, który wykonuje predefiniowane polecenia do Terminala lub uruchamia określone programy. Wynikiem działania CI jest sukces lub porażka, które są wyświetlane bezpośrednio w projekcie, ale także wysyłane do administratorów, którzy mogą na nie reagować. Dobrą praktyką jest uruchamianie zadań CI po wystąpieniu określonego zdarzenia (na przykład: commit do repozytorium, otrzymane żądanie scalenia/wyciągnięcia, tag/wersja/wydanie lub uruchomienie pętli czasowej crona).

Jak konkretnie działa CI
------------

Podstawą CI jest plik konfiguracyjny, w którym określamy jego zachowanie i reakcję na zdarzenia. W przypadku GitHuba wystarczy utworzyć katalog pomocniczy `.github/workflows` i utworzyć w nim dowolny plik z rozszerzeniem `.yml`. GitHub będzie je przetwarzał przy każdym commit'cie i wykonywał w razie potrzeby.

Wyobraźmy sobie, że chcemy zdefiniować nowy plik konfiguracyjny z określonym zadaniem. Ponieważ jest to zadanie, które będzie uruchamiane bezpośrednio przez GitHub lub inne środowisko na maszynie wirtualnej, musimy określić podstawowe rzeczy dotyczące środowiska, w tym:

- Nazwa zadania (na przykład nazwa testu)
- Zdarzenia wyzwalające (na podstawie jakiego zdarzenia ma być wyzwalane zadanie, np. za każdym razem, gdy przesyłasz do repozytorium lub otrzymujesz prośbę o ściągnięcie)
- Definicja samych zadań (może być wiele zadań działających równolegle)

W przypadku miejsc pracy definiujemy dalej:

- Nazwa stanowiska
- Środowisko uruchomieniowe (zwykle nazwa systemu operacyjnego)
- Etapy budowy pojemnika

Powyższy kontener jest już pierwszym przygotowaniem do **pełnej dockeryzacji projektu**. Podstawowe użycie CI jest stosunkowo proste i nie trzeba w ogóle rozumieć Dockera, wystarczy znać polecenia w Terminalu.

W ramach poszczególnych kroków w zadaniu należy uruchomić poszczególne polecenia, które utworzą instalację systemu operacyjnego, który właśnie zainstalowaliśmy. Tak więc w przypadku PHP ważne jest, aby zainstalować tylko język PHP (i określić jego wersję), sklonować repozytorium projektu do runnera, zainstalować zależności projektu (najczęściej [Composer](/composer)) i uruchomić sam test.

Dlaczego warto korzystać z GitHub Actions (CI)?
--------

W społeczności informatycznej panuje powszechny przesąd, że Microsoft to zła korporacja, która produkuje sprzęt niskiej jakości. W przypadku Akcji GitHub jest to jednak zupełnie nieprawdziwe, ponieważ obiektywnie rzecz biorąc, mają oni najlepszą platformę CI na świecie.

Podstawową zaletą GitHub CI jest prostota i elegancja notacji. Wystarczy kilka wierszy, aby skonfigurować własne środowisko testowe i zarządzać nim automatycznie, nawet bez wcześniejszej wiedzy. Jednocześnie za ogromną zaletę uważam szybkość przetwarzania konkretnych zadań. Na przykład test z zakresu codestyle (formatowanie kodu) i PhpStan (sprawdzanie typów danych, logiki i ogólnej poprawności) może być oceniony przez GitHub Actions na zwykłym projekcie w czasie poniżej 50 sekund, podczas gdy na przykład GitLab ocenia ten sam test w czasie prawie 8 minut! Inne rozwiązania, takie jak Travis, są stosunkowo drogie, a większość programistów i tak nie docenia korzyści płynących z ich specjalnych funkcji.

Przygotowane działania
-----

Na GitHubie znajduje się duża baza gotowych akcji i pakietów, z których można korzystać od razu. Zazwyczaj są to systemy operacyjne, języki programowania lub biblioteki pochodzące od społeczności.

Bazę danych akcji można znaleźć w [Marketplace](https://github.com/marketplace?type=actions), na przykład moją ulubioną akcją jest [Wyślij SMS w przypadku awarii za pośrednictwem Twillio](https://github.com/marketplace/actions/twilio-sms).

Gotowe przykłady
------

Bardzo wiele przykładów [publikuję we własnych repozytoriach](https://github.com/baraja-core), w których można przejrzyście zobaczyć, jakiej konkretnie konfiguracji używam.

Na przykład w lutym 2021 r. użyłem tej konfiguracji do sprawdzania stylu kodowania w projekcie:

```yaml
name: Coding Style

on:
  push:
    branches:
      - master
  pull_request:
    types: [ assigned, opened, synchronize, reopened ]
  schedule:
    - cron: '1 * * * *'

jobs:
  nette_cc:
    name: Nette Code Checker
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 7.4
          coverage: none

      - run: composer create-project nette/code-checker temp/code-checker ^3 --no-progress
      - run: php temp/code-checker/code-checker --strict-types --no-progress

  nette_cs:
    name: Nette Coding Standard
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: shivammathur/setup-php@v2
        with:
          php-version: 8.0
          coverage: none

      - run: composer create-project nette/coding-standard temp/coding-standard ^3 --no-progress --ignore-platform-reqs
      - run: php temp/coding-standard/ecs check src
```

Ten plik konfiguracyjny instaluje najnowsze Ubuntu (`runs-on: ubuntu-latest`), klonuje repozytorium projektu (`uses: actions/checkout@v2`), instaluje PHP (`uses: shivammathur/setup-php@v2`) w wersji 7.4 (`php-version: 7.4`), instaluje pakiet code-checker, a następnie uruchamia zadanie za pomocą PHP.

Jeszcze kilka uwag dla użytkowników, którzy nie są tak doświadczeni:

- CI przy każdym uruchomieniu uruchamia maszynę wirtualną z kompletnym systemem operacyjnym, który może być używany do wywoływania poleceń w Terminalu, tak jak np. serwer
- Do sprawdzenia wyników testu używane są tzw. kody powrotu zdefiniowane przez samego Linuksa. W szczególności, gdy program zwraca `0` (zero), oznacza to sukces, a każda inna liczba oznacza porażkę. Na przykład skrypty powłoki działają w ten sam sposób
- Gdy chcemy napisać własny test lub bardziej złożoną logikę, możemy użyć na przykład PHP i uruchomić go jako zadanie CLI (polecenie `php file.php`), które zostanie uruchomione i będzie mogło robić wszystko, do czego ma prawa (pracować z plikami, komunikować się przez Internet, wyświetlać dane na ekranie, ...).
- Podczas pracy CI wszystkie dane wyjściowe są rejestrowane na ekranie i mogą być przeglądane bezpośrednio w przeglądarce internetowej.
- CI nie jest interaktywny, mimo że Terminal może wykonywać interaktywne operacje z użytkownikiem, CI nie obsługuje tego i musi być zaprojektowany jako zadanie, które czasem się zaczyna, a czasem kończy
- Dla kontenera CI zdefiniowano limit czasu działania wynoszący 1 godzinę, jeśli w tym czasie nie wszystko zostanie przetworzone (np. program się zatrzyma), cały system zostanie przymusowo zamknięty i zostanie zgłoszony błąd
- GitHub może uruchamiać zadania CI automatycznie przez crona, jednak bardzo często nie uruchamia ich w odpowiednim czasie, lecz z opóźnieniem
- Można również zawinąć całą logikę CI w kontener Docker i uruchomić go lokalnie na wszystkich maszynach lub na serwerze, np.
- Jeśli potrzebujesz dostępu do tajnych informacji (np. hasła do bazy danych), istnieje możliwość zdefiniowania tajnych zmiennych bezpośrednio w GitHubie, a CI udostępni je w czasie pracy
- Wdrażanie na serwer WWW zawsze najlepiej przeprowadzać przez SSH
- Całkowity czas działania zadań CI jest ograniczony, zwykle do 2 tys. minut miesięcznie (bardzo często jest to wystarczająca ilość, ja nigdy go nie wykorzystałem, ponieważ jedno zadanie działa maksymalnie przez minutę), można też odpłatnie wydłużyć ten czas
- Możesz również hostować runnery CI na swoim serwerze i ominąć limit minut, ale bardzo prawdopodobne jest, że Twój serwer nie będzie dużo szybszy, ponieważ Microsoft zainwestował dużo w Azure, gdzie hostowany jest GitHub Actions i działa on na bardzo wydajnych serwerach.
- Bardzo często przydaje się instalacja specyficznych pakietów tylko dla CI (zazwyczaj PhpStan i różne zabezpieczenia), więc umieść je w sekcji `composer.json` pliku `require-dev`, aby nie zostały zainstalowane w konkretnym projekcie jako obowiązkowa zależność

Aplikacje i boty GitHuba
-----

W przeciwieństwie do innych repozytoriów GitHub obsługuje tzw. aplikacje GitHub Apps i boty automatyzujące. Jest to duża baza gotowych botów, które mogą wykonywać zautomatyzowane operacje na Twoich projektach (nawet bez CI) i kiedy coś napotkają, na przykład zgłoszą błąd lub wyślą pull request z poprawką.

Osobiście z niego korzystam i polecam go Wam:

- Dependabot](https://dependabot.com) - automatyczne sprawdzanie zależności w Composerze, na przykład, gdy pojawi się nowa wersja pakietów, których używasz i które są kompatybilne, automatycznie wysyła prośbę o ściągnięcie (pull request)
- Codecov](https://github.com/marketplace/codecov) - sprawdza pokrycie kodu testami automatycznymi i wyświetla wykresy, które części kodu nie zostały jeszcze pokryte.
- [Snyk](https://github.com/marketplace/snyk) - automatyczne sprawdzanie luk w zabezpieczeniach
- [Imgbot](https://github.com/apps/imgbot) - automatyczna optymalizacja rozmiaru danych obrazu

Wiele innych aplikacji można znaleźć w witrynie [Marketplace](https://github.com/marketplace?type=apps).

Więcej przydatnych porad i wskazówek
-----

Bardzo polubiłem korzystanie z zadań automatyzacji w GitHubie.

We wszystkich moich repozytoriach mam ustawione automatyczne kontrole, więc kiedy wysyłam dowolny commit (lub ktokolwiek inny w społeczności), dostaję w czasie rzeczywistym informację zwrotną, czy jakość kodu (codestyle i PhpStan), bezpieczeństwo i inne elementy zostały naruszone.

Niektóre testy są uruchamiane automatycznie co godzinę. Na przykład, jeśli wystąpi luka w zabezpieczeniach lub CVE, będę o tym wiedział najpóźniej w ciągu godziny, nawet na poziomie konkretnych projektów i tego, co dokładnie się wydarzyło.

Z biegiem czasu, po przetestowaniu różnych konfiguracji, doszedłem do wniosku, że za idealną minimalną konfigurację uważam następujące zależności od Composera:

- `phpstan/phpstan` - sprawdzanie poprawności i logiczności kodu
- `tracy/tracy` - raportowanie i rejestrowanie błędów wysokiego poziomu
- `phpstan/phpstan-nette` - rozszerzenia i specjalizacje Phpstan w Nette
- `spaze/phpstan-disallowed-calls` - genialna biblioteka autorstwa Michała Spacka, która zajmuje się (nie)używaniem niebezpiecznych rzeczy w kodzie (od zapomnianych zrzutów zmiennych do możliwości uruchomienia łańcucha użytkownika jako polecenia powłoki)
- `roave/security-advisories` - sprawdza instalację niebezpiecznych wersji pakietów (jeśli w danym pakiecie zostanie znaleziona luka w zabezpieczeniach lub zostanie opublikowane CVE, pakiet ten uniemożliwi instalację uszkodzonej wersji)

Najlepiej byłoby, gdyby podstawowa konfiguracja zabezpieczeń była ustawiona na automatyczne uruchamianie co najmniej co 8 godzin i wysyłanie błędów na adres e-mail. Ponieważ za każdym razem, gdy instalacja projektu nie powiedzie się lub zostanie odkryta nowa luka w zabezpieczeniach, chcę o tym jak najszybciej wiedzieć i natychmiast reagować.

Pamiętaj, że wszystko działa automatycznie, więc możesz mieć pewność, że nawet jeśli zastosujesz skomplikowane procesy sprawdzania zabezpieczeń i innych rzeczy, nie będzie Cię to kosztowało dodatkowego wysiłku, a wystarczy tylko dobrze przeprowadzona konfiguracja wstępna.

Ponieważ w zapobieganiu i automatyzacji tkwi ogromny potencjał!
