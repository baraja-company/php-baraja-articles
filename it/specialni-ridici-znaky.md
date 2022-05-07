Caratteri di controllo speciali in PHP
======================================

> id: '888e6ee4-2ff3-4f0d-8f5a-4741e62e11d0'
> slug:
> 	cs: specialni-ridici-znaky
> 	it: caratteri-di-controllo-speciali-in-php
> 
> publicationDate: '2021-11-24 14:05:00'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '2711968227a253040a0a6ed1464aa447'

Le stringhe PHP possono contenere caratteri di controllo speciali che hanno significati diversi in un contesto particolare e non si comportano necessariamente come caratteri regolari.

Molti di questi vi saranno già intuitivamente familiari. Alcuni sono riservati per usi speciali e altri sono riservati ai caratteri della tastiera, per esempio.

Scrivere caratteri speciali
-----------------------

I caratteri speciali sono scritti tra doppi apici.

Quindi è molto semplice:

```php
$message = "Salve.";
```

Il codice precedente contiene un'interruzione di riga tra `Hello` e `world`.

Tabella dei caratteri speciali
-------------------------

Se la stringa è racchiusa tra doppi apici ("), PHP interpreterà le seguenti sequenze di escape come caratteri speciali:

| Sequenza, significato.
|----------|--------|
| `\n` | linefeed (`LF` o `0x0A (10)` in ASCII) |
| Ritorno a capo (`CR` o `0x0D (13)` in ASCII) |
| ``t` | tab orizzontale (`HT` o `0x09 (9)` in ASCII) |
| ``v` | tabulazione verticale (`VT` o `0x0B (11)` in ASCII) |
| escape (`ESC` o `0x1B (27)` in ASCII) |
| `\f`` `imboccatura di forma (`FF` o `0x0C (12)` in ASCII) |
| Il backslash è un'altra cosa.
| Il segno del dollaro.
| Doppie virgolette
| `[0-7]{1,3}` | La sequenza di caratteri corrispondente a un'espressione regolare è un carattere in notazione ottale che trabocca silenziosamente in un byte. (per esempio `"\400" === "\000"`) |
| La sequenza di caratteri corrispondente a un'espressione regolare è un carattere in notazione esadecimale.
| La sequenza di caratteri che corrisponde all'espressione regolare è un punto di codice Unicode, che verrà emesso nella stringa come rappresentazione UTF-8 di quel punto di codice.

Come per le stringhe tra apici singoli, un backslash verrà emesso quando si esegue l'escape di qualsiasi altro carattere.

Quando si delimitano le stringhe con le virgolette, si tenga presente che le variabili contenute saranno espanse (i valori delle variabili saranno scritti direttamente nella stringa). Questo comportamento può essere estremamente pericoloso.
