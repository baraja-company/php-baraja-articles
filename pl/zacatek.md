Kurs PHP online dla początkujących
==================================

> id: '10ecc1cc-8d49-4882-8ecf-114ad74902df'
> slug:
> 	cs: zacatek
> 	pl: kurs-php-online-dla-poczatkujacych
> 
> perex:
> 	- 'Výuka PHP online, kurz pro začátečníky. Naučíte se programovat v PHP přímo od senior vývojáře.'
> 	- 'Samouczek PHP online, kurs dla początkujących. Naucz się programować w PHP bezpośrednio od doświadczonego programisty.'
> 
> publicationDate: '2020-02-09 14:27:47'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: f4f8a8d74e09eb298963f7ab8ff00959

PHP to język skryptowy po stronie serwera, zaprojektowany z myślą o nowoczesnych aplikacjach internetowych.

Język PHP oferuje bardzo szybką krzywą uczenia się, tzn. w bardzo krótkim czasie (rzędu tygodni) będziesz w stanie zrozumieć większość zasad tego języka do tego stopnia, że będziesz w stanie stworzyć prawie każdą prostą aplikację internetową z wykorzystaniem formularzy, kont użytkowników, bazy danych i wielu innych elementów.

Kolejną zaletą PHP jest jego masowe rozpowszechnienie na prawie wszystkich serwerach (w przypadku hostingu) oraz ciągły rozwój, dzięki czemu masz pewność, że Twoja aplikacja/strona będzie działać wszędzie.

Jak zacząć?!?
------------

Przed przystąpieniem do pracy należy upewnić się, że zostały spełnione poniższe warunki:

- Mózg, to w dużej mierze myślenie,
- Komputer (lub serwer), na którym można uruchamiać skrypty,
- Przydatna jest wiedza z zakresu matematyki lub jakiejś dziedziny technicznej,
- Odpowiednie materiały do nauki (takie jak ta strona internetowa i <a href="https://www.php.net">oficjalny podręcznik</a>),
- Podstawowa znajomość HTML i CSS,
- Przydatna jest przynajmniej podstawowa znajomość języka angielskiego (większość materiałów, takich jak oficjalny podręcznik i fora internetowe, jest dostępna wyłącznie w języku angielskim),
- Znajomość innego języka programowania jest dodatkowym atutem (bardzo podobnego do C/C++, na którym bazuje PHP),
- Zdecydowanie zalecam podstawową znajomość HTML i CSS, bez której zrozumienie PHP jest bardzo trudne.
- Podstawowa znajomość oprogramowania (różni się w zależności od systemu, a najlepsze programy **nie są darmowe**).

Oprogramowanie podstawowe
-----------------

Komputer z systemem Windows:`
- Każda nowoczesna **przeglądarka internetowa**, która oferuje tryb debugowania. Osobiście używam <a href="https://www.google.com/chrome">Google Chrome</a>.
- Na początek wystarczy lepszy **redaktor tekstu** z kolorowaniem składni. Najlepszym na świecie jest prawdopodobnie <a href="https://www.sublimetext.com">Sublime Text</a> (który oferuje zaawansowaną pracę z dowolnym tekstem w wielu formatach, pracę z wieloma kursorami, wyrażenia regularne i ogólnie jest uniwersalnym narzędziem nie tylko do programowania). W przeszłości używałem czeskiego edytora <a href="https://www.pspad.com/cz/">PSpad</a> (który obecnie uważam za bardzo przestarzały i niewystarczający dla nowoczesnych stron internetowych), niektórzy używają też <a href="https://www.slunecnice.cz/sw/notepad/">Notepad++</a>.
- Jeśli poważnie myślisz o programowaniu, wolałbym używać pełnego środowiska programistycznego. W pracy używam programu <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, który uważam za najlepszy edytor do pisania kodu, jaki kiedykolwiek powstał.
- Serwer WWW**, który obsługuje PHP, bazę danych MySql i umożliwia konfigurowanie ustawień. Obecnie uważam, że najlepszym wyborem dla systemu Windows jest <a href="https://www.apachefriends.org/download.html">Xampp</a>, który jest pakietem wstępnie spakowanym.

Linux (zwłaszcza serwer WWW):`
- Dowolna przeglądarka, na przykład <a href="https://www.google.com/chrome">Google Chrome</a> lub Firefox.
- W Ubuntu używam <a href="https://www.sublimetext.com">Sublime Text</a>, oba są wystarczające na początek.
- Instalacja **serwera WWW** jest trudniejsza niż w przypadku systemu Windows. Na przykład w Ubuntu istnieje do tego celu program <a href="https://wiki.ubuntu.cz/servery/apache_s_mysql_a_php">Tasksel</a>, który jest kontrolowany przez Terminal.
- Jeśli instalujesz serwer z systemem Linux, warto rozważyć także <a href="https://www.nginx.com/resources/wiki/">Ngnix</a>.

Mac:`
- Mac świetnie nadaje się do programowania, jest dostosowany do potrzeb użytkownika.
- Do pracy nad projektem na MacBooku Pro używam <a href="https://www.jetbrains.com/phpstorm/">PhpStorm</a>, który uważam za najlepsze środowisko programistyczne, a do edycji zwykłych plików tekstowych używam <a href="https://www.sublimetext.com">Sublime Text</a>, który bardzo dobrze radzi sobie z dużymi plikami.
- Serwer zainstalowałem samodzielnie za pomocą **Terminala**, co może być trudne dla początkujących, ale istnieje narzędzie o nazwie **Mamp**, które umożliwia klikanie wszystkich elementów za pomocą myszy.

Zalecenia dla seniorów:`

Od 2020 roku zaczyna być jasne, że wszystkie problemy z uruchamianiem PHP i całych aplikacji można łatwo rozwiązać za pomocą kontenerów Docker. Nauczenie się, jak pracować z Dockerem, pozwoli zaoszczędzić setki godzin w przyszłości i łatwo zintegrować nowe osoby z istniejącym projektem.

Części serii
------------

Aby zapoznać się z PHP, napisałem kilka artykułów, które pozwolą pokonać barierę dla początkujących i zagłębić się w podstawy PHP:

- <a href="/wprowadzenie">Wprowadzenie do nauki PHP</a>.
- <a href="/first-script">Pierwszy skrypt</a>.
- <a href="/principles-of-variable-writing</a>.
- <a href="/rowery">rowery</a>.
- <a href="/how-to-find-out">Jak odnaleźć się w kodzie</a>.
- <a href="/metody-wysylania-danych">Metody wysyłania danych</a>.
- <a href="/include-file">Include (składanie stron z fragmentów)</a>.
- <a href="/warunki">Warunki i rozgałęzienia</a>.
- <a href="/safe-application">safe-app</a>.

Później jednak tworzenie stron WWW jest już dość skomplikowane i naprawdę trzeba mieć dużą wiedzę (lub przynajmniej podejrzewać, że taka istnieje). Ponieważ koncepcja całego języka i tworzenia stron internetowych jest dość złożona, przygotowałem przynajmniej <a href="/knowledge">podstawowy przegląd wiedzy</a>, który stopniowo uzupełniam i o którym piszę artykuły.

Do tworzenia złożonych aplikacji zalecam używanie <a href="/oop">programowania zorientowanego obiektowo</a>.

Licencja
-------

Materiały te udostępniam za darmo na stronie `php.baraja.cz`, dlatego nie mogą być one wykorzystywane w żadnym innym płatnym kursie. Teksty mogą zawierać błędy i nieścisłości. Nie jest to oficjalne tłumaczenie podręcznika.

Zastrzegam sobie wszelkie prawa do tych tekstów (naprawdę) i dlatego ich kopiowanie jest **zabronione**. Użytkownik może korzystać z adresu URL tej witryny (podanego tutaj) oraz przykładowego kodu źródłowego bez dalszych ograniczeń.

Kontakt
-------

Chętnie porozmawiam z Tobą o tworzeniu stron internetowych, chętnie udzielę Ci ogólnych porad, ale **bardziej złożone prace traktuje jako płatną pracę**.

- E-mail: jan@barasek.com
- Osobiste <a href="https://www.facebook.com/janbarasek">Facebook</a>.

<a href="https://baraja.cz/kontakt">Wszystkie kontakty</a>.
