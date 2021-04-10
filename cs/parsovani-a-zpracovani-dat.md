Parsování a zpracování dat v PHP
================================

> id: "7dfc991d-a7ee-4e2b-8159-16cef1d27483"
> slug:
> 	cs: parsovani-a-zpracovani-dat
> 
> publicationDate: "2017-10-15 09:54:02"
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec

> Tento článek bude pojednávat o metodách, jak v PHP zpracovávat data, ale zatím není hotový.

Tak dočasně jen rychlý nástřel možností:

- **Procházení znak po znaku** je hodně stará metoda, která znepřehledňuje kód, ale interně ji vykonávají všechny další metody
- <a href="/explode">Explode</a>, rozdělení stringu podle oddělovače
- <a href="/regex">**Regulární výrazy**</a> představují nejlepší způsob, jak zpracovat jednoduché řetězce
- **Tokenizér**, rozděluje složitý řetězec na kousky (tokeny) podle regulárních výrazů, třeba takto se zpracovává jazyk PHP
