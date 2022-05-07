Komplexität der Algorithmen
===========================

> id: f0baa25a-4cff-4d3d-b758-5e03f4a8c69c
> slug:
> 	- null
> 	de: komplexitaet-der-algorithmen
> 
> cs: slozitost-algoritmu
> publicationDate: '2021-08-03 20:40:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '6269d38b9a2a8d75ec01b569af8b371c'

Jeder Algorithmus hat seine eigene Komplexität, die in mathematischer Notation ausgedrückt werden kann. Diese Übersicht zeigt die typische Komplexität von Algorithmen in Abhängigkeit von der Größe der Eingabedaten (d. h. der Anzahl der Elemente, mit denen sie arbeiten) und welche Arten von Algorithmen für welche Art von Aufgabe geeignet sind.

Im Allgemeinen gibt es für jede Art von Problem einen besten spezialisierten Algorithmus. Kein Algorithmus ist universell am besten, und man muss immer den Kontext kennen.

Big-O-Notation
--------------

Die *Big O Notation* wird verwendet, um Algorithmen danach zu klassifizieren, wie ihr Laufzeit- oder Speicherbedarf mit zunehmender Eingabegröße steigt.

Das folgende Diagramm zeigt die häufigsten Wachstumsordnungen von Algorithmen, die in Big-O-Notation angegeben sind.

Nachfolgend finden Sie eine Liste der am häufigsten verwendeten Big-O-Notationen und einen Vergleich ihrer Leistung in Bezug auf verschiedene Eingabedatengrößen.

| Big O Notation | Komplexität für 10 Elemente | Komplexität für 100 Elemente | Komplexität für 1000 Elemente |
| -------------- | ---------------------------- | ----------------------------- | ------------------------------- |
| **O(1)** | 1 | 1 | 1 |
| **O(log N)** | 3 | 6 | 9 |
| **O(N)** | 10 | 100 | 1000 |
| **O(N log N)** | 30 | 600 | 9000 |
| **O(N^2)** | 100 | 10000 | 1000000 |
| **O(2^N)** | 1024 | 1.26e+29 | 1.07e+301 |
| **O(N!)** | 3628800 | 9.3e+157 | 4.02e+2567 |

Komplexität von Datenstrukturoperationen
----------------------------------

| Datenstruktur | Zugriff | Suchen | Einfügen | Entfernen | Kommentar |
| ----------------------- | :-------: | :-------: | :-------: | :-------: | :-------- |
| **Array** | 1 | n | n | n | n | | |
| **Stapel** | n | n | n | 1 | 1 | | |
| **Warteschlange** | n | n | n | 1 | | |
| **Verknüpfte Liste** | n | n | n | 1 | n |
| **Hashtabelle** | - | n | n | n | n | | Im Falle einer perfekten Hash-Funktion ist die Komplexität O(1) |
| **Binärer Suchbaum** | n | n | n | n | Im Falle eines ausgeglichenen Baums ist die Komplexität O(log(n)).
| **B-Baum** | log(n) | log(n) | log(n) | log(n) | log(n) | log(n) | |
| **Red-Black Tree** | log(n) | log(n) | log(n) | log(n) | |
| **AVL-Baum** | log(n) | log(n) | log(n) | log(n) | log(n) | |
| **Bloom-Filter** | - | 1 | 1 | - | Bei der Suche nach "falsch positiven Ergebnissen" |

Komplexität von Sortieralgorithmen
----------------------------

| Name des Algorithmus | Bester | Durchschnitt | Schlechtester | Speicher | Stabil? | Kommentar |
| --------------------- | :-------------: | :-----------------: | :-----------------: | :-------: | :-------: | :-------- |
**Bubble sort** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | Ja | |
| **Einfügungssortierung** | n | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
| **Auswahlsortierung** | n<sup>2</sup> | n<sup>2</sup> | n<sup>2</sup> | 1 | | |
**Heap sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | 1 | | Nein |
**Merge sort** | n&nbsp;log(n) | n&nbsp;log(n) | n&nbsp;log(n) | n | Ja | |
| **Quicksort** | n&nbsp;log(n) | n&nbsp;log(n) | n<sup>2</sup> | log(n) | Nein | Quicksort wird in der Regel mit O(log(n)) Stack-Komplexität durchgeführt. |
| **Shell sort** | n&nbsp;log(n) | hängt von der Reihenfolge ab | n&nbsp;(log(n))<sup>2</sup> | 1 | | Nein |
**Zählende Sortierung** | n + r | n + r | n + r | n + r | n + r | Ja | r - die größte Zahl in der Reihe |
**Radix-Sortierung** | n * k | n * k | n * k | n + k | Ja | k - Länge des längsten Schlüssels |
