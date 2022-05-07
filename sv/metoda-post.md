Ta emot data med POST-metoden
=============================

> id: c2f273fa-7730-4521-82d0-1b3096269fac
> slug:
> 	cs: metoda-post
> 	sv: ta-emot-data-med-post-metoden
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '51a30cb759cdbb184ee5bb0bf3070af1'

Att skicka data med POST skiljer sig från <a href="/method-get">GET</a>, det är säkrare, texten kan vara längre och värdet kan inte bestämmas annat än med hjälp av ett formulär eller en rubrik (som du inte får av misstag).

Källa
--------------------------

Source skiljer sig inte så mycket från <a href="/method-get">GET</a>-metoden. Det är i stort sett samma sak, förutom att parametrarna inte visas i URL:en, utan endast filnamnet är synligt.

```php
echo $_POST['Artikel'] ?? '';
```

Egenskaper, fördelar och nackdelar
--------------------------

- Parametrar kan inte länkas till, men ett formulär måste skickas in.
- Kan inte indexeras (relaterat till föregående punkt).
- Säkrare än <a href="/method-get">GET</a> (data skickas på ett dolt sätt, värdena visas inte i historiken)
- POST får inte förväxlas med SSL, POST är inte krypterat.
