Pure funkce - čisté funkce v PHP
================================

> id: "d94c18a9-8bc4-4377-8042-e7c8b48320a2"
> slug:
> 	cs: pure-funkce
> 
> publicationDate: "2021-10-27 10:30:00"
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3

Ve funkcionálním programování existuje pojem tzv. **čisté funkce** (anglicky **pure function**), což označuje funkci, která na stejný vstup vrací vždy stejný výstup (tedy je deterministická), a zároveň netrpí žádnými side-efekty (tedy neovlivňuje své okolí).

Jak vypadá pure funkce
----------------------

Příklad pure funkce:

```php
// This is a pure function
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Jedná se o čistou funkci, protože výstup je vždy stejný na základě vstupních argumentů.

Co není pure funkce
-------------------

```php
// This is an impure function
function add(int $a, int $b): int
{
	echo 'Adding...';
	file_put_contents('file.txt', 'value: ' . $a);
	return $a + $b;
}
```

Tento typ funkce není čistý, protože funkce mění souborový systém. Další typ nečisté funkce je případ, kdy interaguje s databází, vypisuje na obrazovku a podobně.
