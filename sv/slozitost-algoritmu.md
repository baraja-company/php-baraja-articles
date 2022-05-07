Algoritmernas komplexitet
=========================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	sv: algoritmernas-komplexitet
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '6269d38b9a2a8d75ec01b569af8b371c'

Varje algoritm har sin egen komplexitet, som kan uttryckas i matematisk notation. Denna översikt visar algoritmernas typiska komplexitet beroende på storleken på indata (dvs. antalet element som de arbetar med) och vilka typer av algoritmer som är lämpliga för vilken typ av uppgift.

I allmänhet finns det en bästa specialiserad algoritm för varje typ av problem. Ingen algoritm är alltid bäst och du måste alltid känna till ditt sammanhang.

Big O-notation
--------------

*Big O-notationen* används för att klassificera algoritmer efter hur deras körtids- eller minneskrav ökar med ökande storlek på indata.

Följande diagram visar de vanligaste tillväxtordningarna för algoritmer som anges i Big O-notation.

Nedan följer en förteckning över några av de vanligaste Big O-notationerna och en jämförelse av deras prestanda med avseende på olika storlekar på indata.

| Big O Notation | Komplexitet för 10 element | Komplexitet för 100 element | Komplexitet för 1000 element |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
| **O(log N)** | 3 | 6 | 9 |
| **O(N)** | 10 | 100 | 1000 |
| **O(N log N)** | 30 | 600 | 9000 |
| **O(N^2)** | 100 | 10000 | 1000000 |
| **O(2^N)** | 1024 | 1.26e+29 | 1.07e+301 |
| **O(N!)** | 3628800 | 9.3e+157 | 4.02e+2567 |

Komplexiteten hos datastrukturoperationer
----------------------------------

| Datastruktur | Åtkomst | Sök | Infoga | Ta bort | Kommentera |
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| **Array** | 1 | n | n | n | n | n | n | n | n | | |
| **Stack** | n | n | n | n | n | 1 | 1 | | | | |
| **Queue** | n | n | n | n | n | 1 | | | | | |
| **Länkad lista** | n | n | n | n | n | 1 | n | n |
| **Hashtabell** | - | n | n | n | n | n | n | n | I fallet med en perfekt hashfunktion blir komplexiteten O(1) |
| **Binary Search Tree** | n | n | n | n | n | n | n | n | I fallet med ett balanserat träd blir komplexiteten O(log(n)). |
| **B-Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | | |
| **Red-Black Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | | |
| **AVL Tree** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | | |
| **Bloom Filter** | - | 1 | 1 | 1 | - | | Vid sökning efter "falska positiva" |

Komplexitet hos sorteringsalgoritmer
----------------------------

| Algoritmens namn | Bäst | Medel | Sämst | Minne | Stabilt? | Kommentar |
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
| **Bubbelsortering** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Ja | | |
| **Insättningssortering** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | | | | |
| **Väljningssortering** | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | | |
| **Heap sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | | Nej |
| **Merge sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Ja | | |
| | **Snabbsortering** | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | Nej | Snabbsortering utförs vanligen med O(log(n))-komplexitet i stacken. |
| **Shell sort** | n&nbsp;log(n) | beror på sekvensen | n&nbsp;(log(n))<sup>2</sup> | 1 | | | | Nej |
| **Räkningssortering** | n + r | n + r | n + r | n + r | n + r | n + r | Ja | r - det största talet i matrisen |
| **Radix-sortering** | n * k | n * k | n * k | n * k | n + k | Ja | k - längden på den längsta nyckeln |
