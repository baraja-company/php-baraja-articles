File_put_contents
=================

> id: '7ec781c6-e3db-484d-8a49-0b93ab8252b1'
> slug:
> 	cs: file-put-contents
> 	it: file-put-contents
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: '78211a3696ebba56d559045f8a0ab9e4'

La funzione **file_put_contents** è adatta per la scrittura automatica su un file. In alternativa, potete anche usare <a href="/fopen">fopen()</a>, che non consiglio ai principianti.

Campione
--------------------------

```php
$file = 'file.txt';
$content = 'Contenuto da salvare in un file.';

file_put_contents($file, $content);
```

file_put_contents ha 2 parametri:

- `filename` dove scrivere,
- Il `contenuto del file` che scriveremo.

> Nota: `file_put_contents()` sovrascrive il file con gli ultimi contenuti.

Attenzione alla sovrascrittura
--------------------------

Se salvate tramite file_put_contents, fate attenzione a sovrascrivere i dati. La funzione cancellerà tutto il contenuto corrente e lo sostituirà con il nuovo contenuto. Quindi, se vuoi solo aggiungere il testo, puoi aggiungerlo all'inizio o alla fine usando il tuo script:

```php
$file = 'file.txt';
$content = 'Nuovo contenuto.';

$oldContent = file_get_contents($file);
file_put_contents($file, $content . $oldContent);
```

Quindi prima si apre il file, poi si scrive il nuovo contenuto, e dopo si scrive il contenuto originale...

Se vogliamo aggiungere il vecchio contenuto prima del nuovo, dobbiamo solo modificare leggermente lo script:

```php
$file = 'file.txt';
$content = Nový obsah.';

$oldContent = file_get_contents($soubor);
file_put_contents($file, $oldContent . $content);
```
