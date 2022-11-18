Co bym zmienił w Nette, gdybym był Davidem Grudlem
==================================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	pl: co-bym-zmienil-w-nette-gdybym-byl-davidem-grudlem
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

Od prawie 6 lat aktywnie używam Nette Framework do tworzenia oprogramowania komercyjnego. Przez pierwsze lata byłem bardzo entuzjastycznie nastawiony, a framework doskonale odpowiadał na wszystkie potrzeby, które mieliśmy jako zespół i w zasadzie nie było powodu, aby szukać innego narzędzia.

Od około 2019 roku zaczynam widzieć pewien spadek pierwotnego entuzjazmu w ramach Nette, gdzie zaczynam tęsknić za niektórymi zaawansowanymi funkcjami. Często są to rzeczy poza samym frameworkiem, więc nawet nie spodziewam się, że kiedykolwiek zostaną zaimplementowane, ale z drugiej strony deweloper musi podjąć decyzję, jak kontynuować rozwój do przodu, a może sięgnąć po inną technologię.

Warstwa bazy danych
-----------------

Nette Database uważam za jeden z największych błędów frameworka, niestety.

Co tydzień mamy w firmie rozmowy kwalifikacyjne, w których juniorzy zaraz po szkole, a nawet osoby średniozaawansowane, które przechodzą z innego frameworka, aplikują i odkrywają Nette Database jako część swojej eksploracji Nette. Jako warstwa bazodanowa nie jest zbyt zła, ale problemem jest wtedy praktyczne wykorzystanie w prawdziwych projektach.

Dzieje się tak dlatego, że Nette Database nie podejmuje wysiłku, aby popchnąć programistów do używania ścisłych typów danych od początku, do nadawania nazw kolumnom i relacjom, do oddzielania metod zapytań (repozytorium) od modeli i innych małych niuansów, które ogólnie robią wiele.

Przez wiele lat używałem biblioteki Dibi, która w swoich czasach odgrywała ogromną rolę. Pozwalało to na całkiem ładne odpytywanie bazy danych i ujednolicało interfejs. Z drugiej strony, czasy poszły do przodu, a kiedy bazy danych zwracają nieopisane obiekty, PHPStan po prostu nie będzie szczęśliwy tylko na zasadzie.

Osobiście, albo włączyłbym natywne wsparcie dla podmiotów i repozytoriów do Nette Database, albo zapewniłbym oficjalne wsparcie dla Doctrine w ramach Nette. Jeśli programujesz aplikacje na poziomie dużej korporacji, musisz poważnie podchodzić do rozwoju, a Doctrine w szczególności ma dużo sensu. Jednocześnie chcesz od razu edukować nowych w lepszej technologii, a nie chcesz uczyć ich starych nawyków.

Śledzenie i rejestrowanie błędów
---------------------

Nadal uważam Tracy za najlepszy debugger runtime dla PHP.

Kiedy w 2017 roku przyszedłem do nienazwanej agencji cyfrowej i zobaczyłem stan techniczny ich projektów Nette, byłem przerażony. Ileż to razy mieli strony napisane w czystym PHP bez próby logowania błędów. Dość łatwym rozwiązaniem było zainstalowanie Tracy na projekcie, wywołanie `Debugger::enable()` i nic więcej nie robienie. Błędy były automatycznie rejestrowane do katalogu, który wystarczyło ręcznie odebrać co kilka dni.

Co do Tracy, to wydaje mi się, że jest to gotowy produkt, który David ma niewiele powodów, by poprawiać. Z drugiej strony wciąż widzę pole do popisu w nowym kierunku.

Na przykład, dlaczego Tracy nie zawiera płatnych funkcji dla firm lub osób prywatnych, które poważnie myślą o bezpieczeństwie?

Tracy może wdrożyć niestandardowy interfejs internetowy typu Sentry, w którym tworzysz konto, a błędy produkcyjne są automatycznie wysyłane za pośrednictwem interfejsu API, gdzie możesz je śledzić w różnych projektach. Aplikacja mogłaby też odpytywać poszczególne strony i automatycznie wykrywać, że jest niedostępna, i powiadamiać właściciela w inny sposób (skoro Tracy nie potrafi logicznie wykryć awarii serwera). Czasami natrafiam również na problem, że Tracy jest przypadkowo włączony w witrynie produkcyjnej, a możesz dowiedzieć się za pośrednictwem DIC, jak uzyskać dostęp do bazy danych lub gdzie indziej. Tracy powinna cię o tym również powiadomić.

Tracy mógł również zaimplementować inne małe rzeczy, które znam z Node.js. Na przykład, dla wyświetlania kodu źródłowego, można by wybrać interwał znaków, przy którym linia jest podświetlana w specjalny sposób, dzięki czemu istnieje możliwość podświetlenia konkretnego tokenu w obrębie linii. Jednocześnie byłoby miło dodać wsparcie dla drugiej (być może niebieskiej) linii podświetlenia, aby pokazać kontekst w następnej części aplikacji.

Podczas wyświetlania fragmentu kodu źródłowego nie bałbym się dodać przycisku, który może renderować resztę kodu w ajaxie, więc mogę go zbadać bezpośrednio w przeglądarce z callstack. To właśnie robi Symfony i jest to świetna funkcja. Gdyby uzupełnić to o podstawową analizę statyczną z możliwością indeksowania metod, klas i dziedziczenia, zaoszczędziłoby to wiele pracy przy debugowaniu dla całego zespołu.

Jeśli callstack przemierza pakiet, byłoby miło pokazać wersję pakietu, z którym pracuję dla konkretnego pliku. Często rzucę błąd w pakiecie, który rozwijam, i równie dobrze mogę wizualnie wiedzieć, że jest to starsza wersja.

Routing statyczny
----------------

W ramach Nette Router pozwoliłbym na zdefiniowanie nowego typu o nazwie static router. Byłby to potomek podstawowego routera, ale byłby w stanie zaakceptować tylko string-literal, który nie byłby wyrażeniem regularnym, ale prostą ścieżką. Wiele stron działa statycznie (kontakty, różne artykuły, zaskakująco często nawet strona główna), a router musi za każdym razem oceniać reguły regex dla dopasowania i kompozycji URL z tego powodu.

Jeśli statyczna trasa została użyta w szablonie Latte, na przykład, mogłaby być bezpiecznie przetłumaczona na statyczny ciąg `{$basePath}/path` w czasie kompilacji szablonu, więc nie byłoby potrzeby kompilowania 100 adresów URL, które są w układzie, na przykład, na każdym żądaniu.

Rozumiem, że mogę osiągnąć tę samą funkcjonalność poprzez buforowanie po stronie szablonu, ale o wiele bardziej doceniłbym rozwiązanie systemowe.

W frameworku Next.js całkowicie wpadłem na koncepcję statycznego routingu. Nie ma czegoś takiego jak `n:href`, w skrócie trzeba znać URL z góry, co zmusza do nie robienia tylu przekierowań, myślenia o formatach URL z wyprzedzeniem i utrzymywania ich w stanie trwałym.

Interfejs PSR
------------

Uważam, że szkoda, że Nette nie implementuje natywnie interfejsu PSR. Jako twórca pakietów prowadzi cię to na manowce, jeśli chcesz w ogóle zdefiniować zależność Composera od Nette. Z jednej strony Nette rozwiązuje za Ciebie wiele problemów, z drugiej strony wiesz, że po prostu nie zastąpisz go potem. Nie raz przekonałem się, że z powodu braku generycznego interfejsu wolę używać Symfony, Laravel lub zupełnie innego.

Brak wsparcia dla standardów generycznych dla przyszłej przenośności jest głównym powodem, dla którego w 2022 roku całkowicie odszedłem od Nette i rozwijamy nowe aplikacje w zespołach wyłącznie w generycznych.

Warstwa API
----------

Co jeśli chcesz użyć Nette jako backendu do obsługi żądań REST API? Czy budujesz prezentera i wysyłasz odpowiedź jako `$this->sendJson()`? Czy zainstalujesz bibliotekę innej firmy? A może jesteś Davidem Grudlem, który rozwiązuje potrzebę brakującej biblioteki, pisząc od razu nową?

Dlaczego Nette nie może zaimplementować czegoś tak powszechnego jak REST API zaraz po wyjęciu z pudełka? Dziś niemal każda aplikacja potrzebuje komunikacji za pośrednictwem API - na przykład w celu dostarczenia danych do frontendu.

Dla nowicjuszy, to znowu prowadzi ich do korzystania z wbudowanego Latte i snippets, zamiast dowiedzieć się, że istnieje lepsza opcja, a to jest zbudować cały frontend osobno obok, i po prostu dostarczyć dane do niego.

Zaufanie do tego, co będzie za 10 lat
-------------------------

Ostatni punkt jest bardzo pragmatyczny i pochodzi z debaty, którą słyszymy od czasu do czasu z działów biznesowych w zespole.

Co by się stało, gdyby David Grudl przestał całkowicie rozwijać Nette? Co by było, gdyby ktoś przestał ją rozwijać? Na co byśmy się przestawili? I jakie to będzie wyzwanie?

Wielu programistów (często bez doświadczenia korporacyjnego) lubi szydzić z tej obawy, mówiąc, że jest w porządku. Niby tak, ale nie jest.

Kiedy zmagasz się z pytaniem, czy zainwestować czas w Nette czy React i Node.js, aby wyszkolić młodszych programistów w swojej firmie, niestety wygrywa ta druga opcja.

Nette jeszcze nie tęskni za pociągiem, ale w dziesiątkach rozmów, które przeprowadziłem w ciągu ostatniego półrocza, czuję to dość mocno.
