Apostrofi e virgolette
======================

> id: '526ad995-3412-446e-bb56-9627dff8e29e'
> slug:
> 	cs: apostrofy-a-uvozovky
> 	it: apostrofi-e-virgolette
> 
> perex:
> 	- Použití uvozovek a apostrofů pro ohraničení řetězců v PHP. Escapování řetězců a řešení kontextů.
> 	- Usare le virgolette e gli apostrofi per delimitare le stringhe in PHP. Escaping delle stringhe e risoluzione del contesto.
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c5b0bf8b74238134be5348f886591e2a

Si possono usare sia le virgolette che gli apostrofi per delimitare le stringhe. Personalmente, preferisco solo **apostrofi** a meno che non si tratti di un carattere speciale di interruzione di riga o di tabulazione.

Ci sono diverse ragioni per questo, esaminiamole in modo costruttivo.

> La ragione principale per non usare le virgolette è la sicurezza e la gestione inappropriata dei tipi di dati.

Usare i tag HTML all'interno di una stringa
--------------------------------

Se abbiamo bisogno di restituire il codice HTML in una stringa, e avvolgiamo la stringa tra virgolette, dobbiamo fare l'escaping in modo abbastanza imbarazzante:

```php
return "<a href=\"{$link}\">{$text}</a>";
```

O fare la stessa cosa in un modo più leggibile, mantenendo le virgolette senza escape:

```php
return '<a href="' . $link . '">' . $text . '</a>';
```

Maggiore distanza visiva della stringa dalla variabile
---------------------------------------------

Non c'è niente di peggio che appiccicare variabili all'interno di una stringa direttamente l'una sull'altra, dove non si può dire visivamente cosa sia una variabile e cosa una stringa:

```php
$url = "{$baseImageUrl}/{$dirName}/{$basename}.{$ext}";
```

Questa notazione non ha alcun effetto negativo sulla funzionalità, solo le singole variabili e i caratteri fissi della stringa sono impilati troppo vicini, il che degrada la leggibilità.

Proviamo di nuovo e rendiamolo più leggibile:

```php
$url = $baseImageUrl . '/' . $dirName . '/' . $basename . '.' . $ext;
```

Il vantaggio principale che vedo è la pulizia intorno alle variabili, dove non ci sono caratteri inutili nel nome.

Inoltre, sarà più facile da avvolgere e non ci saranno caratteri di avvolgimento nella stringa ;)

```php
$url = $baseImageUrl
       . '/' . $dirName
       . '/' . $basename . '.' . $ext;
```

Non è possibile chiamare una funzione tra virgolette
---------------------------------------

Ma una variabile può, e questo è il motivo per cui "qualcosa" viene aggiunto all'esterno della stringa (specialmente le funzioni), ma le variabili sono scritte all'interno. E a volte il programmatore aggiunge anche la variabile dopo la stringa. In breve, confusione su confusione.

Perché non possiamo fare le cose in modo uniforme?

Mappatura inappropriata di un altro tipo di dati in una stringa
---------------------------------------------------

Considerate la seguente chiamata di funzione:

```php
echo getFullName("{$user->name}");

function getFullName(string $name): string
{
	// ... implementazione ...
}
```

È possibile inserire variabili tra virgolette, il che farà sì che vengano riscritte come una stringa. Tuttavia, se la variabile è nella stringa stessa ed è di un tipo di dati diverso da stringa, le informazioni sul tipo di dati originale possono essere corrotte. Per esempio, se il costrutto `$user->name` restituisse `false` o `null`, non saremmo in grado di dire che si tratta di un errore.

Un'applicazione dovrebbe sempre riconoscere una condizione di errore e fallire correttamente, piuttosto che ignorarla. Una corretta segnalazione degli errori porta a un migliore debugging in futuro.

Occasionalmente, si possono incontrare degli override, specialmente perché una funzione richiede un particolare tipo di dati:

```php
trim("{$returnText}");
```

In questo caso, sono più incline a scriverlo:

```php
trim ((string) $returnText);
```

che non è così "magico" ed è ovvio anche per gli utenti meno esperti cosa è successo alla variabile.

Eliminare il valore `null` dal database
----------------------------------

Supponiamo di recuperare il nome di un hotel da un database:

```php
return "{$row->hotel->name}";
```

Ma cosa succede se il nome non esiste ed è `null`? Usando le virgolette, abbiamo creato un errore potenzialmente difficile da rilevare, perché sarà riscritto in una stringa vuota. Tuttavia, vorremmo restituire un vero `null`. Fa differenza se il record non esiste, o esiste ma è vuoto.

Quindi il modo corretto di farlo sarebbe quello di non specificare nulla:

```php
return $row->hotel->name;
```

La funzione richiede la restituzione di un tipo di dati specifico ed è rigorosa su questo? Se non possiamo soddisfare la specifica, dovremmo lanciare un'eccezione.

```php
function getHotelName(int $hotelId): string
{
   // implementazione

   if ($row->hotel->name === null) {
      throw new \Exception('Il nome dell'hotel non esiste.');
   }

   return $row->hotel->name;
}
```

Incoerenze nella codifica dei caratteri e dei tipi di dati
--------------------------------------------

Non mi capita spesso di incontrare una costruzione del genere:

```php
echo "{$returnCode}" . self::CRLF;
```

Che è meglio scritto come:

```php
echo $returnCode . "\n";
```

L'uso delle virgolette intorno alla variabile non è necessario in questo caso e rende solo più difficile la lettura perché sono caratteri extra non necessari. Allo stesso tempo, il line wrapping del tipo `CRLF` non è moderno ed è meglio usare solo `\n`, che è nativo della codifica `UTF-8`.

Correttamente, potremmo ancora convalidare il tipo di dati per il codice. Per esempio, che sia un numero ed eventualmente restituire una sostituzione. Questo può essere fatto con l'operatore <a href="/ternary-operator">ternary operator</a>:

```php
echo (is_int($returnCode) ? $returnCode : '?') . "\n";
```

> Nota: l'uso della funzione `is_int()` indica una cattiva progettazione dell'applicazione, perché non dovrebbe mai accadere di non conoscere il tipo di dati di una variabile.

Avvolgere la stringa di output in apostrofi
---------------------------------------

È spesso necessario racchiudere la stringa in uscita da una variabile tra virgolette o apostrofi nel testo dell'eccezione. Tuttavia, questo crea inutilmente un "payoff" per entrambi i tipi di delimitatori di stringa, e se la stringa inizia e finisce con lo stesso carattere, non è possibile determinare rapidamente e senza ambiguità dove la stringa è iniziata e finita nel caso di stringhe multiple se un apostrofo appare all'interno:

```php
throw new \Exception(__METHOD__ . ": Unsupported data type '{$fileType}'");
```

Personalmente lo risolvo racchiudendolo in un tipo di carattere diverso (le parentesi quadre funzionano bene per me), in modo che l'inizio e la fine della stringa possano essere visti elegantemente:

```php
throw new \Exception(__METHOD__ . ': Tipo di dati non supportato [' . $fileType . ']');
```

Oppure:

```php
throw new \Exception("Status '{$status}' is invalid");
```

Può essere scambiato con:

```php
throw new \Exception('Stato [' . $status . '] non è valido');
```

Parsing per linee
--------------------

Le virgolette sono utili, per esempio, per il parsing da una nuova linea:

```php
foreach(explode("\n", $text) as $line) {
	// implementazione
}
```

Sono stati creati appositamente per questo scopo.

Tuttavia, se hai bisogno di elaborare un documento più complesso (come il codice sorgente di un linguaggio di programmazione o una pagina HTML), ti consiglio di usare **Tokenizer** insieme a <a href="/regex">espressioni regolari</a>.

Non combinare due metodi per racchiudere
-----------------------------------

Se per qualche ragione hai intenzione di usare comunque le virgolette, almeno mantienile coerenti per tutto il tempo.

Questo è un caso dissuasivo:

```php
throw new \Exception("L'immagine sull'URL non esiste. ResponseSize:" . strlen($result) . ')');
```

Dove l'inizio della stringa è delimitato da virgolette e la fine da apostrofi.

Questo difetto si verifica soprattutto quando si usano strumenti di formattazione automatica (come **Code Sniffer**) che cercano di unificare le convenzioni, ma non sempre ci riescono.
