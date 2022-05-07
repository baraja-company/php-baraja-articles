Złożoność algorytmów
====================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	pl: zlozonosc-algorytmow
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '6269d38b9a2a8d75ec01b569af8b371c'

Każdy algorytm ma swoją własną złożoność, którą można wyrazić w notacji matematycznej. W tym zestawieniu pokazano typową złożoność algorytmów w zależności od wielkości danych wejściowych (tj. liczby elementów, z którymi pracują) oraz to, jakie typy algorytmów są odpowiednie do danego typu zadań.

Ogólnie rzecz biorąc, dla każdego typu problemu istnieje najlepszy wyspecjalizowany algorytm. Żaden algorytm nie jest uniwersalnie najlepszy i zawsze trzeba znać kontekst.

Notacja Big O
--------------

Notacja *Big O* służy do klasyfikowania algorytmów na podstawie tego, jak ich czas działania lub wymagania pamięciowe rosną wraz ze wzrostem rozmiaru danych wejściowych.

Na poniższym wykresie przedstawiono najczęściej spotykane rzędy wzrostu algorytmów określonych w notacji Big O.

Poniżej znajduje się lista kilku najczęściej używanych notacji Big O oraz porównanie ich wydajności w odniesieniu do różnych rozmiarów danych wejściowych.

| Notacja Big O | Złożoność dla 10 elementów | Złożoność dla 100 elementów | Złożoność dla 1000 elementów |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
| **O(log N)** | 3 | 6 | 9 |
**O(N)** | 10 | 100 | 1000 |
| **O(N log N)** | 30 | 600 | 9000 |
**O(N^2)** | 100 | 10000 | 1000000 |
| **O(2^N)** | 1024 | 1.26e+29 | 1.07e+301 |
**O(N!)** | | 3628800 | 9.3e+157 | 4.02e+2567 |

Złożoność operacji na strukturach danych
----------------------------------

| Struktura danych | Dostęp | Wyszukiwanie | Wstawianie | Usuwanie | Komentarz |
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| 1 | n | n | n | n | |
**Stack** | n | n | n | 1 | 1 | |
**Queue** | n | n | n | 1 | | | |
| n | n | 1 | n | 1 | n |
| | n | n | n | n | W przypadku doskonałej funkcji skrótu złożoność wynosi O(1).
**Binarne drzewo poszukiwań** | n | n | n | W przypadku zrównoważonego drzewa złożoność wyniesie O(log(n)). |
| **B-Drzewo** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | |
**Drzewo czerwono-czarne** | log(n) | log(n) | log(n) | log(n) | log(n) | |
| **AVL Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | |
| | 1 | 1 | - | Przy wyszukiwaniu `fałszywych pozytywów` |

Złożoność algorytmów sortowania
----------------------------

| Nazwa algorytmu | Najlepszy | Średni | Najgorszy | Pamięć | Stabilny? | Komentarz |
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
| **Sortowanie bąbelkowe** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Tak | |
| **Insertion sort** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | | | |
| | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | | |
| **Heap sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | Nie |
| **Merge sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Tak | |
| **Quick sort** | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | Nie | Quicksort jest zwykle wykonywany z złożonością stosu O(log(n)). |
| n&nbsp;log(n) | zależy od sekwencji | n&nbsp;(log(n))<sup>2</sup> | 1 | | Nie |
| n + r | n + r | n + r | n + r | n + r | Tak | r - największa liczba w tablicy |
| n * k | n * k | n * k | n + k | Tak | k - długość najdłuższego klucza |
