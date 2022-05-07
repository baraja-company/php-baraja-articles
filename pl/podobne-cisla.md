Podobne liczby - jak je rozpoznać
=================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	pl: podobne-liczby---jak-je-rozpoznac
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

W przeszłości w tym artykule opisywaliśmy metody rozpoznawania podobnych liczb.

Na przykład, `500 199 Kč` i `500 210 Kč` są prawie takie same.

Rozwiązaniem jest obliczenie proporcji i porównanie jej z epsilonem.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // Liczby są podobne
}
```
