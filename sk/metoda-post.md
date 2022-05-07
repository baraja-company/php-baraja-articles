Prijímanie údajov metódou POST
==============================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	sk: prijimanie-udajov-metodou-post
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

Odosielanie údajov pomocou POST je úplne odlišné od <a href="/method-get">GET</a>, je bezpečnejšie, text môže byť dlhší a jeho hodnota sa nedá určiť inak ako pomocou formulára alebo hlavičky (ktorú omylom nezískate).

Zdroj
--------------------------

Zdroj sa príliš nelíši od metódy <a href="/method-get">GET</a>. Je to takmer rovnaké, až na to, že parametre sa nezobrazujú v adrese URL, viditeľný je len názov súboru.

```php
echo $_POST["clanek] ?? '';
```

Charakteristika, výhody a nevýhody
--------------------------

- Parametre nie je možné prepojiť, ale je potrebné odoslať formulár
- Nemožno indexovať (súvisí s predchádzajúcim bodom)
- Bezpečnejšie ako <a href="/method-get">GET</a> (údaje sa odosielajú skrytým spôsobom, hodnoty sa nezobrazujú v histórii)
- Nezamieňajte s protokolom SSL, POST nie je šifrovaný.
