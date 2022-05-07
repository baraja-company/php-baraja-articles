Algorytm wyszukiwarki internetowej - Drzewa i StopLead
======================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	pl: algorytm-wyszukiwarki-internetowej---drzewa-i-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

W każdej sekundzie w Internecie pojawia się 5 milionów nowych stron, a liczba ta stale rośnie. Aby uporządkować to ogromne morze informacji i znaleźć w nim coś dla siebie, istnieją wyszukiwarki. Poniższa praca ma na celu przybliżenie zagadnienia wyszukiwania i wyjaśnienie całego procesu od utworzenia nowej strony do znalezienia jej w wyszukiwarce.

Zadanie znalezienia i posortowania zbioru miliardów dokumentów nie jest łatwe. Samo Google potrzebuje 300 000 serwerów internetowych, aby wykonać to zadanie w ciągu zaledwie kilku godzin. W rzeczywistości poszukiwanie odpowiedzi na zapytanie odbywa się na długo przed zadaniem pytania. Google przechowuje już w swojej pamięci wyniki wyszukiwania, o które użytkownik będzie pytał w najbliższych dniach.

Architektura wyszukiwarek
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Architektura wspólnej wyszukiwarki do 50 milionów dokumentów" class="w-100 mb-3">

Prawidłowo zaprojektowana wyszukiwarka zawiera wiele komponentów, których jest kilkaset. Schemat ten opisuje najbardziej podstawowe elementy, które wyszukiwarka musi zawierać jako minimum, aby mogła funkcjonować. Jest to stara i nieużywana już architektura, która działała do około 2005 roku, kiedy w sieci było zaledwie kilka milionów dokumentów. Taka architektura nie pozwala na "nieskończoną" ilość treści, a także na ewentualne przestoje serwerów wyszukiwania (wyszukiwanie w bazie). W przypadku awarii jednego elementu cały system przestanie działać, a wyszukiwarka stanie się niedostępna.

Pełny opis aktualnych trendów w projektowaniu nowych architektur wykracza poza zakres niniejszego opracowania, ponieważ są to zazwyczaj tajemnice handlowe dużych firm. Celem niniejszego opracowania jest wyjaśnienie, jak działa ta podstawowa architektura. Poszczególne elementy zostały szczegółowo omówione w kolejnych rozdziałach.

Wprowadzanie zapytań
------------

Najbardziej widocznym elementem, nawet dla zwykłych użytkowników, jest wprowadzanie zapytań, które obecnie najczęściej jest przedstawiane w postaci pola tekstowego. Pole wprowadzania danych znajduje się na specjalnej stronie, która zwykle służy wyłącznie do wprowadzania danych.

W kolejnym etapie użytkownik wpisuje wyszukiwane hasło i przesyła formularz do serwerów wyszukiwarki, inicjując w ten sposób komunikację z komponentem wydającym, zwanym niekiedy wyszukiwaniem głównym. Serwery dystrybucyjne są przystosowane do obsługi ogromnych obciążeń ze strony użytkowników i muszą być w stanie obsłużyć wszystkich szukających jednocześnie.

Architektura serwera dyspozytorskiego to zazwyczaj prosty serwer Apache z podstawową instalacją i doskonałą przepustowością sieci. Kiedy zapytanie trafia do serwera, jest przetwarzane przez drzewa decyzyjne regresji i tworzona jest "mapa zapytania", która zawiera pełną semantykę pozostałych znaczeń, aby wyjaśnić, czego użytkownik tak naprawdę szuka. Metody przepisywania zapytań wykraczają poza zakres niniejszego opracowania, dlatego przedstawimy tylko ogólne opisy tego, jak może wyglądać przepisywanie, a jak nie.

Rozważmy następujące zapytanie:

```txt
"O pejskovi a kočičce"
```

Zapytanie to zostanie przekształcone w drzewo binarne, które uchwyci jego istotę semantyczną:

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Symboliczny diagram drzewa parseków" class="w-100 mb-3">

Istotą drzewa parsowania jest uchwycenie sposobu, w jaki wyszukiwarka będzie patrzeć na wyniki wyszukiwania. Drzewo to, wraz z zapytaniem, jest wysyłane do serwerów wyszukiwania pomocniczego (w niniejszym artykule oznaczanych jako Wyszukiwanie bazowe), gdzie wyszukiwane są poszczególne słowa kluczowe (odczyt indeksu), a następnie sortowane według reguł (według operacji).

| Operator | Operacje sortowania |
|----------|------------------
| AND | Intersection |
| OR | Suma |
| NOT | Dopełnienie |

Komponent ten nie sortuje wyników wyszukiwania, lecz wykonuje szybkie operacje binarne na ogromnych ilościach danych (często milionach rekordów) i zasadniczo porównuje identyfikatory poszczególnych dokumentów.

Sposób działania faktycznego sortowania wyników wyszukiwania na stronie wyników zostanie omówiony w następnych sekcjach. Serwer wyjściowy jest po prostu wykorzystywany do łączenia wielu danych z różnych serwerów wyszukiwania w celu zwiększenia ogólnej wydajności i rozłożenia obciążenia, a następnie przekazuje wykryte wartości na stronę z wynikami (jest to tzw. strona internetowa), zawierającą złożone informacje z kilkudziesięciu serwerów.

Przepisanie na drzewo binarne
-----------------------

Jak wspomniano w poprzednim rozdziale, całe wyszukiwanie opiera się na drzewach binarnych, które mówią, jak będą wyglądały wyniki i czego należy szukać. Cała "logika" i "inteligencja" wyszukiwarki zależy bezpośrednio od jakości programu, który tworzy drzewo - czyli deskryptora.

Prawidłowo skonstruowane drzewo powinno zawierać wszystkie niezbędne metainformacje o zapytaniu w takiej postaci, aby można je było łatwo przetworzyć nawet przy pomocy sekwencyjnego odczytu z dysku oraz powinno eliminować przypadkowe dostępy do pamięci, a więc zazwyczaj zawiera poszczególne wyszukiwane słowa (pochodzące od użytkownika), ich transkrypcje (oraz formy pisane) i wreszcie powiązania między słowami, czyli ich względne odległości w tekście.

Metody transkrypcji zapytań
---------------------

Najbardziej podstawowym sposobem transkrypcji zapytania jest podzielenie go na pojedyncze słowa (według spacji), które będą dalej przetwarzane. Drugi krok polega na wyszukaniu StopWords i zastąpieniu ich zmiennym wskaźnikiem (ponieważ, jak pokażemy później, nie ma sensu w ogóle szukać StopWords).

Załóżmy następujące zapytanie: "O psie i kocie", którego transkrypcja w najbardziej podstawowym stanie wygląda tak:

```txt
"O (and) pejskovi (and) a (and) kočičce"
```

Drugim krokiem jest wyeliminowanie słów, które nie mają związku z wyszukiwaniem. Oczywiście na przykład słowo "o" lub "i" ma znaczenie dla nas, ludzi, ale nie dla bazy danych, ponieważ można je zastąpić innym słowem i wyświetlić wyniki. Celem nie powinno być znalezienie dosłownego dopasowania zapytania do indeksu, ponieważ prowadziłoby to do złych wyników, ponieważ często słowa w tekście w Internecie mają różną pisownię, a ta metoda próbuje znaleźć wyniki w innych formach niż te, które otrzymaliśmy od użytkownika. Jest to zatem pierwszy sygnał, że mamy do czynienia z "inteligentną" wyszukiwarką.

Te bezsensowne słowa są często nazywane "stopwords", każda wyszukiwarka powinna prowadzić indeks takich słów i ten indeks powinien być często aktualizowany (ręcznie).

```txt
["pejskovi (and) * (and) kočičce"]
```

W tym momencie szukamy dwóch różnych słów ("pies" i "kot") z dodatkowym słowem pomiędzy nimi (wewnętrznie oznaczamy je gwiazdką).

Celem tej modyfikacji nie jest podniesienie jakości wyszukiwania, lecz zwiększenie wydajności. Dzieje się tak dlatego, że gdy wyszukujemy słowo, które występuje zbyt często, następuje gwałtowne obciążenie, a taka modyfikacja wspomaga proces - zwłaszcza dzięki temu, że nie wyszukujemy słowa, które i tak będzie występować na tej pozycji w większości dokumentów.

Należy również pamiętać, że nie zawsze możemy dokonać tej korekty (pominięcia słowa), dlatego każda wyszukiwarka powinna prowadzić osobny indeks fraz znalezionych w Internecie, aby sprawdzić, która korekta prowadzi do dobrych wyników, a która nie. Jednak czasami wyszukiwarka dokona takiego dostosowania, nawet jeśli nie ma danej frazy w swoim indeksie - zwłaszcza gdy użytkownik szuka znaczącego słowa, po którym można się spodziewać, że na pierwszych pozycjach znajdą się znaczące strony, co nie ma nic wspólnego z tym "błędem", ponieważ użytkownicy i tak zazwyczaj ich szukają.

Ostatnim krokiem jest praca z językiem angielskim i "oczyszczenie" zapytania z niepotrzebnych znaków. Na przykład w przypadku zapytania "pralka" użytkownik zazwyczaj oczekuje wyników tylko dla słowa "pralka", dlatego możemy zignorować znak przecinka.

Dokładne metody działania tego systemu nie są znane, a każda wyszukiwarka ma swój własny. Uważa się, że w większości przypadków jest to po prostu model statystyczny, który dokonuje tych korekt na podstawie wiedzy o miliardach tekstów w bazie danych.

Transkrypcja języka angielskiego jest również nauką samą w sobie, dlatego w tym miejscu zostanie przedstawiony jedynie podstawowy opis. W większości przypadków wystarczy do każdego słowa dodać jego formy łącznikowe (zgodnie ze słownikiem) i wyszukiwać je razem z tym słowem, dzięki czemu wyszukiwarka może znaleźć dokumenty, w których dane słowa występują nie tylko w formie podstawowej (wpisanej przez użytkownika), ale także w wersjach fleksyjnych - jest to bardzo przydatna funkcja. Problem z tym podejściem polega na odmianie słów niejasnych i problematycznych, a także na odmianie zapytań, które są krótkie (a więc nie można na podstawie kontekstu określić, jak należy odmieniać dane słowo). Przykładem problematycznej fleksji niech będzie słowo "gry".

Biorąc pod uwagę zapytanie w języku angielskim "I love her", źle zaprojektowany flekser dokona fleksji słowa "games" jako, na przykład, "games, gaming, gaming room, ...", tak więc konieczne jest również indeksowanie otaczających słów jako fraz i dokonywanie fleksji tylko w znanych sytuacjach lub zastosowanie innej procedury (tu właśnie często wkraczają algorytmy fonetyczne).

Ostatnim krokiem jest wysłanie gotowego drzewa do serwerów wyszukiwania w bazie, gdzie odbędzie się rzeczywiste wyszukiwanie w beczkach - jest to temat następnego rozdziału.
