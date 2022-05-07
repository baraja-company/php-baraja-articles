Classi autocaricanti in PHP
===========================

> id: f6cd5762-261f-4153-b27b-075dd8b5ed13
> slug:
> 	cs: autoloading-trid
> 	it: classi-autocaricanti-in-php
> 
> publicationDate: '2020-02-09 10:00:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '441a1d4107bb32e8cfe4dbb926c2decd'

Sono sicuro che lo sai, quando programmiamo script PHP dividiamo il codice in molti file e per avere tutte le parti disponibili le carichiamo con una serie di chiamate `include`, `require` o preferibilmente `require_once`, che garantisce il caricamento solo una volta.

Nel codice appare così:

```php
require_once 'Router.php';
require_once 'Pagina.php';
require_once 'Paginator.php';
```

Lo svantaggio principale di questo approccio è che il programmatore deve costantemente assicurarsi che tutto sia sempre caricato. Ma se carica molto, perde prestazioni inutilmente e raggiunge il disco molte volte. Quindi la soluzione manuale ha solo problemi.

Autocaricamento nativo
-------------------

Fortunatamente, c'è un supporto nativo in PHP per il cosiddetto **Class Autoloading**, che è la logica nel codice che carica un file di classe solo quando è necessario (tipicamente quando un'istanza viene creata per la prima volta).

Una semplice implementazione potrebbe quindi apparire come questa:

```php
spl_autoload_register(function (string $className): void {
    include 'src/' . $className . '.php';
});

$obj  = new MyClass1();
$obj2 = new MyClass2();
```

Quando si crea un'istanza della classe `MyClass1`, la funzione `spl_autoload_register` legge il file `MyClass1.php` dalla directory `src`. Questa implementazione presuppone che ogni classe sia in un file separato chiamato dal nome della classe o dell'interfaccia.

Casi più complessi di autoloading
-------------------------------

In un'applicazione reale, possono sorgere molte situazioni spiacevoli che complicano l'autoload, per esempio:

- La classe o l'interfaccia non esiste affatto
- Il file è già stato caricato una volta
- Ci sono più classi o interfacce nello stesso file
- Il percorso della classe o dell'interfaccia non corrisponde al nome
- La posizione della classe o dell'interfaccia cambia nel tempo

Tuttavia, non abbiamo bisogno di programmare la nostra soluzione per tutto questo, ma possiamo usare la soluzione esistente una volta progettata.

Se stai usando <a href="https://getcomposer.org/doc/01-basic-usage.md">Composer</a>, probabilmente stai usando anche il suo autoloading nativo. Questo perché quando si installa qualsiasi pacchetto, Composer genera automaticamente una `class map`, che è una panoramica delle classi e delle loro posizioni fisiche.

Poi all'inizio del codice (tipicamente in `index.php`) basta usare:

```php
require __DIR__ . '/vendor/autoload.php';
```

Tuttavia, l'autoloading viene generato solo una volta quando viene chiamato il comando `composer dump`, quindi è necessario rigenerare l'autoloading ogni volta che l'applicazione cambia.

RobotLoader - una soluzione elegante per lo sviluppo
----------------------------------------

Per lo sviluppo locale, mi piace molto il pacchetto `nette/robot-loader`, che attraversa automaticamente la struttura delle directory e memorizza le posizioni delle classi. Così se carichiamo una classe, prima guarda nella cache e solo se non esiste, reindicizza automaticamente il progetto. Pertanto, il programmatore non deve tenere traccia di dove si trova qualsiasi file o classe e semplicemente programmare.

Installazione tramite Composer:

```txt
composer require nette/robot-loader
```

La spiegazione di base della funzionalità è descritta nella <a href="https://doc.nette.org/cs/3.0/robotloader">documentazione</a> stessa:

> Simile a come il robot di Google scansiona e indicizza le pagine web, RobotLoader scansiona tutti gli script PHP e nota quali classi, interfacce e tratti ha trovato in essi. Poi mette in cache i risultati della sua ricerca e li usa nella richiesta successiva. Quindi devi solo specificare quali directory sfogliare e dove fare la cache.

È poi estremamente facile da usare:

```php
$loader = new Nette\Loaders\RobotLoader;

// aggiungere le directory che RobotLoader dovrebbe indicizzare
$loader->addDirectory(__DIR__ . '/app');
$loader->addDirectory(__DIR__ . '/libs');

// imposta la cache su disco nella directory 'temp'.
$loader->setTempDirectory(__DIR__ . '/temp');
$loader->register(); // avviare RobotLoader
```

L'impostazione `$loader->setAutoRefresh(true o false)` determina se RobotLoader deve reindicizzare i file quando incontra una nuova classe. Questo dovrebbe essere disabilitato sui server di produzione.

Ecco, ora non dovrete più avere a che fare con l'autoloading.

Soluzione combinata
------------------

Uso una soluzione combinata quando sviluppo un progetto reale.

Il modo in cui funziona nella vita reale è che carico i pacchetti installati tramite Composer autoload (che è molto efficiente) e questo risolve il caricamento di tutte le classi nella directory `vendor`.

Il codice per il progetto specifico viene poi messo nella directory `app`, dove gestisco l'autocaricamento di poche classi tramite RobotLoader. La cosa importante è mantenere sempre l'applicazione concreta più piccola possibile e usare il più possibile pacchetti già pronti, questo favorisce molto la riusabilità.

La struttura del progetto si presenta così:

```txt
/app
    Bootstrap.php <-- konfigurace
    /model
        UserForm.php <-- projektové třídy
        RegisterFactory.php
        ...
/vendor
    ... <-- knihovny
/www
    index.php <-- inicializace
```

Test di autocaricamento
------------------------

A volte può succedere che non tutti i file si carichino sempre e che si trovino dei problemi.

Per il debug, raccomando la funzione <a href="/get-list-of-all-loaded-files">get_included_files()</a>.
