Lokalnie wrażliwe haszowanie
============================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	pl: lokalnie-wrazliwe-haszowanie
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

Zasadą większości funkcji haszujących odciski palców dokumentów jest to, że zawsze zwracają to samo wyjście dla każdego wejścia. To się nazywa zachowanie deterministyczne. Jednocześnie mała zmiana na wejściu spowoduje dużą zmianę na wyjściu (skutecznie zwracając zupełnie inny hash). Jest to szczególnie przydatne, gdy chcemy sprawdzić, czy dokument wejściowy się zmienił, a jeśli tak, otrzymujemy coś zupełnie innego. Przykładem takiej funkcji jest MD5 lub SHA1.

Jeśli chcemy wywnioskować zawartość oryginalnego wejścia z hasha, nie ma łatwego sposobu, aby to zrobić i nie mamy wyboru, jak tylko wymuszać każdą opcję, dopóki nie uzyskamy trafienia. Dzieje się tak dlatego, że ze względu na dużą zmianę danych wyjściowych nie możemy określić przez iteracje, czy zbliżamy się do celu, czy nie.

Niektóre funkcje haszujące, takie jak BCrypt dla tego samego wejścia, zwrócą inne wyjście za każdym razem, aby uczynić je odpornymi na haszowanie haseł. W rzeczywistości wyjście bezpośrednio zawiera sól, co uniemożliwia tzw. atak słownikowy. Ta funkcja jest przydatna tylko do hashowania haseł, ale bardzo nie nadaje się do przeglądania dokumentów.

Sprawdzanie podobieństwa dokumentów i wyszukiwanie duplikatów treści
-----------------------------------------------------------

Wyobraźmy sobie, że rozwiązujemy algorytm wyszukiwarki internetowej, która chce sprawdzić, jak bardzo zmieniła się strona internetowa. Ewentualnie chce szybko sprawdzić, czy tekst z jednej strony jest podobny do tekstu na innej stronie lub nawet całkowicie zduplikowany.

Jednym ze sposobów sprawdzenia tego jest porównanie każdej strony ze sobą, co jest bardzo wymagające pod względem zasobów systemowych. Inną opcją jest obliczenie hasha SHA1, ale to haszuje zawartość całego dokumentu, a jeśli strona zmieni się tylko o jeden znak, otrzymamy inny hash - więc nie nadaje się do tych celów.

Dlatego jednym z możliwych rozwiązań jest obliczanie hasha z różnych obszarów dokumentu w różny sposób, a następnie szukanie zmian na wyjściu funkcji haszującej.

Zasada haszowania wrażliwego na lokalizację
----------------------------------

Pobraliśmy kod HTML strony internetowej, dla której chcemy obliczyć hash. Dokument dzielimy na różne regiony (komórki) za pomocą algorytmu, który musi być odpowiednio skonfigurowany.

Haszujemy każdy region niezależnie przy użyciu wspólnego algorytmu i konkatenujemy wynik w jeden ciąg.

Wyjściem może być wtedy na przykład `3791744029724361934` (ten hash treści został dostarczony przez narzędzie Ahrefs).

Na przykład, jeśli wiemy, że zawartość nagłówka reprezentuje pierwsze 5 znaków, a stopka reprezentuje ostatnie 5 znaków, wiemy, że środek ciągu reprezentuje zawartość strony. Jeśli treść stopki zmienia się na wszystkich stronach (na przykład webmaster dodaje nowy link, zmienia się data aktualizacji itp.), to po pobraniu nowej wersji strony zmienia się nieznacznie tylko kilka znaków w haszu w prawym obszarze i wiemy, że zmieniła się tylko stopka, ale treść pozostaje bez zmian. Możemy więc zignorować taką zmianę i nie musimy przechodzić przez całą stronę.

Jak Google optymalizuje indeksowanie stron internetowych?
----------------------------------------

Boty internetowe muszą oszczędzać gdzie się da. Przeszukiwanie stron internetowych jest bardzo kosztowne, a aktualizacje kosztują czas obliczeniowy.

Na przykład obserwując zachowanie algorytmu robotów Google, zauważyłem, że reaguje on tylko na duże zmiany w treści. Jeśli strona niewiele się zmienia, to indeksuje się w normalny sposób. Ale kiedy na przykład stopka i nagłówek witryny zmieniają się znacząco, ocenia to jako redesign i przechodzi przez większość witryny na raz, aby uzyskać nowy wygląd tak szybko, jak to możliwe.

W podobny sposób wykrywa również duplikaty w różnych witrynach. Kiedy grupa stron jest bardzo podobna lub ma tę samą treść, otrzymują bardzo podobny hash, który robot może wykorzystać do szybkiej weryfikacji, że dokumenty są podobne i może wybrać kanoniczny i zignorować resztę.

Implementacja funkcji haszowania
-----------------------------

W PHP nie ma gotowej implementacji funkcji haszującej locale-sensitive. Jednocześnie nie znam żadnego swobodnie dostępnego pakietu, który dobrze implementuje tę funkcję.
