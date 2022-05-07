Čisté funkcie v PHP
===================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	sk: ciste-funkcie-v-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

Vo funkcionálnom programovaní existuje pojem **čistá funkcia**, ktorý označuje funkciu, ktorá na rovnaký vstup vracia vždy rovnaký výstup (t. j. je deterministická) a zároveň nemá žiadne vedľajšie účinky (t. j. neovplyvňuje svoje okolie).

Ako vyzerá čistá funkcia
----------------------

Príklad čistej funkcie:

```php
// Toto je čistá funkcia
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Ide o čistú funkciu, pretože výstup je vždy rovnaký na základe vstupných argumentov.

Čo nie je čistá funkcia
-------------------

```php
// Toto je nečistá funkcia
function add(int $a, int $b): int
{
	echo "Pridanie...;
	file_put_contents('file.txt', 'hodnota: ' . $a);
	return $a + $b;
}
```

Tento typ funkcie nie je čistý, pretože funkcia mení systém súborov. Ďalším typom nečistej funkcie je interakcia s databázou, výpis na obrazovku a podobne.
