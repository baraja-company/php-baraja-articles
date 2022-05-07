PHP 8 è uscito - panoramica completa
====================================

> id: '8b6ce751-195f-41d2-82c6-1af4be3e86b5'
> slug:
> 	cs: php-8-kompletni-prehled-novinek
> 	it: php-8-e-uscito---panoramica-completa
> 
> publicationDate: '2020-11-26 11:53:54'
> mainCategoryId: '17545205-215b-4962-b910-0d67ad1e933a'
> sourceContentHash: f657145ac1d2a1109fcc2a1ff3b6e6cf

Oggi, 26 novembre 2020, la nuova versione principale di PHP 8 è stata rilasciata dopo diversi anni, e include un audace set di nuove funzionalità. Questo è uno dei più grandi aggiornamenti da molto tempo e merita un articolo speciale.

In questo articolo, riassumeremo tutte le principali novità e le differenze nella sintassi e nelle opzioni rispetto alla vecchia versione. La maggior parte delle nuove caratteristiche sono retrocompatibili e portano miglioramenti comportamentali che vi piaceranno.

> **Informazione importante:** PHP 8 è ora in una fase di `feature freeze`, il che significa che non si possono più aggiungere nuovi comportamenti e solo i bug vengono corretti. Così potete contare sulla compatibilità e fare il debug completo delle vostre applicazioni.

Tipi di unione
----------

PHP in generale si è spostato negli ultimi anni da un linguaggio puramente dinamico dove qualsiasi variabile potrebbe contenere qualsiasi cosa a una forma rigorosa dove sappiamo in anticipo quale tipo di dati sarà in quale variabile, parametro, argomento o proprietà. L'uso di [data-types](/datove-types) è ancora facoltativo, ma raccomando l'uso della digitazione forte e la uso io stesso in tutti i progetti.


I tipi unionali esprimono un insieme di tipi multipli, accettando qualsiasi argomento o proprietà in essi.

Per esempio:

```php
function validatePsc(string|int $psc): bool
{
	// implementazione
}
```

La funzione `validatePsc()` nella variabile `$psc`` accetta i tipi di dati `string` (stringa) e `int` (intero).

Nella versione precedente di PHP 7.4, questa notazione non era possibile ed era tipicamente aggirata da [comment](/document-comment):

```php
/**
 * @param string|int $psc
 */
function validatePsc($psc): bool
{
	// implementazione
}
```

Tuttavia, questo commento di annotazione è ignorato da PHP (è un commento, dopo tutto) e abbiamo dovuto eseguire un ulteriore controllo con uno strumento esterno come PhpStan, che molti sviluppatori hanno ignorato. Ora il controllo viene fatto direttamente a runtime (quando l'applicazione è in esecuzione) e non può essere aggirato.

Tuttavia, PHP conosce un certo tipo di tipo di unione dalla versione 7, quando era possibile dire che il tipo principale poteva anche essere `nullable`, cioè accettava il tipo di dati principale più il valore `null`.

Questo è stato scritto in due modi, ognuno con un significato diverso:

```php
function setPhone(?string $phone): void
{
	// implementazione
}

// o

function setPhone(string $phone = null): void
{
	// implementazione
}

// o combinazione

function setPhone(?string $phone = null): void
{
	// implementazione
}
```

Tutte le voci dicono che il telefono `int` (intero) è una `stringa` o `null`.

- La prima notazione richiede sempre il passaggio del valore
- La seconda notazione non richiede che venga passato alcun valore; se non viene passato nulla, il valore predefinito è `null` (questo è un argomento opzionale)
- La terza voce è una combinazione delle opzioni e si comporta come la seconda voce

Quando si usano i tipi di unione, non potremo più usare una notazione con un punto interrogativo e dovremo definire rigorosamente il tipo di dati `null`, per esempio:

```php
function setPhone(string|int|null $phone = null): void
{
	// implementazione
}
```

Il numero di telefono deve ora essere `stringa`, `int` o `null`.

I tipi di unione hanno ancora un certo numero di usi, che gli sviluppatori avanzati leggeranno nella documentazione o nell'implementazione di librerie specifiche.


JIT - elaborazione più veloce degli script
----------------------------------

Il compilatore JIT (just in time) porta un miglioramento significativo nelle prestazioni di complicazione degli script (parsing e comprensione). Tuttavia, questo comportamento può variare nel contesto delle richieste web.

Ora puoi vedere se hai abilitato JIT nella barra Tracy all'interno del framework Nette, e vedi [articolo separato](https://stitcher.io/blog/php-jit) per maggiori dettagli.

La cosa generale da dire sulla compilazione è che PHP cerca di elaborare il codice in anticipo in modo che quando elabora una particolare richiesta, non deve passare attraverso un file di script fisico, analizzarlo e interpretarlo. In passato, questo veniva gestito tramite l'estensione OPCache (che i server e gli host hanno disponibile di default) e migliorava la velocità di elaborazione di circa la metà.

Come regola generale, se avete un'applicazione lenta, è sempre meglio scegliere un algoritmo adatto a gestire un particolare compito piuttosto che fare micro-ottimizzazioni nel codice. Di solito i grandi ritardi sono causati dall'attesa del database e dalle sue lente query, dal salvataggio delle sessioni, dall'attesa che lo spazio sul disco rigido si liberi, e da altre operazioni hardware.

Operatore nullsafe (concatenamento opzionale)
-------------------------------------

Molto spesso in un'applicazione reale, è necessario verificare l'esistenza di un valore di ritorno (che non sia `null`) da un metodo e poi chiamare condizionatamente un altro. Gli [operatori ternari] (/ternary-operator) sono ottimi per questo, ma lavorano solo con una condizione e non possono essere annidati. L'operatore nullsafe permette la nidificazione in modo nativo.

> **TIP:** Praticamente lo stesso comportamento è già supportato dal sistema di template Latte, ma esso sovrascrive questo tipo di sintassi nel codice nativo di PHP, quindi si può usare l'operatore nullsafe sulle vecchie versioni di PHP (da PHP 7 in poi). Complimenti a David per questa modifica!

Questo lo rende facile da usare:

```php
$orderId = $order?->getId();
```

La variabile `$orderId` contiene o il valore restituito dal metodo `getId()`, o `null` se la variabile `$order` contiene il valore `null` e il metodo `getId()` non può essere chiamato.

Questo tipo di problema è stato aggirato in PHP 7 con la seguente sintassi tramite l'operatore ternario:

```php
$orderId = isset($order) ? $order->getId() : null;
```

Forse una condizione:

```php
if (isset($order)) {
	$orderId = $order->getId();
} else {
	$orderId = null;
}
```

La voce può essere scritta più avanti nella chiamata. Ho preso il campione da [Latte documentation](https://latte.nette.org/cs/syntax#toc-volitelne-retezeni-s-nullsafe-operatorem), che lo descrive perfettamente:

```php
$orderName = $order->item?->name;

// lo stesso di:

$orderName = isset($order->item) ? $order->item->name : null;
```

L'uso tipico è quando si elencano strutture più complesse in un template, per esempio in Latte appare così (esempio tratto dalla documentazione):

```php
{$user?->address?->street}
// significa approx ($user !== null) && ($user->address !== null) ? $user->address->street: null

{$items[2]?->count}
// sostituire approx ($items[2] !== null) ? $items[2]->count : null

{$user->getIdentity()?->name}
// sostituire ca $user->getIdentity() !== null ? $user->getIdentity()->name: null
```

Nel codice reale può apparire così, per esempio, che vogliamo scoprire il paese del cliente leggendo il suo profilo (e avete i dati nel database memorizzati bene tramite sessioni, come si suppone), allora nel vecchio PHP sembrava così:

```php
$country =  null;
if ($session !== null) {
    $user = $session->user;
    if ($user !== null) {
        $address = $user->getAddress();
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

Ora può essere accorciato in una sola riga:

```php
$country = $session?->user?->getAddress()?->country;
```

L'uso dell'operatore nullsafe previene anche vari bug che non potrebbero essere facilmente rilevati da uno sviluppatore inesperto in PHP 7.

Per esempio, questa voce genererà un errore fatale:

```php
var_dump($invoice->getDate()->format('Y-m-d') ?? null);

// return: fatal error: uncaught Error: call to a member function format() on null
```

La sintassi corretta è la seguente:

```php
var_dump($invoice->getDate()?->format('Y-m-d'));

// restituisce: null
```

Argomenti nominati
---------------------

Nel buon vecchio PHP, le chiamate di funzione con argomenti dovevano essere scritte passando gli argomenti nell'ordine esatto definito dalla funzione di destinazione. Non c'è niente di male in questo, tuttavia, quando si usa un certo numero di parametri con valori simili, potrebbe causare una scarsa leggibilità. O se volevamo passare fino all'ennesimo parametro nell'ordine, tutti i parametri opzionali dovevano essere passati prima, il che poteva avere un effetto negativo sulla leggibilità e la compatibilità in avanti.

Immaginate, per esempio, la funzione `setCookie()` in Nette, che ha molti argomenti:

```php
public function setCookie(
	string $name,
	string $value,
	$time,
	string $path = null,
	string $domain = null,
	bool $secure = null,
	bool $httpOnly = null,
	string $sameSite = null
)
```

I primi tre argomenti (`$name`, `$value` e `$time`) sono obbligatori, ma se vogliamo passare l'argomento `$httpOnly`, dobbiamo passare tutti i precedenti e calcolare l'ordine correttamente:

```php
$http->setCookie(
	'myCookie',
	'A David piacciono i cavalli',
	'ora',
	null, // percorso
	null, // dominio
	null, // sicuro
	true
);
```

Cosa che semplicemente non si vuole fare se non è necessario.

La scrittura elegante si presenta quindi come:

```php
$http->setCookie(
    name: 'myCookie',
    value: 'A David piacciono i cavalli',
    time: 'ora',
    httpOnly: true
);
```

Questo tipo di sintassi richiede che i nomi degli argomenti nella funzione di destinazione non cambino mai, perché saranno ancora scritti quando vengono chiamati. Almeno gli sviluppatori saranno in grado di nominarli meglio.

Se vogliamo usare solo uno degli argomenti, la sintassi può essere combinata e condensata in una sola riga:

```php
$http->setCookie('myCookie', 'A David piacciono i cavalli', 'ora', httpOnly: true);
```

I primi 3 argomenti sono passati nel modo originale, poi viene passato l'argomento opzionale `httpOnly` (perché è nominato).

Attributi
---------

La maggior parte dei principali linguaggi come Java o C# includono già nativamente le cosiddette annotazioni, che sono una sintassi del linguaggio nativo che permette di aggiungere meta informazioni ad altri costrutti del linguaggio.

In PHP, questo tipo di sintassi è stato a lungo assente, ed è stato aggirato usando i commenti DOC, che è un classico commento su un metodo, tranne che ha due asterischi `/**`.

Questi commenti sono ignorati durante l'elaborazione dello script e una speciale logica utente deve essere aggiunta per analizzarli e interpretarli in fase di esecuzione tramite reflection. Probabilmente puoi capire l'impatto sulle prestazioni che questo può avere, inoltre la sintassi dei commenti non può essere richiesta ed è molto difficile da controllare in fase di compilazione (quando lo script viene elaborato prima di essere eseguito), e ancora una volta devi usare strumenti aggiuntivi al di fuori del normale toolkit PHP per farlo.

Per preservare la compatibilità all'indietro, PHP fornisce attributi con una sintassi simile alla notazione alternativa dei commenti, che non interrompe l'esecuzione dello script su PHP legacy.

La notazione originale (usata, per esempio, per le dipendenze Inject in Nette Presenter):

```php
final class HomepagePresenter extends BasePresenter
{
	/** @inject */
	public EntityManager $entityManager;
}
```

Ora puoi rimuovere il commento e usare l'attributo nativo:

```php
use App\Attributes\Inject;

final class HomepagePresenter extends BasePresenter
{
	#[Inject]
	public EntityManager $entityManager;
}
```

È molto importante che l'attributo non sia più solo un pezzo di stringa in un commento, ma una classe fisica che è codice PHP valido.

Questo è ottimo, perché ora potete validare in modo sicuro gli input di un attributo, e l'uso di un attributo diventa effettivamente una chiamata al suo costruttore dove può essere usata altra logica. Non vedo l'ora di vedere questo supportato nativamente da Doctrine, che usa le annotazioni per tutto.

L'implementazione dell'attributo stesso potrebbe quindi assomigliare a questo:

```php
#[Attribute]
class Inject
{
    public string $value;

    public function __construct(string $value)
    {
        $this->value = $value;
    }
}
```

La logica rigorosa può essere usata di nuovo all'interno degli attributi, come il controllo dei tipi di dati degli argomenti, dei tipi di unione e di altre caratteristiche del linguaggio.

Espressione di corrispondenza
----------------

Il nuovo costrutto di linguaggio `match()` è un miglioramento modernizzato del buon vecchio `switch()` (che cerco di non usare), e porta una serie di caratteristiche interessanti (che mi faranno ricominciare ad usarlo).

Per esempio, vogliamo modificare il valore di una variabile in base all'input:

```php
$pozdrav = match(bool $formal) {
    true => 'Ciao',
    false => 'Ciao',
};
```

Una nuova importante caratteristica della sintassi è che non dobbiamo usare `break` (come il vecchio `switch`) e la sintassi è generalmente molto più economica.

Allo stesso tempo, possiamo validare più input contemporaneamente all'interno di una condizione (separati da una virgola) ed eventualmente restituire un valore predefinito (quando nessuno soddisfa).

Questo è utile quando si riscrive il codice di condizione HTTP in un messaggio di errore, per esempio (lo apprezzerete sicuramente quando gestite i codici di eccezione):

```php
$message = match ($statusCode) {
    200, 300 => null,
    400 => 'non trovato',
    500 => 'errore del server',
    default => 'codice di stato sconosciuto',
};
```

Il confronto dei valori è fatto rigorosamente dall'operatore `===` (switch usa solo `==`), il che dimostra ancora una volta che PHP segue il percorso di progettazione rigoroso. Pertanto, l'input `'200'` (una stringa contenente un numero) non sarà accettato nel caso precedente.

Se non si specifica un valore per `default` e non c'è corrispondenza, viene lanciato un `UnhandledMatchError`.

La nuova sintassi permette anche di usare un'espressione o una chiamata di funzione per la corrispondenza (si comporta come una condizione). In caso di errore, possiamo lanciare un'eccezione (poiché il token `throw` è ora un'espressione e può essere usato in questo modo):

```php
$message = match ($statusCode) {
    200 => null,
    $this->checkServerError($statusCode) => throw new ServerError(),
    default => 'codice di stato sconosciuto',
};
```

Propagazione delle proprietà nel costruttore
-------------------------------------------------

Questo è solo uno zucchero sintattico che sarà utile per definire rapidamente e facilmente un'entità e le sue proprietà direttamente nel costruttore.


Per esempio, l'entità originale:

```php
final class User
{
    public string $name;

    public function __construct(
        string $name,
    ) {
        $this->name = $name;
    }
}
```

Può essere abbreviato solo in:

```php
final class User
{
    public function __construct(
        public string $name
    ) {}
}
```

La proprietà `$name` è validata rispetto al tipo di dati `string` e il suo valore può essere letto direttamente dall'istanza perché è una proprietà pubblica. Inoltre, se usate SmartObject in Nette (cosa che piuttosto non raccomando per PHP 8), potete accedere alle proprietà private chiamando prima il loro metodo getter, e questa sintassi semplifica ancora le cose.

Tipo di ritorno statico
--------

In passato, potevamo usare il tipo di dati `self` come valore di ritorno di un metodo, ma esso restituisce un'istanza della classe stessa in cui è definito. Il datatype `statico` può generalmente farlo anche nel caso dell'ereditarietà, e restituirà il datatype della classe da cui l'istanza è stata eseguita, non il suo antenato.

Per esempio:

```php
class BaseEndpoint
{
    public function getInstance(): static
    {
        return new static();
    }
}
```

Tipo di dati misto
------------------

Il tipo `mixed` può ora essere usato come argomento di una funzione o di un metodo. Questo significa che il metodo deve sempre accettare qualche input (ed è quindi un argomento obbligatorio).

Se potete almeno un po', usate sempre un tipo di dati diretto, o almeno l'unione. Mixed è utile solo se la funzione accetta davvero qualsiasi cosa. In pratica, l'uso è utile, per esempio, per varie funzioni di dump che accettano input arbitrari e devono essere in grado di visualizzarli.

Il tipo `mixed` accetta i seguenti tipi: `string`, `int`, `float`, `null`, `bool`, `array`, `callable`, `object`, `resource`.

David userà poi il tipo misto per la sua funzione:

```php
function bdump(mixed $var): mixed
{
	Tracy\Debugger::barDump($var);
	return $var;
}
```

Lancio del token come espressione
------------------------

Il token `throw` è ora diventato un'espressione, questo significa in pratica che un'eccezione può essere lanciata quando la funzione lambda `fn()` viene troncata, o quando viene controllato un operatore ternario:

```php
$error = fn () => throw new \InvalidArgumentException('Questo lancia sempre un errore.');

$userName = $user['nome'] ?? throw new \LogicException('L'utente deve avere un nome.');
```

La funzione str_contains()
-----------------------

PHP finalmente include una funzione nativa per controllare che la stringa predefinita contenga una sottostringa.

Per esempio:

```php
if (str_contains('A Honzik piacciono i gatti.', 'gatti')) {
	echo 'La funzione gestisce i gatti.';
}
```

In passato, l'occorrenza di una sottostringa era verificata dalla funzione [strpos](/strpos-function):

```php
if (strpos('A Honzik piacciono i gatti.', 'gatti') !== false) {
	echo 'La funzione gestisce i gatti.';
}
```

Funzioni str_starts_with() e str_ends_with()
----------------------------------------------

Una coppia di nuove funzioni per controllare se una stringa inizia o finisce con una sottostringa:

```php
str_starts_with('A Honzik piacciono i gatti.', 'Honzik'); // vero

str_ends_with('A Honzik piacciono i gatti.', 'gatti.'); // vero
```

Funzione get_debug_type()
-------------------------

Migliora l'output della funzione esistente [gettype](/function-gettype), che restituisce solo il tipo generico della variabile passata. La funzione è usata, per esempio, quando si lancia un'eccezione, quando otteniamo un input non valido e vogliamo dire all'utente cosa ha effettivamente passato.

Quando chiamiamo la funzione `gettype()` con una variabile contenente un'istanza della classe `App\User`, la funzione restituisce `object`, quindi non sappiamo che classe sia. La nuova funzione `get_debug_type()` restituisce il nome della classe.

La funzione get_resource_id()
--------------------------

Questa funzione restituisce l'identificatore di una risorsa esterna da una variabile.

Per esempio, la connessione a un database MySql è gestita da PHP utilizzando uno speciale tipo di dati `resource`, ora è possibile scoprire quale ID è stato assegnato ad esso.

> **Nota storica:**
>
> Il tipo `resource` in PHP è stato creato in un momento in cui non sapeva ancora come usare gli oggetti e doveva in qualche modo capire come passare riferimenti a qualcosa come un `data type`. In futuro, ci si può aspettare che `resource` venga rimosso del tutto dal linguaggio, quindi è meglio non usare questa caratteristica.

L'estensione ext-json è sempre disponibile
-------------------------------------

In passato, PHP poteva essere compilato senza supporto per json. Ora, json sarà sempre disponibile, quindi puoi rimuovere la dipendenza `ext-json` dai tuoi file `composer.json` e sapere sempre che json può essere usato.

Precedenza di concatenazione
-------------------------------------------------------

Immaginate qualcosa come:

```php
echo 'La somma totale:' . $a + $b;
```

L'aggiunta del numero viene fatta prima, o la variabile `$a` viene prima aggiunta alla stringa e poi l'intera nuova stringa viene aggiunta a `$b`?

Ci si aspetterebbe che l'aggiunta venga fatta per prima, ma è una bella supposizione. PHP in realtà fa qualcosa del genere:

```php
echo ('La somma totale:' . $a) + $b;
```

PHP 8 ora si comporta in modo prevedibile:

```php
echo 'La somma totale:' . ($a + $b);
```

In generale, però, è sempre meglio usare le parentesi per delimitare un'espressione.

Ordinamento stabile
---------------

Prima di PHP 8, l'ordinamento delle stringhe era fatto usando il cosiddetto algoritmo instabile, il che significa che PHP non garantiva che gli elementi con lo stesso valore (o altrimenti equivalente) non sarebbero stati scambiati. La nuova versione cambia il comportamento di tutte le funzioni di ordinamento in stabile, così l'ordinamento è sempre fatto in modo deterministico e si ottiene sempre lo stesso output.

Questo risolve, per esempio, i casi in cui classificavamo le valutazioni degli utenti per rilevanza, ma alcune valutazioni avevano lo stesso punteggio. Ora appariranno nello stesso ordine ogni volta che si ordina e non salteranno continuamente.

Altre nuove caratteristiche
---------------

PHP ha molte altre nuove caratteristiche e miglioramenti minori. Per esempio, gli errori saranno lanciati in modo diverso (ma questo non disturba noi che scriviamo codice senza errori, giusto?).

Puoi sempre vedere l'elenco completo dei cambiamenti nella documentazione ufficiale e nel post RFC.

Cosa mi manca nel nuovo PHP
-----------------------

Mi piacerebbe che PHP supportasse finalmente i tipi di array composti, per esempio quando un metodo restituisce un array di identificatori dobbiamo ancora specificare solo `getIds(): array` e qualcosa come `getIds(): int[]` sarebbe molto meglio. Forse lo vedremo presto e il controllo dei tipi forti sarà completo.

Altre risorse
------------

David Grudl ha fatto un bel discorso sulle novità di Posobot. Vi consiglio di guardare la registrazione:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/ZYkwmcCUTCg" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Questo è per ringraziare David per la sua conferenza, dato che ne ho tratto alcune informazioni per questo articolo. In particolare, roba sul passaggio di Nette a PHP 8 e altri consigli dietro le quinte su PHP.
