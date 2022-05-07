Algorytm wyszukiwarki internetowej - indeksowanie i kanonizacja
===============================================================

> id: daa97e66-e77f-4741-ade2-905c1cfdbd56
> slug:
> 	cs: vyhledavac-indexace-a-kanonizace
> 	pl: algorytm-wyszukiwarki-internetowej---indeksowanie-i-kanonizacja
> 
> publicationDate: '2016-09-11 11:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d73a30f413e4b8b77dbceda60b3cf173

W dzisiejszej lekcji zajmiemy się indeksowaniem i kanonikowaniem dokumentów w Internecie.

Indeksowanie
--------

Proces indeksowania jest wykonywany przez komponent zwany indekserem. Jest to specjalnie zaprojektowany program, który przekształca pobrane dane (dane, które zostały pobrane przez program Crawler) w specjalny typ danych do wyszukiwania - beczki.

Problem z indeksowaniem polega na tym, że nie można "inteligentnie" przeglądać dokumentów, ale czytanie sekwencyjne (czytanie całego tekstu od początku do końca) jest nieuniknione, więc jest to wymagająca dyscyplina, a wyszukiwarki wykorzystują do tego zadania najpotężniejsze serwery. Żadne inne zadanie w procesie wyszukiwania nie jest tak wymagające jak indeksowanie, w którym zwykły tekst staje się indeksem.

Weźmy na przykład stronę o kotach, pobraną z Wikipedii. Indekser otrzymuje pełny tekst strony i musi usunąć niepotrzebne elementy (np. menu kontroli użytkownika, reklamy, stopki, ...) i przetworzyć stronę, aby uzyskać zwykły tekst. Na przykład, tekst może brzmieć:

> Kot domowy (Felis silvestris f. catus) jest udomowioną formą dzikiego kota, który towarzyszy człowiekowi od tysięcy lat. Podobnie jak jego dziki krewniak, należy on do podrodziny małych kotów i jest typowym przedstawicielem tej grupy. Ma smukłe i muskularne ciało, doskonale przystosowane do polowania, ostre pazury i zęby oraz doskonały wzrok, słuch i węch.

Fragment tekstu z [Wikipedii](http://cs.wikipedia.org/wiki/Ko%C4%8Dka_dom%C3%A1c%C3%AD).

Wyszukiwarka wyodrębnia teraz poszczególne słowa z tekstu i zapisuje je w pojedynczych beczkach. Nie może jednak zapisywać beczek bezpośrednio (głównie dlatego, że każda beczka istnieje w wielu kopiach, a także dlatego, że przeszkadzałoby to w wyszukiwaniu), więc tworzy listę wymagań dotyczących wyglądu przyszłej beczki i zapisuje ją w pamięci tymczasowej. W naszym przypadku taka lista może wyglądać następująco (niektóre nieistotne słowa można zignorować lub oznaczyć jako stopwords):

| Identyfikator dokumentu | Słowo | Pozycja | Typ |
|--------------|-------|--------|-----------|
| 1 | Kot | 1 | Normalny |
| 1 | Krajowy | 2 | Normalny |
| 1 | Felis | 3 | Normalny |
| 1 | Jest | 7 | StopWord |
| 1 | Udomowiony | 8 | Normalny |

Taka lista jest ogromna (znacznie większa niż oryginalne teksty), ale mimo to jest szybsza w wyszukiwaniu, ponieważ nie musimy czytać wszystkich tekstów po kolei, lecz możemy wyszukiwać według indeksu w każdej kolumnie, a słowa z jednej strony są posortowane w rzędach jedno po drugim.

Po upływie pewnego czasu (lub po skompletowaniu pewnej liczby dokumentów) indeksator przestaje pracować nad budową tej listy żądań (dla przyszłej beczki) i zaczyna ponownie odczytywać dane i odbudowywać poszczególne beczki (lista ta zawiera stare rekordy, które znajdują się w już działającej beczce). W przypadku dodania nowych rekordów ze znanych adresów proces ten spowoduje ich aktualizację, a w przypadku nowych dokumentów - uwzględnienie ich.

W ten sposób indekser ponownie przejdzie przez listę i powoli utworzy nowe beczki zawierające wszystkie elementy (będą one wyglądać tak samo, jak pokazano w przykładzie w poprzednich rozdziałach), a gdy wszystkie beczki zostaną skompletowane, zostaną wysłane do poszczególnych serwerów wyszukiwania. Aktualizacja beczek na serwerach jest czasochłonna (głównie ze względu na ogromną ilość przenoszonych danych), dlatego podczas tej operacji serwery nie są dostępne, a dane są aktualizowane na różnych serwerach w różnym czasie. Powoduje to na przykład, że niektórzy użytkownicy mogą otrzymywać różne wyniki, ponieważ każdy z nich szuka na innym serwerze wydającym (z powodu rozkładu obciążenia). Po zakończeniu aktualizacji wszystko wraca do normy, a wszyscy użytkownicy znajdują wszystkie dokumenty tak samo.

Proces indeksowania jest ważny dla każdej wyszukiwarki, a ta, która robi to najczęściej i najdokładniej, ma najbardziej aktualny obraz Internetu. Google wykonuje tę operację co kilka godzin, Seznam raz w tygodniu (i ma milion razy mniej danych).

Kanonizacja dokumentów
--------------------

W pierwotnym projekcie wyszukiwarki pełnotekstowej nie było potrzeby stosowania czegoś takiego jak kanonizacja, ponieważ Internet był medium, które stale tworzyło nowe treści. Z czasem jednak pojawiła się duplikacja (tzn. ta sama treść pojawiająca się pod wieloma różnymi adresami URL), a wyszukiwarki muszą się do tego dostosować. Typowym przykładem jest Wikipedia, która zawiera wiele artykułów. Niektórzy autorzy innych stron przejmują te teksty (częściowo lub w całości), tworząc w ten sposób duplikaty. W większości przypadków nie ma to jednak znaczenia, ponieważ strona źródłowa ma znacznie wyższą rangę (jakość linków) niż strona plagiatowana, ale czasami może się zdarzyć, że pogorszy to pozycję oryginału kosztem pirata.

Wyszukiwarki internetowe musiały dostosować się do tej wady, w związku z czym powstał termin "kanonizacja", który można rozumieć jako "usunięcie" pewnych stron z indeksu. Dzięki temu indeksy stają się mniejsze, a wyszukiwarka nie musi niepotrzebnie przeszukiwać ciągle tej samej zawartości.

Duplikaty są wewnętrznie podzielone na dwie duże kategorie przez każdą wyszukiwarkę:

Naturalne duplikaty
-------------------

Wynikają one z naturalnego zachowania Internetu i jego cech.

Na przykład, URL `http://mathematicator.cz` prawdopodobnie będzie miał taką samą zawartość jak URL `http://www.mathematicator.cz` lub `http://mathematicator.cz/index.php`, ponieważ jest to standardowe zachowanie serwera Apache (i ogólnie Internetu).

Jeśli zostanie znaleziona naturalna duplikacja, wyszukiwarka tworzy "zestaw kanoniczny", czyli grupę stron, z której wybiera jednego reprezentanta wyróżniającego się w wyszukiwaniu. Jeśli link prowadzi do dowolnej strony z zestawu kanonicznego, jej ranga zostanie automatycznie przekazana do głównego reprezentanta.

Często dobrym pomysłem jest pomoc wyszukiwarce w utworzeniu tego zestawu i prawidłowym ustawieniu przekierowania na stronie, co spowoduje, że wyszukiwarka lepiej przyjrzy się stronie, a główny przedstawiciel zostanie lepiej wybrany.

Duplikaty prowadzące do plagiatu
----------------------------

Plagiat jest dziś problemem w Internecie. Sam fakt istnienia plagiatu nie ma większego znaczenia, jednak użytkownikom przeszkadza on szczególnie, ponieważ na to samo zapytanie znajdują wciąż te same wyniki. Czy zdarzyło Ci się również znaleźć kilka stron z identycznym tekstem dla danego zapytania? Jest to dokładnie takie zachowanie, któremu wyszukiwarki starają się zapobiegać.

Największym problemem jest określenie, która strona jest oryginalnym źródłem - i zrobienie tego maszynowo. Również w tym przypadku wyszukiwarki umieszczają wszystkie podobne strony w zbiorze kanonicznym i wybierają z tego zbioru głównego reprezentanta. Jeśli źródła pochodzą z różnych domen, nie można traktować sytuacji jako naturalnej duplikacji (i wybierać dowolnego kandydata), ale należy ocenić wszystkie strony pod względem jakościowym i wybrać najlepszą z nich w sposób obiektywny - a najlepiej z oryginalnego źródła treści.

Wyszukiwarki często podejmują decyzje na podstawie rangi całej domeny i siły sieci linków do danego dokumentu, ale nawet to jest dość zawodne podejście. Drugim czynnikiem jest zazwyczaj czas utworzenia (indeksacji) dokumentu. Dlatego każda wyszukiwarka śledzi, które strony są często wykorzystywane do tworzenia nowych treści i często odwiedza te strony, tak aby nowa strona została idealnie od razu zauważona i nie była wybierana jako proxy.

Szczegółowy opis metod działania tej selekcji wykracza poza ramy niniejszego opracowania i mógłby być tematem całej książki.
