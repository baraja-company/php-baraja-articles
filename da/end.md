End()
=====

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	da: end
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Understøtter PHP 4, PHP 5
- Kort beskrivelse: Sætter den interne pointer i et array til dets sidste element.
- Krav.

Beskrivelse
--------------------------

Funktionen `end()` sætter en intern array-pointer til det sidste element og returnerer værdien af dette.

Lignende funktioner
--------------------------

- `current()`
- `each()`
- `prev()`
- <a href="/reset">reset()</a>
- `næste()`

Eksempel
--------------------------

```php
echo end([
    'æble',
    'banan',
    'tranebær',
]); // skriver tranebær ud
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // udskriver 0
```

Parametre
--------------------------

| # | Type | Beskrivelse |
| --- | ------- | ----- |
| 1 | `array` | Navnet på det array, der skal arbejdes med. |

Returneringsværdier
--------------------------

Returnerer værdien af det sidste element eller `false` for et tomt array.

Forskelle fra tidligere versioner
--------------------------

Ingen
