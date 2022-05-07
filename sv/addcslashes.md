Addcslashes
===========

> id: '48278937-b1c5-479b-bac2-9b1ec552df4c'
> slug:
> 	cs: addcslashes
> 	sv: addcslashes
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1f53fe14dd5fbf79958c645d15c4d204'

Stöd för PHP4, PHP5

`addcslashes` - snedstreck i C-stil

Beskrivning
--------------------------

```php
string addcslashes (string $str, string $charlist)
```

Återger en sträng med backslashes före de tecken som anges i parametern charlist.

Parametrar
--------------------------

**str** Textsträng

**charlist**

tecken som ska tas bort. Om charlist innehåller tecknen `\n`, `\r` och andra konverteras de till C-stil. Andra icke-alfanumeriska ASCI-tecken med längder mindre än 32 och större än 126 ändras.

När du definierar en sekvens av tecken i ett charlist-argument ska du se till att du vet vilka tecken du anger som början och slutet på intervallet.

```php
echo addcslashes('foo[ ]', 'A..z');

// Värden: \f\o\o\o\o\o[ \]
// Tar bort alla små och stora bokstäver.
```

Returvärden
--------------------------

Återger den ändrade strängen.

Exempel
--------------------------

```php
$escaped = addcslashes($not_escaped, "\0..\37!@\177..\377");
```

charlist `\0..\37!@\177..\377`, tar bort alla tecken med ASCII-kod mellan 0 och 31.
