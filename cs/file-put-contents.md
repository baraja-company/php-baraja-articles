File_put_contents
=================

> id: "7ec781c6-e3db-484d-8a49-0b93ab8252b1"
> slug:
> 	cs: file-put-contents
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f

Funkce **file_put_contents** je vhodná pro automatický zápis do souboru. Alternativně lze použít taky <a href="/fopen">fopen()</a>, kterou začátečníkům nedoporučuji.

Ukázka
--------------------------

```php
$file = 'soubor.txt';
$content = 'Obsah k uložení do souboru.';

file_put_contents($file, $content);
```

file_put_contents má 2 parametry:

- `název souboru` kam budeme zapisovat,
- `obsah souboru`, který zapíšeme.

> Pozor: `file_put_contents()` soubor přepíše nejnovějším obsahem.

Pozor na přepsání
--------------------------

Pokud budete přes file_put_contents ukládat, dejte si pozor na přepsání dat. Funkce totiž celý aktuální obsah smaže a nahradím obsahem novým. Pokud tedy chcete text pouze připsat, můžete buď na začátek nebo na konec pomocí vlastního scriptu:

```php
$file = 'soubor.txt';
$content = 'Nový obsah.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Nejprve se tedy soubor otevře, pak se připíše nový obsah, a za něj se připíše původní obsah...

Pokud chceme přidat starý obsah před nový, stačí script lehce upravit:

```php
$file = 'soubor.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
