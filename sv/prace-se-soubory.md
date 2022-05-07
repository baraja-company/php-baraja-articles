Arbeta med filer
================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	sv: arbeta-med-filer
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

Det finns ett antal funktioner i PHP för att arbeta med filer.

- <a href="/fopen">fopen()</a>, lågnivååtkomst till filer på disk.
- <a href="/file-get-contents">file_get_contents()</a>, hämtar innehållet i en fil eller URL.
- <a href="/file-put-contents">file_put_contents()</a>, sparar en sträng till en lokal fil.

Diskfunktioner
--------------

- `unlink($path)`, raderar fil
- `copy($from, $to)`, kopierar en fil från en plats till en annan (kan vara från en URL till den lokala disken)
- `rename()`, byta namn på eller flytta en fil på disken
- `chmod()`, ändrar behörigheter för att läsa, skriva eller köra en fil.
