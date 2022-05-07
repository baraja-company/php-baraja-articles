Algorytm wyszukiwarki internetowej - ledwie raczkujący
======================================================

> id: '5ad042bf-7fda-4b2e-a38c-762169d0b594'
> slug:
> 	cs: vyhledavac-barely-a-crawler
> 	pl: algorytm-wyszukiwarki-internetowej---ledwie-raczkujacy
> 
> publicationDate: '2016-09-11 12:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '5daf3854688c39f9e0071a93ab11dd4e'

W dzisiejszej lekcji omówimy beczki z danymi, ich strukturę, StopSlovas, a na koniec opiszemy crawlery.

Beczki z danymi
-------------

Jest to specjalny typ danych, który znajduje się na wielu serwerach jednocześnie w wielu kopiach. Są to zazwyczaj pliki o dużej objętości setek GB, które wolno się wczytują (dlatego są dzielone na części) i praktycznie nie da się ich edytować. Jeśli chcemy wprowadzić choćby minimalną zmianę, musimy przekalkulować całą beczkę. Na przykład wyszukiwarka Seznam może przeliczać beczki z danymi co najwyżej raz na kilka dni lub tygodni, podczas gdy Google przelicza je raz na kilka godzin (i to tylko niektóre elementy, nigdy całość).

Beczki zawierają słowa i ich wystąpienia w Internecie. Można powiedzieć, że są to surowe dane dotyczące wyników wyszukiwania w postaci jeszcze nieposortowanej (czasami są one częściowo posortowane według względnej ważności) i przygotowane z wyprzedzeniem. Samo wyszukiwanie odbywa się na długo przed zadaniem zapytania przez użytkownika i to właśnie tutaj przygotowywane są wszystkie wyniki. Samo wyszukiwanie nie byłoby możliwe w czasie rzeczywistym, więc wyszukiwarka w zasadzie odczytuje już przygotowane wyniki (wyszukiwaniem zajmuje się komponent indeksera, który zostanie omówiony w osobnym rozdziale).

Beczki zawierają wszystkie podstawowe informacje potrzebne do tworzenia wyników wyszukiwania dla każdego słowa występującego w Internecie, dlatego podczas czytania sekwencyjnego należy zwrócić uwagę na problemy z wydajnością. W praktyce beczki są zazwyczaj przechowywane w partiach na wielu serwerach, a także w pamięci RAM w celu zapewnienia szybszego dostępu. Na przykład wyszukiwarka Google w ogóle nie odczytuje beczek z dysku, lecz przechowuje je w całości w pamięci RAM, a na dysku zachowuje jedynie ich kopię na wypadek utraty danych z pamięci RAM.

Duże wyszukiwarki o wystarczających zasobach przechowują również tzw. "złote beczki", które zawierają najważniejsze strony w Internecie i są przeszukiwane w przypadku dużego obciążenia wyszukiwaniem. Na przykład w przypadku awarii kilku serwerów wyszukiwania złote beczki tymczasowo zapewniają obsługę wyszukiwań. Wyszukiwarka może nie znaleźć wszystkich wyników, ale przynajmniej wyszukiwanie nie zostanie całkowicie przerwane, a jedynie liczba przeszukiwanych dokumentów zostanie tymczasowo ograniczona. Konieczne jest także regularne aktualizowanie beczek, ponieważ liczba stron w Internecie stale rośnie. Ponieważ beczki mają zwykle rozmiar rzędu gigabajtów, nie jest możliwe wykonywanie konserwacji przyrostowej, ale konieczne jest odbudowywanie całości raz na określony czas. Dlatego wyszukiwarka prowadzi pomocniczą beczkę, w której wszystkie dane mają postać dużej tabeli, z której budowane są beczki - na przykład Google buduje beczki mniej więcej co 12 godzin. Dużym wyzwaniem jest właśnie to, że beczki muszą być przynajmniej częściowo posortowane i odzwierciedlać rzeczywisty stan Internetu. Tak więc dopóki wyszukiwarka nie zaktualizuje beczek, użytkownik nie znajdzie nowych stron internetowych.

Konstrukcja beczki
----------------

W praktyce beczka jest przechowywana w postaci dużego pliku tekstowego, który zawiera identyfikator każdego dokumentu, w którym występuje szukane słowo, oraz pozycję wystąpienia aktualnie szukanego słowa.

Przykład bardzo małej beczki z trzema stronami:

```txt
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

Ta przykładowa beczka zawiera strony o identyfikatorach `1`, `5` i `12`. Przyporządkowane numery wskazują pozycję wystąpienia słowa w dokumencie. Taka beczka przechwytuje tylko wystąpienia pojedynczego słowa, więc każde słowo w Internecie ma swoją własną beczkę. W przypadku wyszukiwania wielu słów kluczowych konieczne jest odczytanie wielu beczek (innej dla każdego słowa), a następnie wykonanie operacji zgodnie z drzewem binarnym (więcej w poprzednich rozdziałach).

Zazwyczaj przecięcie odbywa się poprzez przeglądanie jednego z dokumentów i przeszukiwanie drugiego. Jeśli beczki są duże, mogą nie zostać odczytane w całości, więc wyszukiwarki często zdołają odczytać tylko ich część, a następnie muszą zwrócić wynik, dlatego sensowne jest przynajmniej częściowe posortowanie beczek, tak aby najważniejsze strony (których użytkownik prawdopodobnie będzie szukał) znajdowały się na początku beczki, a cała reszta na jej końcu.

StopLead
---------

Może Ci się wydawać, że w przypadku niektórych słów beczki mogą być bardzo duże i dlatego trudno z nimi pracować. Na przykład słowo `a` lub `1` występuje w ponad połowie dokumentów, a mimo to nie mają one żadnego znaczenia dla osoby wyszukującej (ponieważ są rzadko wyszukiwane i wymagają większej liczby słów otaczających). Takie słowa są nazywane stoperami (ang. Stopwords).

Problem ze StopWords polega na tym, że nie da się ich wszystkich wyszukać, a wyszukiwarki pokazują jedynie zniekształconą rzeczywistość tego, jak wygląda Internet dla wyszukiwanego słowa.

Przykładem wyszukiwania z użyciem StopSlovy jest zapytanie `Prague 1`.

Dla tego zapytania wyszukiwarka podaje tylko `3 020 000 wyników`. W rzeczywistości jednak takich stron jest znacznie więcej. Jednak ze względów wydajnościowych nie wszystkie z nich są przeszukiwane.

Wyszukaj opcje dostępu za pomocą programu StopSlovy:

- Nie indeksować ich w ogóle i nie robić dla nich beczek (tak robią małe wyszukiwarki)
- Indeksowanie ich, ale przechowywanie tylko odpowiednich stron w beczkach (tak robi np. Google i Seznam)
- Indeksować je jako zwykłe słowa i tworzyć kompletne beczki (to rozwiązanie ma ten problem, że beczki mogą łatwo przekroczyć rozmiar zwykłego dysku twardego i dlatego nie mogą być zapisane, a ich odczytanie może zająć kilka sekund - to rozwiązanie nie jest zatem stosowane w praktyce lub tylko w małych i wyspecjalizowanych wyszukiwarkach)

Ogólnie rzecz biorąc, nie ma uniwersalnie poprawnego podejścia i nawet Google nie stosuje go idealnie. Jest to tak duży problem, że jego rozwiązanie może zająć całe życie. Dla celów niniejszego opracowania wystarczy jednak ten ogólny opis.

Crawler (robot, także pająk)
---------------------------

Crawler to program, który systematycznie przeszukuje Internet i śledzi, jakie słowa pojawiają się na jakich stronach, a także gromadzi informacje pomocnicze (zwane również metainformacjami). Crawler zazwyczaj działa na wielu serwerach, ponieważ przeszukiwanie Internetu jest wymagające pod względem przepustowości sieci, dlatego też wiele stron musi być przeszukiwanych jednocześnie. Google szacuje, że jednocześnie indeksowanych jest do 30 000 stron.

Przed otwarciem strony Crawler przegląda bazę danych zawierającą adresy, które zamierza odwiedzić w przyszłości. Jeśli już odwiedził dany adres, zaktualizuje tylko swoje rekordy (na główne witryny wchodzi co kilka godzin, na witryny informacyjne co kilka minut, na zwykłe witryny najwyżej raz w miesiącu).

Jeśli robot trafi na nowy adres, którego nie zna, przejrzy zawarte w nim odnośniki do innych dokumentów (stron). Dla każdego łącza obliczana jest jego ważność, a jeśli jest to ważne łącze, pojawia się ono później na stronie.

Na przykład Lista przeszukuje w ten sposób tylko kilka poziomów łączy w każdej witrynie, natomiast Google stara się przeszukiwać całą witrynę, łącznie z nieistotnymi dokumentami, dzięki czemu jest najbardziej wszechstronną wyszukiwarką w Internecie.
Robot próbuje również uszeregować stronę w rankingu podczas jej przeszukiwania i oblicza jej "rangę", która jest liczbową reprezentacją tego, jak ważna jest dana strona w Internecie. W przeszłości Google używało wskaźnika PageRank, który ma zakres od 0 do 10. Google oblicza PageRank 10 tylko dla siebie i dla witryn takich jak microsoft.com czy w3c.org. Obecnie oblicza wartość w inny sposób (sztuczna inteligencja i matematyka wektorowa).

Najwyższy PageRank w Republice Czeskiej ma strona Seznam.cz z wartością 7. Ranga ma duży wpływ na to, jak często robot będzie odwiedzał stronę i jak wysoko znajdzie się ona w wynikach wyszukiwania. Rangi są obliczane na podstawie backlinków wskazujących na daną stronę.

Crawler nie robi nic innego z danymi, tylko pobiera je z Internetu i zapisuje w bazie danych, gdzie oczekują na proces indeksowania.
