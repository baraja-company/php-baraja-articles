Arbejde med filer
=================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	da: arbejde-med-filer
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

Der findes en række funktioner i PHP til at arbejde med filer.

- <a href="/fopen">fopen()</a>, adgang på lavt niveau til filer på disken
- <a href="/file-get-contents">file_get_contents()</a>, henter indholdet af en fil eller URL
- <a href="/file-put-contents">file_put_contents()</a>, der gemmer en streng til en lokal fil.

Diskfunktioner
--------------

- `unlink($path)`, sletter fil
- `copy($from, $to)`, kopierer en fil fra et sted til et andet (kan være fra en URL til den lokale disk)
- `rename()`, omdøber eller flytter en fil på disken
- `chmod()`, ændrer tilladelser til at læse, skrive eller udføre en fil
