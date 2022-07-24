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
> sourceContentHash: f6ea0b06d6ace3c41684a49938f7ce8e

Il processo di hashing (a differenza della crittografia) produce un output dall'input dal quale non è più possibile ricavare la stringa originale.

È quindi adatto per proteggere stringhe sensibili, password e checksum.

Un'altra bella caratteristica delle funzioni di hashing è che generano sempre output della stessa lunghezza e una piccola modifica dell'input cambia sempre completamente l'intero output.

Funzioni di hashing
----------------

Esistono molte funzioni hash in PHP, le più importanti sono:

- **Bcrypt: password_hash()** - L'hashing più sicuro per le password, computazionalmente lento, utilizza un sale interno ed esegue l'hashing in modo iterativo.
- **md5()** - Funzione molto veloce adatta all'hashing dei file. L'output è sempre di 32 caratteri.
- **sha1()** - Funzione hash veloce per l'hashing dei file, usata internamente da Git per l'hashing dei commit. L'output è sempre di 40 caratteri.

Hashing
-----------

```php
$password = 'password segreta';

echo password_hash($password); // Bcrypt
echo md5($password);
echo sha1($password);
```

> **Attenzione:** Né `md5()` né `sha1()` sono adatti per l'hashing delle password, perché è computazionalmente facile scoprire la password originale, o almeno precompilare le password. È molto meglio usare `bcrypt`, che è stato sviluppato per l'hashing delle password.
>
> Il sito web <a href="https://www.md5cracker.com/">md5cracker.com</a> contiene un database di checksum (hash), provate a cercare hash: `79c2b46ce2594ecbcb5b73e928345492`, come potete vedere, quindi il puro `md5()` non è così sicuro per parole e password comuni.

L'unica soluzione corretta: `Bcrypt + salt`.
--------------------------------------

Nell'intervento <a href="https://www.youtube.com/watch?v=F58_A5TM-Sc">Come non sbagliare nel piano di destinazione</a>, David Grudl ha affrontato i modi per eseguire correttamente l'hash e la memorizzazione delle password.

L'unica soluzione corretta è: `Bcrypt + salt`.

In particolare:

```php
$password = 'hash';

// Genera un hash sicuro
echo password_hash($password, PASSWORD_BCRYPT);

// In alternativa, con una complessità maggiore (l'impostazione predefinita è 10):
echo password_hash($password, PASSWORD_BCRYPT, ['costo' => 12]);
```

Il vantaggio di Bcryp risiede principalmente nella sua velocità e nella salatura automatica.

Il fatto che impieghi **lungo** per essere generato, ad esempio 100 ms, rende molto costoso per un attaccante testare molte password.

Inoltre, l'hash in uscita viene trattato automaticamente con **sale casuale**, il che significa che quando la stessa password viene sottoposta a hash ripetutamente, l'output è sempre un hash diverso. Pertanto, un aggressore non sarà in grado di utilizzare una tabella hash precompilata.

Pertanto, non sarà possibile verificare la correttezza della password mediante hashing ripetuto, ma sarà necessario chiamare una funzione specializzata:

```php
if (password_verify($password, $hash)) {
    // La password è corretta
} else {
    // La password non è corretta
}
```

Salatura delle password
------------

Per rendere più difficile il cracking dell'hash, è una buona idea inserire una stringa aggiuntiva nell'input originale. Idealmente uno a caso. Questo processo si chiama **salatura delle password**.

La sicurezza si basa sull'idea che un attaccante non sarà in grado di utilizzare una tabella precompilata di password e hash, perché non conoscerà il sale e dovrà decifrare le password singolarmente.

Ad esempio:

```php
$password = 'passaporto_segreto';
$salt = 'fghjgtzjhg';

$hash = md5($password . $salt);

echo $password; // stampa la password originale
echo $hash;     // stampa l'hash della password, compreso il sale
```

Funzioni hash composte
------------------------

Si potrebbe pensare che sarebbe una buona idea eseguire la funzione di hash ripetutamente, aumentando così la complessità del cracking, poiché la password originale dovrà essere sottoposta a hash ripetutamente.

Ad esempio:

```php
$password = 'password';

for ($i = 0; $i <= 1000; $i++) {
    $password = md5($password);
}

echo $password; // 1000x hash tramite md5()
```

Paradossalmente, la difficoltà di sfondamento si riduce o rimane pressoché invariata.

Il motivo è che la funzione `md5()` è estremamente veloce e può calcolare oltre un milione di hash al secondo su un computer normale, quindi provare le password una per una non rallenta molto.

Il secondo motivo è più che altro teorico, ovvero la possibilità di incappare in una cosiddetta collisione. Se si esegue l'hash di una password ripetutamente, col tempo può accadere che si arrivi a un hash che l'aggressore conosce già e questo gli consentirà di eseguire l'hash della password utilizzando il database.

Pertanto, è meglio utilizzare una funzione di hashing lenta e sicura ed eseguire l'hashing solo una volta, trattando comunque l'output finale con il salting.

Confronto estremamente sicuro di due hash/strings
---------------------------------------------------

Sapevate che l'operatore === non è la scelta più sicura per il confronto degli hash nella verifica delle password?

Quando si confrontano le stringhe, le due stringhe vengono attraversate carattere per carattere finché non si raggiunge la fine (successo, sono uguali) o non si nota una differenza (le stringhe sono diverse).

E questo può essere sfruttato in un attacco. Se si misura il tempo in modo sufficientemente preciso, si può stimare quanti caratteri devono ancora essere aggiunti per ottenere una corrispondenza esatta e raggiungere la fine, oppure si può stimare la distanza percorsa dalle stringhe quando si confrontano le stringhe.

La soluzione consiste nell'utilizzare la funzione hash_equals() ovunque vengano confrontate le stringhe, e sarebbe importante se un utente malintenzionato potesse scoprire la posizione in cui il confronto è fallito.

E come fa la funzione a farlo? Si assicura che il confronto di due stringhe qualsiasi richieda sempre la stessa quantità di tempo, in modo che non si possa capire, misurando il tempo, dove si è verificata la differenza. Trovo che alcuni tipi di attacchi siano davvero molto improbabili e difficili da attuare. Questa è una di quelle.
