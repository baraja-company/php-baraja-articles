Come nominare variabili, funzioni, metodi e classi
==================================================

> id: f70ac75d-422f-4696-88d5-9e1b843e060a
> slug:
> 	cs: jak-pojmenovat-veci-v-php
> 	it: come-nominare-variabili-funzioni-metodi-e-classi
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: '10510114a0160c0826c9ee1f54c95b03'

Per mantenere il codice in ordine, è importante scegliere regole chiare su come derivare i nomi. Questa pagina serve come una panoramica degli approcci relativamente popolari usati da un gran numero di programmatori, incluso me e le persone con cui lavoro.

Se lavorate in un team di sviluppo, usate pure le loro regole, ma per lo sviluppo generale è altrettanto vantaggioso stabilire alcune buone abitudini.

Poiché il concetto dell'intera sintassi PHP è molto ampio, ho diviso la guida qui in molte categorie che sono altamente specializzate.

Se cercate una soluzione rapida, vi consiglio di studiare <a href="https://www.php-fig.org/psr/psr-4/">lo standard PSR-4</a>.

Inserimento di script, collegamento a HTML, caricamento di file
---------------------------------------------------

Ogni script deve iniziare con il tag `<?php`.

Se non c'è **nessun HTML** alla fine del file, non dovrebbe essere terminato (per evitare un carattere bianco alla fine della pagina).

Il caricamento di altri file dovrebbe seguire le seguenti regole:

- Se il file non è critico e può essere eliminato durante il rendering della pagina (per esempio, informazioni aggiuntive nel footer), dovrebbe essere caricato tramite include: `include 'file.php';`
- File critici solo tramite il costrutto require: `require 'file.php';`
- Se stiamo lavorando con oggetti, usiamo il costrutto require per caricare l'autoloader, che si prenderà cura di caricare le altre classi (la maggior parte dei framework implementa questo comportamento).


Se è almeno un po' possibile, dovremmo separare la logica di recupero dei dati dal rendering in HTML, cioè usare il modello MVC (model - view - controller).

Quindi prima prepariamo i dati in, diciamo, Presenter:

`Presenter.php`.

```php
$cisla = [1, 2, 3];

$data = [];
$data['numeri'] = $cisla; // passare i dati al modello
include 'renderCisel.php'; // caricare il modello
```

E poi disegnarlo nel modello:

`renderCisel.php`.

```html
<table>
<?php
    foreach ($data['cisla'] as $cislo) {
        echo '<tr><td>' . $cislo . '</td></tr>';
    }
?>
</table>
```

Questo approccio è usato dalla maggior parte dei framework ed è utile per aumentare la chiarezza del codice. Nella parte successiva del processo di sviluppo, questo approccio dovrebbe essere usato da ogni programmatore esperto che vuole sviluppare applicazioni chiaramente strutturate (per applicazioni grandi questo approccio è un must).

Nomi delle variabili
----------------

Se una variabile contiene un array di valori o altri oggetti, dovrebbe essere nominata al plurale:

```php
$numbers = [1, 2, 3];
```

Perché allora possiamo semplicemente iterare i valori per un singolo numero:

```php
foreach ($numbers as $number) {
    // elaborazione dei numeri
}
```

Un nome composto da più parole è combinato in una parola lunga con la sintassi cameCase, cioè la prima parola inizia con una lettera minuscola, ogni parola successiva con una lettera maiuscola:

```php
$promenna = 'Ehi! Ehi!';
$seznamUzivatelu = [
   'Jan Barášek',
   'Barack Obama',
   'Steve Jobs',
   'Stephen Wolfram',
];
$maxFilesInDirectory = 12;
$nameOfPhpScript = 'index.php'; // L'abbreviazione PHP è stata ridotta in dimensioni
```

Nomi di funzioni e metodi
--------------------

Le funzioni e i metodi dovrebbero sempre rendere chiaro nel loro nome ciò che fanno. Spesso i parametri di input previsti e il valore di ritorno possono anche essere inclusi nel nome.

Prova a indovinare cosa fanno le seguenti funzioni e qual è il loro valore di ritorno:

```php
getUserById($id);
saveErrorToLog($message);
createDefaultDirectory($path);
setAuthors(['Jan Barášek', 'Chuck Norris']);
getCurrentTime();
```

Tutto il trucco sta nella prima parola del nome, che rende chiaro quale metodo utilizzerà la funzione. Di solito si segue la seguente convenzione:

- `get` - recupera i dati come array o oggetto, i parametri di input specificano l'entità da cercare
- `save` - salva in un file o in un database
- `create` - crea un'entità (per esempio, crea un'istanza di un oggetto)
- `set` - salvare dati in una variabile predefinita (all'interno di una funzione)

Nomi di classe
----------

Una classe è una grande entità che contiene un gran numero di proprietà e metodi, quindi dovrebbe anche iniziare con una lettera maiuscola. Una classe dovrebbe anche portare una sola entità (e descrivere le sue proprietà), quindi dovrebbe essere nominata con un nome singolare. Se abbiamo bisogno di lavorare con più entità, possiamo semplicemente memorizzare ogni istanza in un array.

Esempio:

```php
class User
{
    public string $username;

    public string $password;

    public string $role;
}

class Users
{
    /** @var User[] */
    public array $users;

    public function addUser(User $user): void
    {
        $this->users = array_push($this->users, $user);
    }
}
```

La classe **User** è specializzata in informazioni su un solo utente specifico. Se vogliamo lavorare con più utenti, creiamo un'altra classe (envelope) che porta un array di istanze di un'entità specifica.

Le fabbriche possono spesso essere utili per questo, poiché ci permettono di creare facilmente oggetti simili e di riciclare le istanze originali, ottenendo un codice più chiaro e risparmiando risorse di sistema.

Spazio dei nomi - Namespaces
---------------------------

Mentre Namespace è indipendente dalla directory fisica in cui lo script è disponibile, è una buona pratica rispettare almeno parzialmente il layout del progetto (che porta ad un sistema migliore per la creazione di nuovi nomi che è più univoco in questo modo).

Personalmente nomino i namespace secondo la sottodirectory comune per le classi di quel tipo.

Esempi:

```php
App\Presenters; // Questi sono i nomi di tutti i presentatori
App\Model;      // Questo è il nome del modello generale
App\Model\Math; // Questo è il nome del modello che lavora con la matematica
```

Per un corretto caricamento automatico delle classi, è una buona idea seguire lo standard <a href="https://jakpsatphp.cz/PSR4/">PSR-4</a>.

Convenzioni personalizzate (per esempio, in un team aziendale)
-----------------------------------------

E tu come chiami il tuo? Apprezzerei suggerimenti su come migliorare questo articolo.

In generale, però, le convenzioni personalizzate all'interno di un team non hanno molto senso, perché rendono il codice più difficile da portare su altri framework e bisogna imparare le convenzioni attuali quando si assume un nuovo collega. È quindi meglio seguire lo standard `PSR-4`.
