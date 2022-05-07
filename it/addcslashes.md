Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	it: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Supporto PHP4, PHP5

`addcslashes` - stringa di slash in stile C

Descrizione
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Restituisce una stringa con backslash prima dei caratteri specificati nel parametro charlist.

Parametri
--------------------------

**str** Stringa di testo

**Cartella**

caratteri da rimuovere. Se charlist contiene i caratteri `\n`, `\r`, e altri, sono convertiti in stile C. Altri caratteri ASCI non alfanumerici di lunghezza inferiore a 32 e superiore a 126 cambiano.

Quando definite una sequenza di caratteri in un argomento charlist, assicuratevi di sapere quali caratteri mettete come inizio e fine dell'intervallo.

```php
echo addcslashes('foo[ ]', 'A..z');

// Valori: \f\o \o \o[ \o \o]
// Rimuove tutte le lettere minuscole e maiuscole
```

Valori di ritorno
--------------------------

Restituisce la stringa modificata.

Esempio
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\0..\37!@\177..\377`, rimuove tutti i caratteri con codice ASCII tra 0 e 31.
