Fine()
======

> id: '879c7c9a-a497-4080-865d-f15e6c9e8578'
> slug:
> 	cs: end
> 	it: fine
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d097dd1fae4e4f125d879c92bc2cfbe'

- Supporto PHP 4, PHP 5
- Descrizione breve: imposta il puntatore interno di un array al suo ultimo elemento.
- Requisiti.

Descrizione
--------------------------

La funzione `end()` imposta un puntatore interno all'array all'ultimo elemento e restituisce il suo valore.

Funzioni simili
--------------------------

- `corrente()`
- `each()`
- `prev()`
- <a href="/reset">reset()</a>
- `next()`

Esempio
--------------------------

```php
echo end([
    'mela',
    'banana',
    'mirtillo rosso',
]); // scrive il mirtillo rosso
```



```php
$a = [];
$a[1] = 1;
$a[0] = 0;

echo end($a); // stampa 0
```

Parametri
--------------------------

| # | Tipo | Descrizione |
| --- | ------- | ----- |
| 1 | `array` | Il nome dell'array con cui lavorare.

Valori di ritorno
--------------------------

Restituisce il valore dell'ultimo elemento o `false` per un array vuoto.

Differenze dalle versioni precedenti
--------------------------

Nessuno
