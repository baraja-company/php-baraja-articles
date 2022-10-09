Algoritmernes kompleksitet
==========================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	da: algoritmernes-kompleksitet
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '6269d38b9a2a8d75ec01b569af8b371c'

Hver algoritme har sin egen kompleksitet, som kan udtrykkes i matematisk notation. Denne oversigt viser algoritmernes typiske kompleksitet i forhold til størrelsen af inputdataene (dvs. antallet af elementer, de arbejder med), og hvilke typer algoritmer der egner sig til hvilken type opgave.

Generelt findes der en bedste specialiseret algoritme for hver type problem. Ingen algoritme er altid den bedste, og du skal altid kende din kontekst.

Big O Notation
--------------

*Big O-notationen* bruges til at klassificere algoritmer efter, hvordan deres løbetid eller hukommelseskrav stiger med stigende inputstørrelse.

Følgende skema viser de mest almindelige vækstordner for algoritmer, der er specificeret i Big O-notation.

Nedenfor er der en liste over nogle af de mest almindeligt anvendte Big O-notationer og en sammenligning af deres ydeevne med hensyn til forskellige inputdatastørrelser.

| Big O Notation | Kompleksitet for 10 elementer | Kompleksitet for 100 elementer | Kompleksitet for 1000 elementer |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
| **O(log N)** | 3 | 6 | 9 | 9 |
| **O(N)** | 10 | 100 | 1000 | 1000 |
| **O(N log N)** | 30 | 600 | 9000 | 9000 |
| **O(N^2)** | 100 | 10000 | 1000000 | 1000000 |
| **O(2^N)** | 1024 | 1.26e+29 | 1.07e+301 |
| **O(N!)** | 3628800 | 9.3e+157 | 4.02e+2567 |

Kompleksitet af datastrukturoperationer
----------------------------------

| Datastruktur | Adgang | Søg | Indsæt | Fjern | Kommentar |
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| **Array** | 1 | n | n | n | n | n | n | n | n | | | |
| **Stack** | n | n | n | n | n | 1 | 1 | 1 | | | |
| **Queue** | n | n | n | n | n | n | 1 | | | | | |
| **Linked List** | n | n | n | n | n | n | 1 | n | n |
| **Hash Table** | - | | n | n | n | n | n | n | n | n | I tilfælde af en perfekt hashfunktion vil kompleksiteten være O(1) |
| **Binary Search Tree** | n | n | n | n | n | n | n | n | n | I tilfælde af et balanceret træ vil kompleksiteten være O(log(n)). |
| **B-Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | | |
| **Rødt-sort træ** | log(n) | log(n) | log(n) | log(n) | log(n) | | |
| **AVL Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | | |
| **Bloom Filter** | - | | 1 | 1 | 1 | - | | Ved søgning efter "falske positiver" |

Sorteringsalgoritmers kompleksitet
----------------------------

| Algoritmens navn | Bedste | Gennemsnitlig | Værste | Hukommelse | Stabil? | Kommentar |
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
| **Bubble sort** | n | n | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | Ja | | |
| **Insættelsessortering** | n | n | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | | | |
| **Selektionssortering** | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | | | |
| **Heap sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | | Nej |
| **Sammenlægningssortering** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Ja | | |
| **Quick sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | Nej | Quicksort udføres normalt med O(log(n)) stakkompleksitet. |
| **Shell sort** | n&nbsp;log(n) | afhænger af sekvensen | n&nbsp;(log(n))<sup>2</sup> | 1 | | | | Nej |
| **Tællesortering** | n + r | n + r | n + r | n + r | n + r | n + r | Ja | r - det største tal i arrayet |
| **Radix-sortering** | n * k | n * k | n * k | n * k | n + k | Ja | k - længden af den længste nøgle |
