File_put_contents
=================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	sv: file-put-contents
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

Funktionen **file_put_contents** är lämplig för automatisk skrivning till en fil. Alternativt kan du också använda <a href="/fopen">fopen()</a>, vilket jag inte rekommenderar för nybörjare.

Exempel
--------------------------

```php
$file = 'file.txt';
$content = 'Innehåll som ska sparas i en fil.';

file_put_contents($file, $content);
```

file_put_contents har 2 parametrar:

- `filnamn` där du ska skriva,
- Innehållet i filen som vi ska skriva.

> Notera: `file_put_contents()` skriver över filen med det senaste innehållet.

Se upp för överskrivning
--------------------------

Om du sparar via file_put_contents ska du se upp för att inte skriva över data. Funktionen raderar allt nuvarande innehåll och ersätter det med det nya innehållet. Om du bara vill lägga till texten kan du antingen lägga till den i början eller i slutet med hjälp av ditt eget skript:

```php
$file = 'file.txt';
$content = 'Nytt innehåll.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Först öppnas filen, sedan skrivs det nya innehållet och därefter skrivs det ursprungliga innehållet...

Om vi vill lägga till det gamla innehållet före det nya behöver vi bara ändra skriptet något:

```php
$file = 'file.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
