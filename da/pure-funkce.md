Rene funktioner i PHP
=====================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	da: rene-funktioner-i-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

Inden for funktionel programmering findes der et begreb **ren funktion**, som henviser til en funktion, der altid returnerer det samme output til det samme input (dvs. er deterministisk), og som samtidig ikke lider under nogen sideeffekter (dvs. ikke påvirker omgivelserne).

Sådan ser en ren funktion ud
----------------------

Eksempel på en ren funktion:

```php
// Dette er en ren funktion
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Dette er en ren funktion, fordi output altid er det samme baseret på input-argumenterne.

Hvad er ikke en ren funktion
-------------------

```php
// Dette er en urene funktion
function add(int $a, int $b): int
{
	echo 'Tilføjelse af...';
	file_put_contents('file.txt', 'Værdi:' . $a);
	return $a + $b;
}
```

Denne type funktion er ikke ren, fordi funktionen ændrer filsystemet. En anden type urene funktioner er, når de interagerer med databasen, udskriver på skærmen osv.
