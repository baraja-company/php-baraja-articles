File_get_contents
=================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	da: file-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

Funktionen **file_get_contents** bruges til at læse en fil og sætte dens indhold ind i en variabel. Denne funktion ligner <a href="/include">include</a>-funktionen, men i modsætning til include kan den hente eksterne filer på internettet og overføre deres indhold via variabler.

Eksempel
------

Begge funktioner kan bruges til at indlæse en lokal fil fra disken:

```php
$news = file_get_contents('news.html');

echo 'Seneste nyt:<br>' . $news;
```

Eller fra en ekstern URL:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

Når du henter en URL, kan enhver adresse downloades, og dens indhold kan hentes som en streng i en variabel. I tilfælde af HTML er dette kildekoden.

Siden er gengivet forkert
----------------------------

Det skyldes, at HTML-koden overføres nøjagtigt, som den er placeret i URL'en.

Hvis stien til billedet f.eks. er `<img src="kocka.png">`, findes denne fil muligvis ikke i vores server, så vi skal rette stien til f.eks. `<img src="https://server.cz/kocka.png">`.
