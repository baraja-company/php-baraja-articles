Slut()
======

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	sv: slut
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Stöd för PHP 4, PHP 5
- Kort beskrivning: Sätter den interna pekaren i en array till dess sista element.
- Krav.

Beskrivning
--------------------------

Funktionen `end()` sätter en intern arraypekare till det sista elementet och returnerar dess värde.

Liknande funktioner
--------------------------

- `current()`
- `each()`
- `prev()`
- <a href="/reset">reset()</a>
- `nästa()`

Exempel
--------------------------

```php
echo end([
    'äpple',
    'banan',
    'tranbär',
]); // skriver ut tranbär
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // skriver ut 0
```

Parametrar
--------------------------

| # | Typ | Beskrivning |
| --- | ------- | ----- |
| 1 | `array` | Namnet på den array som ska användas. |

Returvärden
--------------------------

Återger värdet av det sista elementet eller `false` för en tom array.

Skillnader från tidigare versioner
--------------------------

Ingen
