File_put_contents
=================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	sk: file-put-contents
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

Funkcia **file_put_contents** je vhodná na automatický zápis do súboru. Alternatívne môžete použiť aj <a href="/fopen">fopen()</a>, čo však začiatočníkom neodporúčam.

Vzorka
--------------------------

```php
$file = 'file.txt';
$content = 'Obsah, ktorý sa má uložiť do súboru.';

file_put_contents($file, $content);
```

file_put_contents má 2 parametre:

- `filename` kam zapisovať,
- `Obsah súboru`, ktorý budeme zapisovať.

> Poznámka: `file_put_contents()` prepíše súbor najnovším obsahom.

Pozor na prepísanie
--------------------------

Ak ukladáte prostredníctvom file_put_contents, dajte si pozor na prepísanie údajov. Funkcia odstráni celý aktuálny obsah a nahradí ho novým obsahom. Ak teda chcete len pridať text, môžete ho pridať na začiatok alebo na koniec pomocou vlastného skriptu:

```php
$file = 'file.txt';
$content = 'Nový obsah.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Takže najprv sa otvorí súbor, potom sa zapíše nový obsah a po ňom sa zapíše pôvodný obsah...

Ak chceme pridať starý obsah pred nový, stačí skript mierne upraviť:

```php
$file = 'file.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
