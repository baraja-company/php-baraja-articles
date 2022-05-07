Práca so súbormi
================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	sk: praca-so-subormi
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

V jazyku PHP existuje množstvo funkcií na prácu so súbormi.

- <a href="/fopen">fopen()</a>, nízkoúrovňový prístup k súborom na disku
- <a href="/file-get-contents">file_get_contents()</a>, načítanie obsahu súboru alebo adresy URL
- <a href="/file-put-contents">file_put_contents()</a>, uloženie reťazca do lokálneho súboru.

Funkcie disku
--------------

- `unlink($path)`, odstrániť súbor
- `copy($from, $to)`, skopíruje súbor z jedného miesta na druhé (môže byť z URL na lokálny disk)
- `rename()`, premenovanie alebo presunutie súboru na disku
- `chmod()`, zmena oprávnení na čítanie, zápis alebo spustenie súboru
