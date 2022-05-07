Ähnliche Zahlen - wie man sie erkennt
=====================================

> id: d1d911d1-a6cd-4a69-af17-e90ad1d95b6d
> slug:
> 	cs: podobne-cisla
> 	de: aehnliche-zahlen---wie-man-sie-erkennt
> 
> publicationDate: '2019-08-23 15:49:48'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: '07805228d14d5a91ba57a86914c3c8dd'

In der Vergangenheit wurden in diesem Artikel Methoden zur Erkennung ähnlicher Zahlen beschrieben.

So sind zum Beispiel "500 199 Kč" und "500 210 Kč" fast identisch.

Die Lösung besteht darin, den Anteil zu berechnen und mit Epsilon zu vergleichen.

```php
$x = 500199;
$y = 500210;
$epsilon = 0.001;

if (abs($x / $y) < $epsilon) {
    // Die Zahlen sind ähnlich
}
```
