Zwrot z UUID na liczbę całkowitą
================================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	pl: zwrot-z-uuid-na-liczbe-calkowita
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

W procesie tworzenia oprogramowania programista dość często trafia w ślepy zaułek, gdy staje przed decyzją architektoniczną, która będzie miała ogromny wpływ na przyszłość jego pracy przez następne dziesięciolecia. Jednocześnie jest to decyzja, której nie można cofnąć, a za każdy błąd trzeba zapłacić wysoką cenę. Bazy danych są typowym przykładem decyzji architektonicznej, w przypadku której każdy najmniejszy błąd powoduje upadek głowy.

Jedną z ważniejszych decyzji ostatnich czasów jest sposób przechowywania kluczy podstawowych w tabelach bazy danych. Choć wydaje się to banalnym problemem, kryje się za nim o wiele więcej, niż mogłoby się wydawać.

Opcje klucza głównego
-------------------------

Zasadniczo istnieją cztery podstawowe opcje:

- liczba całkowita
- integer unsigned
- big int
- <a href="/uuid-performance">UUID</a>.

Integer to po prostu liczba całkowita (w przypadku `unsigned` to unsigned, więc zawsze dodatnia, a w przypadku `big int` to może osiągać bardzo duże wartości). Bardzo prosta koncepcja. UUID jest to ciąg tekstowy (na przykład w postaci `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`), który składa się z kilku części, z których każda może mieć pewne właściwości i jest przydatna do budowania ogromnych aplikacji wieloserwerowych lub zdecentralizowanych. wokół UUID istnieje duży ekosystem przydatnych technologii, które rozwiązują problemy, o których istnieniu możesz nawet nie wiedzieć, lub które pojawią się w przyszłości.

Użyj właściwego młotka
-------------------------

Niedawno (zima 2020) mój przyjaciel Paweł wyjaśniał koncepcję zastosowania odpowiedniego rozwiązania do problemu o określonej wielkości. Jest to wspaniała i ważna idea, o której wielu programistów lubi zapominać - w ten sposób powstają bardzo złożone rozwiązania, gdy nie jest to konieczne. W języku angielskim mamy na to **nadmierną inżynierię**.

Rozmiar i niepowtarzalność identyfikatora UUID
--------------------------

Podstawową zaletą identyfikatora UUID jest to, że jeśli aplikacja stanie się zbyt duża i rozdzielimy bazę danych na wiele serwerów WWW, a jedna tabela bazy danych jest tak ogromna, że nie zmieści się na dysku jednego komputera, możemy ją rozdzielić na wiele maszyn fizycznych, z których każda będzie znała swój fragment tabeli i odpytywała swoich kolegów o resztę. UUID rozwiązuje także podstawowy problem wstawiania nowych wierszy, gdy w przypadku bardzo dużych aplikacji musimy zapisywać wiersze równolegle w wielu miejscach i nie chcemy czekać, aż zwolni się miejsce zapisu na głównym serwerze.

Koncepcja pisania w wielu miejscach jednocześnie jest wykorzystywana np. w aplikacjach do czatowania. Kiedy użytkownik wysyła wiadomość za pośrednictwem komunikatora Messenger, trafia ona do najbliższego serwera bazy danych Facebooka, który przypisuje jej identyfikator UUID i znacznik czasu, a następnie zapisuje ją w swojej lokalnej bazie danych. Z kolei Twój znajomy na drugim końcu świata zapisuje wiadomości w swoim lokalnym centrum danych, a w międzyczasie cała infrastruktura chmury zapewnia synchronizację na całym świecie. Brzmi fajnie, prawda? :)

Aby taki równoległy zapis mógł działać, należy rozwiązać problem kolizji rekordów. Jeśli poszczególne lokalne bazy danych używają zwykłej liczby całkowitej, bardzo szybko dwa niezależne serwery zapiszą dwa różne rekordy pod tym samym identyfikatorem. Gdy te rekordy są zsynchronizowane, dochodzi do kolizji. Zwykle nie ma rozwiązania - nie można zmienić numeracji identyfikatorów, ponieważ mogą do tego doprowadzić inne sesje.

UUID rozwiązuje to na przykład w ten sposób, że każdy serwer otrzymuje uzgodniony prefiks, który umieszcza na początku każdego identyfikatora UUID, następnie umieszcza znacznik czasu, a potem sam identyfikator.

> **Interesujący fakt:** Przy zapisywaniu tak dużej ilości danych interesuje nas nie tyle to, kiedy jaki rekord został zapisany, ale w jakiej kolejności (abyśmy np. nie przestawiali kolejności wiadomości dla użytkownika).

Można zapytać, jak poradzić sobie z sytuacją, w której serwery nie mogą się porozumieć co do wyboru prefiksu. Problem ten pojawia się na przykład w aplikacjach zdecentralizowanych lub działających w trybie offline. W tym przypadku identyfikator UUID może być generowany losowo.

Pytanie brzmi więc, jakie są szanse, że konflikt wystąpi podczas <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">losowego generowania identyfikatora UUID</a>. Prawdopodobnie nie przytrafi się to Tobie. Istnieje około `2^122` unikalnych identyfikatorów UUID (ponieważ jest to liczba `128-bitowa`). W praktyce prawdopodobieństwo wystąpienia konfliktu wynosi około `0,000000006 (6 × 10-11)`. W praktyce oznacza to, że jeśli będziemy generować **1 miliard identyfikatorów UUID** co sekundę przez następne 100 lat, to szansa na konflikt wyniesie `50%`. Jest więc bardziej prawdopodobne, że konflikt nie wystąpi, a identyfikator UUID jest ostatecznym rozwiązaniem problemów z bazą danych.

Czy istnieje zapotrzebowanie na tak solidne rozwiązanie?
-------------------------------

Jeśli nie wiesz, odpowiedź brzmi: **nie**.

Przy kluczu głównym ustawionym na `int` z flagą `unsigned`, istnieje `4,294,967,295` możliwych wartości (4 miliardy). Porównanie rozmiarów liczb całkowitych można znaleźć w dokumentacji <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">MySql</a>.

Gdy w jednej tabeli przechowuje się 4 miliardy rekordów, prawdopodobnie wcześniej zabraknie miejsca na dysku.

Wydajność całek i złączeń
----------------------

Liczby całkowite są naprawdę bardzo szybkie. W MySql istnieją dla nich natywne optymalizacje. Indeksy działają prawidłowo (a ponadto są znacznie mniejsze), zajmują tylko 4 bajty, można do nich szybko dołączać i w większości przypadków będą wystarczające.

Jeśli masz do czynienia z problemem replikacji bazy danych, najlepszym rozwiązaniem może być umieszczenie całej bazy danych w chmurze, np. w MS Azure, i odpytywanie jej z zewnątrz. Nawet w przypadku przechowywania dziesiątek milionów rekordów szybkość dostępu za pomocą liczby całkowitej do określonego wiersza jest rzędu milisekund (poniżej 3 ms na dobrze skonfigurowanym serwerze), a dzięki indeksowi klastrowemu czas ten może być zrównoważony nawet przy dużej liczbie żądań.

Jeśli naprawdę musisz używać identyfikatorów UUID, lepiej porzuć świat MySql i wybierz bazę danych Postgres, która w przeciwieństwie do MySql ma własny typ danych dla <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUID</a>. Praca z łączeniami stanowi ogromny problem w przypadku identyfikatorów UUID i MySql, a podczas łączenia tylko 3 tabel (z których każda ma tylko po kilkadziesiąt tysięcy rekordów) całe zapytanie może zająć od kilkuset milisekund do kilku sekund. Jest to niestety problem związany z MySql, którego prawdopodobnie nie da się rozwiązać.
