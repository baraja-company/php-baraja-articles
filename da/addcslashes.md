Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	da: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Understøtter PHP4, PHP5

`addcslashes` - Skråstreg i C-stil

Beskrivelse
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Returnerer en streng med skråstreger før de tegn, der er angivet i parameteren charlist.

Parametre
--------------------------

**str** Tekststreng

**charlist**

tegn, der skal fjernes. Hvis charlist indeholder tegnene `\n`, `\r` og andre, konverteres de til C-stil. Andre ikke-alfanumeriske ASCI-tegn med en længde på mindre end 32 og mere end 126 ændres.

Når du definerer en sekvens af tegn i et charlist-argument, skal du sikre dig, at du ved, hvilke tegn du sætter som begyndelsen og slutningen af området.

```php
echo addcslashes('foo[ ]', 'A..z');

// Værdier: \f\o\o\o\o\o[ \]
// Fjerner alle små og store bogstaver
```

Returneringsværdier
--------------------------

Returnerer den ændrede streng.

Eksempel
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\\0..\37!@\177..\377`, fjerner alle tegn med ASCII-kode mellem 0 og 31.
