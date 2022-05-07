Zložitosť algoritmov
====================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	sk: zlozitost-algoritmov
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '6269d38b9a2a8d75ec01b569af8b371c'

Každý algoritmus má svoju vlastnú zložitosť, ktorú možno vyjadriť matematickým zápisom. Tento prehľad ukazuje typickú zložitosť algoritmov v závislosti od veľkosti vstupných údajov (t. j. počtu prvkov, s ktorými pracujú) a od toho, ktoré typy algoritmov sú vhodné pre ktorý typ úlohy.

Vo všeobecnosti existuje najlepší špecializovaný algoritmus pre každý typ problému. Žiadny algoritmus nie je univerzálne najlepší a vždy musíte poznať kontext.

Zápis Big O
--------------

Zápis *Big O* sa používa na klasifikáciu algoritmov podľa toho, ako sa ich požiadavky na čas behu alebo pamäť zvyšujú s rastúcou veľkosťou vstupu.

Nasledujúca tabuľka zobrazuje najčastejšie príkazy rastu algoritmov zadaných v notácii Big O.

Nižšie je uvedený zoznam niektorých najčastejšie používaných zápisov Big O a porovnanie ich výkonnosti vzhľadom na rôzne veľkosti vstupných údajov.

| Big O Notation | Zložitosť pre 10 prvkov | Zložitosť pre 100 prvkov | Zložitosť pre 1000 prvkov |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
| **O(log N)** | 3 | 6 | 9 |
| **O(N)** | 10 | 100 | 1000 |
| **O(N log N)** | 30 | 600 | 9000 |
| **O(N^2)** | 100 | 10000 | 1000000 |
| **O(2^N)** | 1024 | 1,26e+29 | 1,07e+301 |
| **O(N!)** | 3628800 | 9,3e+157 | 4,02e+2567 |

Zložitosť operácií s dátovými štruktúrami
----------------------------------

| Dátová štruktúra | Prístup | Vyhľadávanie | Vloženie | Odstránenie | Komentár |
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| **Array** | 1 | n | n | n | n | |
| **Stack** | n | n | n | 1 | 1 | |
| **Queue** | n | n | n | 1 | | | |
| **Soznam odkazov** | n | n | n | 1 | n |
| **Hash tabuľka** | - | n | n | n | n | V prípade dokonalej hash funkcie bude zložitosť O(1) |
| **Binárny vyhľadávací strom** | n | n | n | n | V prípade vyváženého stromu bude zložitosť O(log(n)). |
| **B-strom** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) |
| **Červeno-čierny strom** | log(n) | log(n) | log(n) | log(n) |
| **AVL Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | |
| **Filter rozkvetu** | - | 1 | 1 | - | Pri vyhľadávaní `falošne pozitívnych výsledkov` |

Zložitosť triediacich algoritmov
----------------------------

| Názov algoritmu | Najlepší | Priemerný | Najhorší | Pamäť | Stabilný? | Komentár |
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
| **Bubble sort** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Áno | |
| **Triedenie vkladania** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | | | |
| **Triedenie výberu** | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
| **Heap sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | Nie |
| **Spojiť triedenie** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Áno | |
| | **Quick sort** | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | Nie | Quicksort sa zvyčajne vykonáva so zložitosťou O(log(n)).
| **Shell sort** | n&nbsp;log(n) | závisí od postupnosti | n&nbsp;(log(n))<sup>2</sup> | 1 | | Nie |
| **Triedenie podľa počtu** | n + r | n + r | n + r | n + r | Áno | r - najväčšie číslo v poli |
| **Radix sort** | n * k | n * k | n * k | n + k | Áno | k - dĺžka najdlhšieho kľúča |
