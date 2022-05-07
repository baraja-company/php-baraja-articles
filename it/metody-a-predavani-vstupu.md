Metodi in DPI e trasferimento di input
======================================

> id: '843fbfb4-daf2-4c2e-9d94-28d494025b2e'
> slug:
> 	cs: metody-a-predavani-vstupu
> 	it: metodi-in-dpi-e-trasferimento-di-input
> 
> publicationDate: '2020-02-16 20:49:35'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: a3e1bca690ed70479ef9807a1f2a1f23

I metodi rappresentano il comportamento di un oggetto perché permettono di lavorare con il suo stato interno e di influenzare gli oggetti tra loro.

Rappresentare i metodi nel mondo reale
----------------------------------

Considerate un qualsiasi oggetto del mondo reale, per esempio un gatto. Il gatto ha certe proprietà (nome, colore, peso, ...) che descriviamo usando le proprietà e poi anche il comportamento (miagolare, camminare, dormire, ...) e lo descriviamo usando i metodi. Poiché ci sono molti gatti nel mondo ("E così abbaio come un tuono, che ci siano un milione di gatti") è importante ricordare che un metodo è qualcosa di generale che si applica a tutti gli oggetti di un dato tipo allo stesso modo.

Attuazione pratica di un metodo
-----------------------------

In termini di sintassi del linguaggio, definiamo una classe per un gatto:

```php
class Cat
{
    public string $name;

    public string $sound = 'Miao';
}
```

Dopo aver creato un'istanza di un particolare gatto, possiamo semplicemente elencare, per esempio, il suono:

```php
$cat = new Cat;

echo $cat->sound; // dice "Miao"
```

Ma cosa succede se vogliamo formattare il suono in un certo modo quando lo scriviamo? Poi entrano in gioco i metodi:

```php
class Cat
{
    public string $name;

    public string $sound = 'Miao';

    public function getFormattedSound(): string
    {
        return 'Sto facendo il suono "' . $this->sound . '"!';
    }
}
```

Dovete già conoscere il principio delle funzioni del passato. Le classi permettono di scrivere una funzione direttamente nel suo corpo con accessibilità definita (come per le proprietà) e sono chiamate metodi.

La variabile `$this` si comporta un po' "magicamente". Memorizza l'istanza corrente dell'oggetto in cui ci troviamo. Se vogliamo leggere un valore da una proprietà o chiamare un altro metodo all'interno di un metodo, dobbiamo solo farlo sulla variabile `$this`.

Il metodo viene poi chiamato dall'interno dell'oggetto come una funzione classica (con una parentesi alla fine) per dire che non è una proprietà:

```php
$cat = new Cat;

echo $cat->sound; // dice "Miao"

echo $cat->getFormattedSound(); // stampa 'Sto facendo un suono 'Meow'!
```

Costruttore - metodo chiamato quando si crea un'istanza
--------------------------------------------------

Quando si crea un'istanza di un oggetto, abbiamo spesso bisogno di definire come impostare il suo stato di base e quali parametri (dati di input) sono obbligatori.

In OOP per risolvere questo problema c'è un metodo pubblico speciale `__construct` che possiamo implementare volontariamente e che viene chiamato sempre e solo quando si crea un'istanza.

Esempio pratico:

```php
class Cat
{
    public string $name;

    public string $sound;

    public function __construct(string $name, string $sound)
    {
        $this->name = $name;
        $this->sound = $sound;
    }

    public function getFormattedSound(): string
    {
        return 'Sto facendo il suono "' . $this->sound . '"!';
    }
}
```

Definendo il costruttore, ci siamo assicurati che quando si crea un'istanza, dobbiamo sempre passare 2 parametri obbligatori (`name` e `sound`) e l'oggetto sarà impostato direttamente.

Poiché il costruttore è un metodo classico, può fare qualcos'altro all'interno con i dati inseriti. Per esempio, riformattare la stringa in modo appropriato, ma ce ne occuperemo più tardi.

Creare un'istanza è poi di nuovo facile, dobbiamo solo passare i parametri iniziali (sono scritti tra parentesi al nome della classe dove viene chiamato il nome della classe e viene creata l'istanza):

```php
$cat = new Cat('Minda', 'Vrr');

echo $cat->name; // C'è scritto "Minda"
echo $cat->sound; // stampa "Vrr"

echo $cat->getFormattedSound(); // stampa 'Sto facendo un suono "Vrr"!
```

Gettery e settery
-----------------

Alcuni metodi sono chiamati `getters` e `setters`. Questi sono metodi comuni, è solo una convenzione per chiamarli così. I metodi sono utilizzati per ottenere e inserire dati in un oggetto. Il vantaggio principale è che possiamo convalidare i dati prima di inserirli ed eventualmente correggerli o rifiutarli e lanciare un errore.

Per ogni proprietà che può essere recuperata (vogliamo abilitare il recupero dei dati), si usa creare un getter che inizia con la parola `get`. Se la proprietà restituisce un booleano (`vero` o `falso`), è consuetudine denominare l'inizio del nome del metodo con la parola `is`.

Esempio semplificato:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->setName($name);
    }

    public function getName(): string
    {
        return $this->name;
    }

    public function isEmpty(): bool
    {
        return $this->name === '';
    }

    public function setName(string $name): void
    {
        $this->name = trim($name);
    }
}
```

Quando si crea un'istanza di gatto, si passa il suo nome al costruttore (che è sempre obbligatorio). Tuttavia, poiché vogliamo condividere la logica di inserimento del nome sia per il costruttore che per il setter (il metodo per inserire un nuovo nome), chiamiamo il metodo `setName()` dentro il costruttore per assicurarci che il nome sia inserito.

All'interno del setter, usiamo la funzione <a href="/function-trim">trim()</a>, che rimuove automaticamente gli spazi bianchi da entrambi i lati della stringa inserita.

Possiamo usare il metodo `getName()` per l'output. Il metodo `isEmpty()` può essere usato per controllare il vuoto.

> **Vantaggi pratici:**
>
> Un enorme vantaggio dei getter e dei setter sono i **tipi di dati garantiti**. Se un getter dichiara di restituire una `stringa`, possiamo sempre fare affidamento su di esso e ottenere sempre una stringa.
>
> Un setter nella sua implementazione dichiara anche di accettare una `stringa`, il che significa per l'applicazione che qualsiasi cosa l'utente inserisca otterrà il suo input convertito in una `stringa` (se usa un tipo compatibile, come `integer`) o otterrà un errore che il tipo non può essere convertito (come `array`).

Il maggior valore aggiunto di getter e setter sarà apprezzato soprattutto nell'uso pratico.

Il metodo magico `__toString()`
-----------------------------

All'interno di una classe, si può definire un metodo magico `__toString()` che viene chiamato automaticamente quando si cerca di scrivere un oggetto come stringa. Il metodo deve sempre restituire una stringa.

Esempio di implementazione:

```php
class Cat
{
    public string $name;

    public function __construct(string $name)
    {
        $this->name = $name;
    }

    public function __toString(): string
    {
        return 'Ciao, sono' . $this->name . ';)';
    }
}
```

L'uso è quindi il seguente:

```php
$cat = new Cat('Minda');

echo $cat; // Dice "Ciao, sono Minda ;)"
```

Riassunto
-------

Abbiamo mostrato come definire i metodi e poi chiamarli all'interno di un'istanza dell'oggetto.

La prossima volta, vedremo il <a href="/incapsulamento">principio di incapsulamento</a>, che sfrutta pienamente le proprietà dei metodi.
