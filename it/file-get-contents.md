File_get_contents
=================

> id: '6c8889f1-95e7-4540-9c3a-0225c6383954'
> slug:
> 	cs: file-get-contents
> 	it: file-get-contents
> 
> publicationDate: '2019-09-11 10:18:03'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: f8b180a5f7208dab0d37761a5acdc9e3

La funzione **file_get_contents** è usata per leggere un file e mettere il suo contenuto in una variabile. Questa funzione è simile alla funzione <a href="/include">include</a>, ma a differenza di include, può recuperare file remoti su Internet e trasferirne il contenuto tramite variabili.

Campione
------

Entrambe le funzioni possono essere usate per caricare un file locale dal disco:

```php
$news = file_get_contents('notizie.html');

echo 'Ultime notizie:<br>' . $news;
```

O da un URL remoto:

```php
$page = file_get_contents('https://www.google.com');

echo $page;
```

Quando si recupera un URL, qualsiasi indirizzo può essere scaricato e il suo contenuto può essere recuperato come una stringa in una variabile. Nel caso dell'HTML, questo è il codice sorgente.

La pagina è resa in modo errato
----------------------------

Questo perché il codice HTML viene passato esattamente come viene messo nell'URL.

Se il percorso dell'immagine è per esempio `<img src="kocka.png">`, allora questo file potrebbe non esistere nel contesto del nostro server, quindi dobbiamo correggere il percorso per esempio: `<img src="https://server.cz/kocka.png">`.
