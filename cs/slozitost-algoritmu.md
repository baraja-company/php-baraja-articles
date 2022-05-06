Složitost algoritmů
===================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
>   cs: slozitost-algoritmu
> 
> publicationDate: "2021-08-03 20:40:00"
> mainCategoryId: "1f73dcfa-92a9-4738-ab30-8cbfb00ad23b"

Každý algoritmus má svojí složitost, kterou můžeme vyjádřit matematickým zápisem. Tento přehled ukazuje typické složitosti algoritmů podle velikosti vstupních dat (tj. počtu prvků, s kterými pracují) a jaké typy algoritmů se hodí podle typu úlohy.

Obecně platí, že pro každý typ problému existuje nejlepší specializovaný algoritmus. Žádný algoritmus není univerzálně nejlepší, a vždy musíte znát váš kontext.

Big O Notation
--------------

*Big O Notation* se používá ke klasifikaci algoritmů podle toho, jak s rostoucí velikostí vstupu roste jejich doba běhu nebo paměťové nároky.

Na následujícím grafu můžete najít nejčastější řády růstu algoritmů specifikovaných v notaci Big O.

Níže je uveden seznam některých nejpoužívanějších notací Big O a srovnání jejich výkonnosti vzhledem k různým velikostem vstupních dat.

| Big O Notation | Složitost pro 10 elementů    | Složitost pro 100 elementů    | Složitost pro 1000 elementů     |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)**       | 1                            | 1                             | 1                               |
| **O(log N)**   | 3                            | 6                             | 9                               |
| **O(N)**       | 10                           | 100                           | 1000                            |
| **O(N log N)** | 30                           | 600                           | 9000                            |
| **O(N^2)**     | 100                          | 10000                         | 1000000                         |
| **O(2^N)**     | 1024                         | 1.26e+29                      | 1.07e+301                       |
| **O(N!)**      | 3628800                      | 9.3e+157                      | 4.02e+2567                      |

Složitost operací datové struktury
----------------------------------

| Struktura dat           | Přístup   | Hledání   | Vložení   | Odebráné  | Komentář |
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| **Array**               | 1         | n         | n         | n         |           |
| **Stack**               | n         | n         | 1         | 1         |           |
| **Queue**               | n         | n         | 1         | 1         |           |
| **Linked List**         | n         | n         | 1         | n         |           |
| **Hash Table**          | -         | n         | n         | n         | V případě dokonalé hashovací funkce bude složitost O(1) |
| **Binary Search Tree**  | n         | n         | n         | n         | V případě vyváženého stromu složitost bude O(log(n)). |
| **B-Tree**              | log(n)    | log(n)    | log(n)    | log(n)    |           |
| **Red-Black Tree**      | log(n)    | log(n)    | log(n)    | log(n)    |           |
| **AVL Tree**            | log(n)    | log(n)    | log(n)    | log(n)    |           |
| **Bloom Filter**        | -         | 1         | 1         | -         | Při hledání jsou možné tzv. `false positives` |

Složitost řadících algoritmů
----------------------------

| Název algoritmu       | Nejlepší        | Průměrná            | Nejhorší            | Paměť     | Stabilní? | Komentář  |
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
| **Bubble sort**       | n               | n<sup>2</sup>       | n<sup>2</sup>       | 1         | Ano       |           |
| **Insertion sort**    | n               | n<sup>2</sup>       | n<sup>2</sup>       | 1         | Ano       |           |
| **Selection sort**    | n<sup>2</sup>   | n<sup>2</sup>       | n<sup>2</sup>       | 1         | Ne        |           |
| **Heap sort**         | n&nbsp;log(n)   | n&nbsp;log(n)       | n&nbsp;log(n)       | 1         | Ne        |           |
| **Merge sort**        | n&nbsp;log(n)   | n&nbsp;log(n)       | n&nbsp;log(n)       | n         | Ano       |           |
| **Quick sort**        | n&nbsp;log(n)   | n&nbsp;log(n)       | n<sup>2</sup>       | log(n)    | Ne        | Quicksort se obvykle provádí se složitostí O(log(n)) zásobníku. |
| **Shell sort**        | n&nbsp;log(n)   | závisí na posloupnosti | n&nbsp;(log(n))<sup>2</sup>  | 1         | Ne         |           |
| **Counting sort**     | n + r           | n + r               | n + r               | n + r     | Ano       | r - největší číslo v poli |
| **Radix sort**        | n * k           | n * k               | n * k               | n + k     | Ano       | k - délka nejdelšího klíče |

