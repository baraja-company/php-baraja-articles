Drukuj
======

> id: '23d672c5-b347-43b8-bd08-8ccb7ecca33f'
> slug:
> 	cs: print
> 	pl: drukuj
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: b7f7c3c32d9f3c1efd794ca38b7db384

print` - wyjście typu String

Opis
--------------------------

```php
print 'Witaj, świecie!';
```

print()` nie jest prawdziwą funkcją (jest to konstrukcja językowa), więc nie trzeba używać nawiasów.

Parametry
--------------------------

- **arg**

Parametr wyjściowy

Wartości zwracane
--------------------------

Zawsze zwraca liczbę 1.

Notatka
--------------------------

Uwaga: Ponieważ jest to **konstrukcja językowa** (a nie funkcja), nie można jej wczytać do zmiennej.

Przykład
--------------------------

```php
print "Witaj, świecie";

print "print" może wyświetlać wiele wierszy tekstu. Ale uważaj na znacznik HTML
ponieważ nie chce się wydrukować. To właśnie to <a href="/nl2br">Nl2br</a>.";

// Przykład połączenia ze zmienną
$a = 'php';

print 'Podoba mi się' . $a; // Lubię php
```

Funkcja **print** jest dokładnie taka sama jak funkcja **echo**. Aby uzyskać więcej informacji, zapoznaj się z artykułem <a href="/echo">echo</a>.
