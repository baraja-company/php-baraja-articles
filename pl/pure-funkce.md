Czyste funkcje w PHP
====================

> id: d94c18a9-8bc4-4377-8042-e7c8b48320a2
> slug:
> 	cs: pure-funkce
> 	pl: czyste-funkcje-w-php
> 
> publicationDate: '2021-10-27 10:30:00'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: e47a3507ddb189a218ad8e3a38703e5c

W programowaniu funkcyjnym istnieje pojęcie **czystej funkcji**, która odnosi się do funkcji, która zawsze zwraca to samo wyjście na to samo wejście (tzn. jest deterministyczna), a jednocześnie nie ma żadnych skutków ubocznych (tzn. nie wpływa na swoje otoczenie).

Jak wygląda czysta funkcja
----------------------

Przykład czystej funkcji:

```php
// To jest czysta funkcja
function add(int $a, int $b): int
{
	return $a + $b;
}
```

Jest to czysta funkcja, ponieważ wynik jej działania jest zawsze taki sam na podstawie argumentów wejściowych.

Co nie jest czystą funkcją
-------------------

```php
// To jest nieczysta funkcja
function add(int $a, int $b): int
{
	echo 'Dodając...';
	file_put_contents('plik.txt', 'Wartość:' . $a);
	return $a + $b;
}
```

Tego typu funkcje nie są czyste, ponieważ zmieniają one system plików. Innym rodzajem nieczystej funkcji jest interakcja z bazą danych, drukowanie na ekranie itd.
