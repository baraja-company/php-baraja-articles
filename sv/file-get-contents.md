File_get_contents
=================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	sv: file-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

Funktionen **file_get_contents** används för att läsa en fil och lägga in dess innehåll i en variabel. Den här funktionen liknar funktionen <a href="/include">include</a>, men till skillnad från include kan den hämta fjärrfiler på Internet och överföra deras innehåll via variabler.

Exempel
------

Båda funktionerna kan användas för att läsa in en lokal fil från disken:

```php
$news = file_get_contents('news.html');

echo 'Senaste nyheter:<br>' . $news;
```

Eller från en fjärr-URL:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

När du hämtar en URL kan vilken adress som helst hämtas och dess innehåll kan hämtas som en sträng i en variabel. När det gäller HTML är detta källkoden.

Sidan visas felaktigt
----------------------------

Detta beror på att HTML-koden skickas exakt så som den är placerad i URL:en.

Om sökvägen till bilden till exempel är `<img src="kocka.png">`, så finns den här filen kanske inte på vår server, så vi måste korrigera sökvägen till till exempel: `<img src="https://server.cz/kocka.png">`.
