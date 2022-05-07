Jak żyje programista w wewnętrznym zespole programistycznym
===========================================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	pl: jak-zyje-programista-w-wewnetrznym-zespole-programistycznym
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

Za uczciwość trzeba zapłacić wysoką cenę.

Ta strona zawsze opisywała rzeczywistość, z jaką stykają się ludzie z branży IT, dlatego chciałbym przyjrzeć się moim doświadczeniom z pracy w zespołach programistycznych. Poniżej przedstawiam ogólne doświadczenia, jakie zdobyłem w różnych firmach. Żadne doświadczenie nie jest związane z jedną konkretną firmą i nie musi służyć jako krytyka.

Firmy zwykle nie chcą pracowitych i proaktywnych ludzi
----------------------------------------------

Masz mnóstwo pomysłów? Czy chcesz wprowadzać innowacje? Czy lubisz znajdować eleganckie rozwiązania złożonych problemów, które rozwiązuje Twój zespół, a które nękają połowę firmy? Czy zdajesz sobie sprawę ze znaczenia bezpieczeństwa, projektowania oprogramowania i wyszukiwania wąskich gardeł w projektach?

Prawdopodobnie przez długi czas będziesz niezadowolony z pracy w zespole programistów.

Praca zespołowa to coś, z czym ostatnio często się zmagam - mam na myśli to, że wręcz pławię się w niej i próbuję zrozumieć zupełnie nieintuicyjne dla mnie zasady, których muszę przestrzegać.

- Praca zespołowa oznacza opracowanie takiego rozwiązania, które będzie zrozumiałe dla całego zespołu. Często nie są one najlepsze. To prawie nigdy nie jest eleganckie. W rzeczywistości jest to zawsze nieefektywne. Ma ona jednak tę fajną zaletę, że cały zespół ją rozumie i można nią zarządzać w dłuższej perspektywie czasowej. Jest to niezwykle ważna idea. W przypadku dużych projektów nie ma tak dużej presji, aby być wydajnym, ponieważ jako firma możesz zwiększać skalę działania dzięki ludziom i masz wystarczająco dużo pieniędzy. O wiele ważniejsze jest dostarczanie rzeczy na czas, nawet jeśli są częściowo uszkodzone i z nieznanymi z góry ograniczeniami, ale na czas. Wiąże się to z przekonaniem, że rozwiązanie, które dostarczysz jutro (nawet jeśli jest niedoskonałe), ma o wiele większą wartość dla klienta niż w 100% sprawdzone rozwiązanie, które będzie dostępne dopiero za rok.
- Wprowadzanie innowacji i wypychanie rzeczy na zewnątrz jest raczej błędem. Ma to związek z faktem, że w firmach pracują prawdziwi ludzie, którzy mają rodziny i chcą pracować tylko w godzinach pracy. Wynika z tego, że ludzie mają ograniczony czas na uczenie się i próbowanie nowych rzeczy. Zasadniczo firmy muszą wcisnąć w godziny pracy ludzi edukację i rozwój, które mogą być wykorzystane w przyszłej pracy. Ciekawą konsekwencją jest odkładanie decyzji i uczenia się nowych rzeczy na później. Ze swojej strony rozumiem to podejście i jest ono dla mnie bardzo sensowne. Gdybym miał pracowników, też wolałbym spokojnych ludzi, którzy będą w firmie przez, powiedzmy, 15 lat, a nie takich, którzy mają świetny ogólny przegląd i chcą się przenieść, ale odbędzie się to kosztem zespołu. To prawda, że rozwiązanie zbiorowe nigdy nie będzie takie samo jak rozwiązanie indywidualne, ale to nie znaczy, że będzie gorsze.

Ludzie tacy jak ja są potencjalnym źródłem problemów i konfliktów. Fajnie jest myśleć o tym w ten sposób.

Podczas przeglądu kodu szukaj tylko obiektywnych błędów
----------------------------------------

Każda firma ma swoje zasady kodeksu postępowania. Niestety. Na początku bardzo mnie to denerwowało.

Jeśli używasz jednego z bardziej dojrzałych języków, takich jak .NET, zasady stylu kodowania są bardziej zbliżone. Dopiero wtedy dowiadujesz się, że na świecie jest jeszcze PHP i JavaScript. Zwłaszcza kwestia PHP jest czasem nieco frustrująca. PHP jest bardzo dojrzałym językiem z wieloma wspaniałymi funkcjami, ale z mojego doświadczenia wynika, że jest on używany w firmach w około jednej trzeciej swojego potencjału. Przyczyny bywają różne, najczęściej jest to bezwładność.

W związku z tym od dawna poszukuję proceduralnego rozwiązania do przeglądu kodu. Od dłuższego czasu bawię się programem PhpStan i śledzę pomysły z nim związane. Podstawowym założeniem programu PhpStan jest to, że podkreśla on tylko obiektywne błędy, które na pewno kiedyś wystąpią. Im bardziej zgłębiam PhpStan i przenoszę go na wyższy poziom, tym bardziej wewnętrznie potwierdzam, że to prawda.

Byłoby miło, gdyby PhpStan został wdrożony przynajmniej w połowie czeskich firm. Byłoby jeszcze lepiej, gdyby był to co najmniej poziom 6. Z praktyki wynika, że najlepsze firmy mają poziom 4 lub 5.

Niedawno zastanawiałem się, czy istnieje realnie działająca aplikacja produkcyjna, która ma swój własny zespół programistów i nie przechodzi nawet poziomu 0 - poziomu, który kontroluje rzeczy, których można oczekiwać od aplikacji. Coś w rodzaju upewnienia się, że wszystkie klasy i funkcje istnieją, pliki nie zawierają błędów parsowania itd. I owszem, istnieją takie aplikacje. Jednak w dłuższej perspektywie czasowej sytuacja ulega poprawie. Mam nadzieję, że tak.

W podobny sposób postępuję z codereview. Zgłaszam tylko rzeczy, które obiektywnie zawsze są złe. Często spotykam się z błędną oceną stopnia - coś jest nie tak, ale nie jest to aż tak duży problem, żeby trzeba było się nim zajmować. Wiele problemów rozwiązuje się na zasadzie konwencji lub stwierdzenia, że ryzyko jest niewielkie, więc nie ma sensu go leczyć.

Zawsze uważałem brak typów danych (nawet złożonych) za błąd krytyczny. Wtedy zrozumiałem, że istnieje test i zrzutka.

Nie sądzę, żebym miał jakieś konkretne wnioski dotyczące tej części. Mam po prostu wiele subiektywnych uwag na ten temat, które raczej mogą być źródłem wzajemnych nieporozumień, więc nie będę ich zamieszczał. W dłuższej perspektywie skłaniam się do wniosku, że nie mogę prowadzić przeglądu kodu - albo jestem zbyt surowy, albo zbyt dobroduszny. Nie potrafię określić na poziomie ogólnym, co jest naprawdę ważne, a co nie. Chciałbym się dowiedzieć, jak robią to inni. Nikt jeszcze nie potrafił udzielić mi odpowiedzi, z której mógłbym skorzystać.

Tworzenie oprogramowania na zamówienie nie jest opłacalne.
---------------------------------

Czy masz agencję i zajmujesz się tworzeniem stron internetowych na zamówienie? Jeśli nie jesteś wystarczająco duży i nie realizujesz dużych projektów dla innych korporacji, prawdopodobnie pewnego dnia nie będziesz istniał.

Dzieje się tak z powodu słabego finansowania projektów niestandardowych. Prawdopodobnie ma to związek z charakterem kontraktów, które są bardziej opłacalne dla nabywców. W rzeczywistości większość agencji jest brutalnie niedofinansowana i przynosi realne zyski tylko właścicielom.

Gdy projekt jest tworzony wewnętrznie, jako firma dla siebie, opóźnienie w dostawie o miesiąc prawdopodobnie nie ma aż tak dużego znaczenia. Jako agencja masz gwarancję, że jeśli nie dostarczysz pracy do miesiąca, to w tym miesiącu będziesz powoli zasypiał w pracy, konsekwentnie rujnując swoje zdrowie, klient będzie przeklinał do telefonu i biletów, zawsze pytał, czy można szybciej, a w nagrodę i tak zapłacisz karę umowną. A jeśli tego nie zrobisz, klient uzna Cię za nierzetelnego wykonawcę i może rozważyć pójście do innego miejsca, gdzie i tak nie będzie lepiej.

Ale takie są reguły gry, które poznałem w praktyce i które spowodowały dziurę w moim budżecie.

Kontraktowanie jest prawdopodobnie najbardziej złożoną formą działalności w branży IT. Nie chcesz zawierać umów. Kontraktowanie zniszczy Cię. Będą do Ciebie dzwonić w weekendy, w nocy, w święta. Za połowę pracy, którą wykonujesz, nie dostajesz wynagrodzenia. Będziesz przepraszać za błędy, których nie możesz popełnić. Będziesz oglądać na Instagramie zdjęcia znajomych, którzy wyjechali na trzecie wakacje w tym roku, podczas gdy Ty będziesz siedzieć w biurze o 3 nad ranem w sobotę i kończyć projekt dla klienta, za który i tak zostaniesz skarcony w poniedziałek, bo w umowie było więcej niż mogłeś zrobić. Ludzie będą brać urlopy i zwolnienia lekarskie, a Ty będziesz pracować na ich miejsce. Albo zostaniesz zwolniony z pracy. A wtedy będziesz zadowolony z płacenia czynszu.

Ale z drugiej strony, można się wiele nauczyć. Samo pozostawanie pod presją wywiera na nas duży nacisk, abyśmy uczyli się i byli skuteczni, tak aby następnym razem móc zrobić trochę więcej w tym samym czasie. Złą stroną jest to, że nie można wykorzystać tej wiedzy i doświadczenia gdzie indziej, ponieważ firmy nie są tym zainteresowane (co wyjaśniłem powyżej).

Na szczęście jest to historia z 2019 r., a do tego jeszcze daleka droga.

Nigdy nie będzie idealnego świata
-------------------------

Czy robię to, co sprawia mi przyjemność? A ty? :D

Myślę, że wszystko jest kwestią proporcji. Prawdopodobnie nigdy nie będziesz wykonywać pracy, która w 100% spełniałaby Twoje oczekiwania. Prawdopodobnie zawsze ktoś będzie Ci tłumaczył, że nie potrafisz zrobić rzeczy, której uczysz się na siłę od 10 lat, a wewnętrznie wiesz, że potrafisz.

Z drugiej strony, za pewne zaprzeczenie własnej osobowości zyskuje się przestrzeń, aby mieć coś więcej. Na przykład czas spędzany w weekendy, lepsze zdrowie, więcej czasu dla siebie, stabilne środowisko itp. To, że nie da się tego utrzymać na dłuższą metę, też jest oczywiste.

Lubię mieć możliwość wypróbowania różnych rzeczy i przekonania się z pierwszej ręki, jak to jest u innych. Praca w zespole programistów to ogromna nuda w porównaniu z pracą na zamówienie.

Nie chodzi już o znalezienie najlepszego i najszybszego rozwiązania, ale o współpracę. Chodzi o to, by poprawić komunikację, emocje, a przede wszystkim nauczyć się być człowiekiem. I to też jest ogromna wartość.
