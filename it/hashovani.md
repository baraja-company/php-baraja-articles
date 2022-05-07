Hashing di stringhe e password
==============================

> id: '7978bee8-62cc-4770-b15b-a8d08d1dcf34'
> slug:
> 	cs: hashovani
> 	it: hashing-di-stringhe-e-password
> 
> perex:
> 	- 'Hash není šifra! Metody hashování dat a hesel. MD5, SHA1, Bcrypt. Ověření hesla.'
> 	- 'L''hash non è un cifrario! Metodi di hashing di dati e password. MD5, SHA1, Bcrypt. Verifica della password.'
> 
> publicationDate: '2019-09-11 10:13:30'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '5d1e289fd93e18ad73eb23ee1bbba8ee'

Il processo di hashing (al contrario della crittografia) produce un output dall'input da cui la stringa originale non può più essere derivata.

È quindi adatto a proteggere stringhe sensibili, password e checksum.

Un'altra bella caratteristica delle funzioni di hashing è che generano sempre output della stessa lunghezza, e un piccolo cambiamento nell'input cambia sempre completamente l'intero output.

Funzioni di hashing
----------------

Ci sono molte funzioni hash in PHP, le più importanti sono:

- **Bcrypt: password_hash()** - L'hashing della password più sicuro, computazionalmente lento, usa il sale interno e l'hashing iterativo.
- **md5()** - Funzione molto veloce adatta all'hashing dei file. L'output è sempre di 32 caratteri.
- **sha1()** - Funzione hash veloce per l'hash dei file, usata internamente da Git per l'hash dei commit. L'output è sempre di 40 caratteri.

Hashing
-----------

```php
$password = 'secret-password';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Attenzione:** Né `md5()` né `sha1()` sono adatti per l'hashing delle password, perché è computazionalmente facile scoprire la password originale, o almeno precompilare le password. È molto meglio usare `bcrypt`, che è stato creato per l'hashing delle password.
>
> Il sito <a href="https://www.md5cracker.com/">md5cracker.com</a> ha un database di checksum (hash), prova a cercare l'hash: `79c2b46ce2594ecbcb5b73e928345492`, come puoi vedere, quindi il puro `md5()` non è così sicuro per parole e password comuni.

L'unica soluzione corretta: `Bcrypt + sale`.
--------------------------------------

Nel discorso <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Come non incasinarsi nel piano di destinazione</a>, David Grudl ha affrontato i modi per hashare e memorizzare correttamente le password.

L'unica soluzione corretta è: `Bcrypt + sale`.

In particolare:

```php
$password = 'hash';

// Genera un hash sicuro
echo password_hash($password, PASSWORD_BCRYPT);

// In alternativa con una complessità maggiore (l'impostazione predefinita è 10):
echo password_hash($password, PASSWORD_BCRYPT, ['costo' => 12]);
```

Il vantaggio di Bcryp è principalmente nella sua velocità e nella salatura automatica.

Il fatto che ci voglia **lungo** per generare, diciamo 100 ms, rende molto costoso per un attaccante testare molte password.

Inoltre, l'hash in uscita è automaticamente trattato con **sale casuale**, il che significa che quando la stessa password è sottoposta ad hash ripetutamente, l'uscita è sempre un hash diverso. Pertanto, un attaccante non sarà in grado di utilizzare una tabella hash precompilata.

Pertanto, non saremo in grado di verificare la correttezza della password tramite hashing ripetuto, ma avremo bisogno di chiamare una funzione specializzata:

```php
if (password_verify($password, $hash)) {
    // La password è corretta
} else {
    // La password non è corretta
}
```

Saldatura della password
------------

Per rendere più difficile il cracking dell'hash, è una buona idea inserire qualche stringa aggiuntiva nell'input originale. Idealmente uno a caso. Questo processo è chiamato **salatura della password**.

La sicurezza si basa sull'idea che un attaccante non sarà in grado di utilizzare una tabella precompilata di password e hash, perché non conoscerà il sale e dovrà craccare le password individualmente.

Per esempio:

```php
$password = 'passaporto_segreto';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // stampa la password originale
echo $hash;     // stampa l'hash della password incluso il sale
```

Funzioni hash composte
------------------------

Potreste pensare che sarebbe una buona idea eseguire la funzione di hash ripetutamente, aumentando così la complessità del cracking, poiché la password originale dovrà essere hashata ripetutamente.

Per esempio:

```php
$password = 'password';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hash tramite md5()
```

Paradossalmente, la difficoltà di sfondare si riduce o rimane quasi la stessa.

La ragione è che la funzione `md5()` è estremamente veloce e può calcolare oltre un milione di hash al secondo su un computer normale, quindi provare le password una per una non rallenta molto.

La seconda ragione è più una teoria, cioè la possibilità di incorrere in una cosiddetta collisione. Se facciamo l'hash di una password ripetutamente, col tempo può succedere che colpiamo un hash che l'attaccante già conosce, e questo gli permetterà di fare l'hash della password usando il database.

Pertanto, è meglio utilizzare una funzione di hashing lento e sicuro ed eseguire l'hashing solo una volta, pur trattando l'output finale con salting.
