Lavorare con i file
===================

> id: '12330cfb-5729-419a-b2e4-cb3d1db998cc'
> slug:
> 	cs: prace-se-soubory
> 	it: lavorare-con-i-file
> 
> publicationDate: '2020-02-16 16:30:05'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '0f5b9e84a974285dcb63baafc912be5f'

Ci sono diverse funzioni in PHP per lavorare con i file.

- <a href="/fopen">fopen()</a>, accesso di basso livello ai file su disco
- <a href="/file-get-contents">file_get_contents()</a>, recupera il contenuto di un file o di un URL
- <a href="/file-put-contents">file_put_contents()</a>, salvando una stringa in un file locale.

Funzioni del disco
--------------

- `unlink($path)`, cancella il file
- `copy($from, $to)`, copia un file da una posizione ad un'altra (può essere da un URL al disco locale)
- `rename()`, rinominare o spostare un file su disco
- `chmod()`, cambia i permessi di lettura, scrittura o esecuzione di un file
