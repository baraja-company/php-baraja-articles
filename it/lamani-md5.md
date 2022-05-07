Come rompere la funzione md5
============================

> id: '9e678fcc-3d5e-46a3-9c3f-c1eb3d1ad367'
> slug:
> 	cs: lamani-md5
> 	it: come-rompere-la-funzione-md5
> 
> publicationDate: '2019-08-23 15:33:10'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '4e176a09e43dbf12e131103be6a9cf4f'

MD5 è una funzione molto usata per calcolare gli hash.

I principianti spesso lo usano per <a href="/hashovani">password hashing</a>, che non è una buona idea perché ci sono molti modi per recuperare la password originale.

Questo articolo descrive metodi specifici per farlo.

Complessità temporale
----------------

Tutta la sicurezza è costruita sul fatto che ci vuole un tempo sproporzionatamente lungo per provare tutte le password. Beh, dovrebbe. Il problema con l'algoritmo `md5()` in particolare è che è una funzione molto veloce. Su un computer normale, non è un problema calcolare più di un milione di hash al secondo.

Se rompiamo la password provando le combinazioni una per una, si tratta di un **attacco di forza bruta**.

Metodi di frattura
----------------

Ci sono diverse strategie:

- Test successivi per tentativi ed errori (attacco a forza bruta)
- Testare le password del dizionario
- Tabelle arcobaleno (database di hash precompilati)
- Ricerche su Google
- Colpire le collisioni nell'algoritmo

Ci sono molti altri metodi, questo articolo descrive solo i più comuni.

Strategie di rottura della forza bruta
-----------------------------

Tutte le combinazioni di lettere, numeri e altri caratteri vengono provate una alla volta.

I tentativi generati sono sottoposti a un hash uno per uno e confrontati con l'hash originale.

Così, per esempio:

```php
aaaaaaabaaacaaadaaaeaaaf...
```

Il problema con questo attacco è nell'algoritmo `md5()` stesso, se dovessimo provare solo le lettere minuscole dell'alfabeto inglese e i numeri, ci vorrebbero al massimo decine di minuti per provare tutte le combinazioni su un computer comunemente disponibile.

È quindi importante scegliere password lunghe, preferibilmente casuali e con caratteri speciali.

Strategia di attacco del dizionario
----------------------------

La gente di solito sceglie password deboli che esistono nel dizionario.

Se approfittiamo di questo fatto, possiamo scartare rapidamente varianti improbabili come `6w1SCq5cs` e indovinare invece le parole esistenti.

Inoltre, sappiamo da precedenti fughe di password di grandi aziende che gli utenti scelgono una lettera maiuscola all'inizio della password e un numero alla fine. Vediamo - la tua password ha anche questo? :)

Tabelle arcobaleno - database precompilato
--------------------------------------

Poiché una password corrisponde sempre allo stesso hash, si può facilmente ricalcolare un enorme database in cui le password saranno cercate per prime.

Infatti, la ricerca è sempre ordini di grandezza più veloce della ricerca di hash più e più volte.

Inoltre, per fughe di dati più grandi, le password possono essere hashate in parallelo in questo modo e, per esempio, il 10% di tutte le password degli utenti può essere recuperato rapidamente.

Un buon database di password è per esempio <a href="https://crackstation.net/">Crack Station</a>.

Ricerca Google
-------------------

Molte password semplici sono conosciute direttamente da Google perché indicizza le pagine che contengono hash.

Uso sempre Google come prima opzione. :)

Trovare le collisioni nell'algoritmo
--------------------------

Il <a href="https://cs.wikipedia.org/wiki/Dirichlet%C5%AFv_princip">principio di Dirichlet</a> descrive che se abbiamo un insieme di hash che sono sempre lunghi 32 caratteri, allora ci sono almeno 2 password diverse di 33 caratteri (una più lunga) che generano lo stesso hash.

In pratica, non ha senso cercare le collisioni, ma a volte l'autore stesso dell'applicazione facilita l'indovinello ricalcolando le collisioni.

Per esempio:

```php
$password = 'password';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hash tramite md5()
```

In questo caso, ha senso indovinare la collisione piuttosto che l'hash originale.

Salute all'imbastitura!
