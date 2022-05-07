PHP Include - inserire un file in una pagina
============================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	it: php-include---inserire-un-file-in-una-pagina
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

Il costrutto `include` inserisce automaticamente file/script aggiuntivi nella pagina corrente.

Per quanto riguarda PHP, sarà automaticamente eseguito e compreso come se fosse sempre stato in quella posizione.

Esempio:

```php
include 'notizie.html';
```

Uso pratico
-----------------

- Menu comuni e menu,
- Notizie, aggiornamenti, novità e lo stesso contenuto,
- Intestazione o piè di pagina, ...

Caricamento dinamico dei file
--------------------------

Spesso abbiamo bisogno di caricare file dinamicamente, per esempio in base a una variabile.

Per esempio:

```php
$clanek = 'neco';

include $clanek . '.html';
```

Oppure possiamo caricare dinamicamente gli articoli nella pagina:

```php
include 'articoli/' . $_GET['pagina'] . '.html';
```

Quando si chiama un URL con il parametro `page`, l'articolo viene inserito automaticamente, per esempio: `https://example.com/?page=novinky`.
