Rena funktioner i PHP
=====================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	sv: rena-funktioner-i-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

Inom funktionell programmering finns begreppet **ren funktion**, som avser en funktion som alltid returnerar samma resultat till samma ingång (dvs. är deterministisk) och som samtidigt inte har några sidoeffekter (dvs. inte påverkar sin omgivning).

Hur en ren funktion ser ut
----------------------

Exempel på en ren funktion:

```php
// Detta är en ren funktion
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Detta är en ren funktion eftersom resultatet alltid är detsamma baserat på de ingående argumenten.

Vad är inte en ren funktion?
-------------------

```php
// Detta är en oren funktion
function add(int $a, int $b): int
{
	echo 'Tillägg av...';
	file_put_contents('file.txt', 'Värde:' . $a);
	return $a + $b;
}
```

Denna typ av funktion är inte renodlad eftersom funktionen ändrar filsystemet. En annan typ av oren funktion är när den interagerar med databasen, skriver ut på skärmen och så vidare.
