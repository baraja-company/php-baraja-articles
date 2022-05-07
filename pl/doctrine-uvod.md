Seria Doktrynalna - Wprowadzenie
================================

> id: fbd0461a-53fe-4713-8926-82e31bd1fb9b
> slug:
> 	cs: doctrine-uvod
> 	pl: seria-doktrynalna---wprowadzenie
> 
> publicationDate: '2021-08-27 11:40:00'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: '2bc62308fcb66acda5a09ac95b5eb2c2'

Doctrine jest zaawansowaną biblioteką PHP służącą do obiektowej pracy z bazami danych. Głównym zadaniem i celem Doctrine jest opisanie schematu bazy danych za pomocą encji danych i manipulowanie danymi w sposób w pełni obiektowy.

Paradygmat ten nosi nazwę ORM (Object-relational mapping) i jest [wzorcem projektowym](/design-patterns) służącym do przekształcania (opakowywania) danych przechowywanych w relacyjnej bazie danych w obiekt, który może być używany w języku obiektowym. Dlatego, aby zrozumieć i używać Doctrine, musisz znać przynajmniej podstawy [Object-Oriented Programming](/oop).

Dlaczego warto uczyć się doktryny?
------------------------

Jest wiele powodów:

- Doctrine jest najczęściej stosowaną bazą ORM, używaną przez większość zaawansowanej społeczności PHP.
- Znacznie upraszcza projektowanie aplikacji PHP
- Zapewniasz spójny sposób projektowania, wersjonowania, przenoszenia i tworzenia kopii zapasowych schematu bazy danych.
- Możesz [uzyskać wiele tabel bazy danych, pobierając pakiet](https://github.com/baraja-core/shop-product) bez konieczności wymyślania i konfigurowania czegokolwiek.
- Relacje między tabelami stają się rzeczywistymi bytami fizycznymi
- Dane wyjściowe bazy danych nie będą zwykłymi tablicami bez atrybutów, lecz prawdziwymi obiektami fizycznymi
- Dzięki temu można łatwo wykonywać wiele operacji jednocześnie w ramach jednej transakcji
- W prosty sposób można zwiększyć bezpieczeństwo i odporność aplikacji, po prostu wiedząc, kiedy co się dzieje i czy dzieje się to bezpiecznie.
- Uzyskasz łatwo testowalny kod i warstwę bazy danych
- Odkryjesz cały ekosystem wokół Doctrine, który w elegancki sposób rozwiązuje wiele problemów. Często można znaleźć proste rozwiązania złożonych problemów, które w innych warunkach byłyby prawie niemożliwe do rozwiązania.
- Dowiesz się wielu nowych rzeczy, poznasz nowe pomysły i wykorzystasz w pełni potencjał bazy danych.
- Pozbądź się skomplikowanych zapytań SQL. Doctrine udostępnia interfejs pisania własnych zapytań (DQL), który jest bardzo rozbudowany
- Aplikacje będą działać szybciej. Z łatwością odkryjesz możliwości optymalizacji aplikacji, wykorzystania leniwego ładowania i znalezienia wąskich gardeł aplikacji.

Autor tego artykułu ([Jan Barasek](https://baraja.cz)) od dawna uważa, że Doctrine jest najlepszym sposobem pracy z bazą danych w PHP. Po prostu nie ma konkurencji.

Jak zacząć?
----------

Zanim zaczniesz w pełni korzystać z Doctrine, musisz przygotować odpowiednie środowisko. Jeśli dopiero zaczynasz swoją przygodę z PHP lub nie masz dużej wiedzy na ten temat, najlepszym wyborem jest zainstalowanie Nette Framework z pakietem rozszerzeń [Baraja Doctrine](https://github.com/baraja-core/doctrine), który automatycznie zapewnia pełną obsługę. Najpierw należy pobrać pakiet przez [Composer](/composer), następnie skonfigurować rozszerzenie DI, a Doctrine zacznie działać automatycznie.

Aby Doctrine działał poprawnie, należy przygotować pustą bazę danych (Doctrine może pracować z istniejącym projektem, ale w pierwszych krokach nie jest to wskazane, gdyż grozi nadpisaniem istniejących danych) i skonfigurować połączenie. Ponieważ Doctrine nie jest tylko biblioteką bazodanową, ale stanowi zaawansowany framework bazodanowy, należy [rozwiązać inną konfigurację](/configure-connections-with-baraja-doctrine). Większość ustawień jest automatycznie nadpisywana w tym pakiecie dla Nette, jednak w minimalnej konfiguracji Twój serwer musi obsługiwać rozszerzenia `APCu Cache` lub `SQLite3`.

Jeśli wszystko zostało skonfigurowane prawidłowo, w Nette zostanie utworzona nowa usługa DI `Baraja\\EntityManager`, którą można [wstrzyknąć](https://doc.nette.org/cs/3.1/di-usage) do programu Presenter:

```php
namespace App\FrontModule\Presenters;

use Baraja\Doctrine\EntityManager;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

Jeśli uda Ci się wstrzyknąć podstawową usługę EntityManager, możesz rozpocząć naukę i pracę z Doctrine.

Jak postępować?
--------

Kolejne rozdziały są połączeniem przewodnika po technologii Doctrine, wieloletniego doświadczenia, wzorców projektowych i gotowych rozwiązań. Wspólnie przejdziemy przez wszystkie podstawowe elementy Doctrine, od definiowania niestandardowej encji, przez generowanie schematu fizycznej bazy danych, po pracę z narzędziem do wersjonowania i wdrożenia produkcyjnego.

Używam Doctrine od bardzo dawna i rozwiązałem w nim tysiące spraw. Przedstawimy wskazówki, jak wykorzystać Doctrine do optymalizacji szybkości działania bazy danych oraz jak odpowiednio zaprojektować bazę danych. Doctrine można także wykorzystać w istniejącym projekcie (jeśli spełnione są pewne warunki). Poniżej pokażemy, jak to zrobić.

Ta seria artykułów została stworzona, aby pomóc moim kursantom w prowadzeniu szkoleń i konsultacji. W celu bardziej szczegółowego omówienia lub wyjaśnienia niektórych tematów można wysłać do mnie e-mail na adres jan@barasek.com. Ponieważ jest to stosunkowo wymagająca technologia, wszystkie pytania będą traktowane jako płatne konsultacje.
