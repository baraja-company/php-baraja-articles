PHP Include - wstawianie pliku do strony
========================================

> id: '7a53145c-8552-425d-b864-283f73a7a7de'
> slug:
> 	cs: include
> 	pl: php-include---wstawianie-pliku-do-strony
> 
> publicationDate: '2019-09-11 10:07:07'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '6e18e67ec03b3a7592473230559fc364'

Konstrukcja `include` automatycznie wstawia dodatkowe pliki/skrypty do bieżącej strony.

Jeśli chodzi o PHP, zostanie on automatycznie wykonany i zrozumiany tak, jakby zawsze znajdował się w tym miejscu.

Przykład:

```php
include 'news.html';
```

Praktyczne zastosowanie
-----------------

- Wspólne menu i menu,
- Wiadomości, aktualizacje, wiadomości i te same treści,
- Nagłówek lub stopka, ...

Dynamiczne ładowanie plików
--------------------------

Często musimy wczytywać pliki dynamicznie, na przykład na podstawie zmiennej.

Na przykład:

```php
$clanek = 'neco';

include $clanek . '.html';
```

Możemy też dynamicznie wczytywać artykuły na stronę:

```php
include 'artykuły/' . $_GET['strona'] . '.html';
```

Po wywołaniu adresu URL z parametrem `strona` automatycznie wstawiany jest artykuł, na przykład: `https://example.com/?page=novinky`.
