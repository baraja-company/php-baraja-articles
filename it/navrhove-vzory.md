Modelli di progettazione in PHP
===============================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	it: modelli-di-progettazione-in-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

I design pattern sono modi di pensare alla programmazione.

Forniscono una raccolta di consigli, pratiche pronte, best-practice e approfondimenti sullo sviluppo. Per ogni paradigma di programmazione e tipo di compito, ci sono alcuni design pattern che sono più adatti.

Vantaggi - perché usare i design pattern?
---------------------------------------

Nella programmazione, risolvere certi tipi di problemi è ripetitivo, quindi ha senso scegliere un metodo per risolvere questi problemi e continuare a ripetere quel metodo.

Il beneficio principale si presenta soprattutto durante lo sviluppo in team, quando tutti sanno come l'applicazione sarà sviluppata (secondo quale design pattern) e lo applicano semplicemente. Questo elimina poi le inutili decine di ore di debugging del codice scritto in modo strano e cercando di capire i principi che l'autore intendeva.

MVC - un esempio di utilizzo di un design pattern
--------------------------------------

Il mio design pattern preferito è `MVC` (da `Model View Controller`), che dice che l'applicazione è divisa in 3 strati indipendenti che si chiamano l'un l'altro in modo sequenziale e si passano dati l'un l'altro.

Per esempio, quando si rende una pagina, potrebbe sembrare che prima decida che tipo di pagina è (per esempio, un dettaglio della categoria), quindi chiama `CategoryController` con il metodo `detail`.

Un esempio concreto (sto semplificando molto):

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Nota:**
>
> Questo è solo un codice di esempio che spiega il principio del design pattern `MVC`.
>
> In un'implementazione reale, dovremmo capire ulteriormente come, per esempio, ottenere un'istanza di `CategoryManager` e come passarla alla proprietà. Tipicamente, l'iniezione di dipendenza è usata per questo tipo di compito.

Prima di rendere la pagina per il dettaglio della categoria, il `CategoryController` viene chiamato per ricevere la richiesta effettiva (cioè, stiamo rendendo il dettaglio della categoria con un certo ID, che è ottenuto dal router, per esempio, nell'URL), ottenere i dati (interrogando il `Modello` corrispondente) e passare i dati finali al modello per il rendering.

L'enorme vantaggio di questo principio è che possiamo scrivere molti modelli (logica dell'applicazione) che è indipendente dal modo in cui i dati sono presentati (il modello), risultando in codice riutilizzabile. Infatti, se vogliamo usare il `CategoryManager` in un altro progetto, semplicemente passiamo i dati attraverso il `Controller` in un modo specifico, che sarà reso secondo il modello definito dal progetto stesso, mentre la logica dell'applicazione rimane la stessa e nessuno se ne preoccupa perché il livello del software ha adempiuto alla sua interfaccia e responsabilità concordate.

> **Nota pratica:**
>
> Il design pattern `MVC` è usato dalla maggior parte dei framework moderni come Nette, Symfony, Laravel e altri.
>
> Possiamo anche incontrare `MVC` nello sviluppo di app mobili e altri tipi di software dove abbiamo bisogno di ottenere dati e renderli in un modello secondo il tipo di pagina o di vista.

Importanti design pattern per lo sviluppo web
---------------------------------------

Generalmente nella programmazione ci sono molti design pattern che non sono adatti allo sviluppo web. Questa lista descrive i modelli più importanti che io stesso uso e con cui dovreste avere familiarità.

Una <a href="/categoria-design-patterns">elenco completo di tutti i design pattern</a>, esempi del loro uso e spiegazioni dettagliate si possono trovare in una pagina separata.

- **MVC** - Il principio di dividere un'applicazione in `Modello` (logica dell'applicazione e dati), `Vista` (template e vista dei dati) e `Controller` (che collega `Modello` e `Vista`).
- **Iniezione di dipendenza** - Invece di creare istanze di classe, definiamo i cosiddetti servizi che abbiamo iniettato automaticamente. Il codice ammette tutte le dipendenze, le singole parti possono essere scambiate e noi costruiamo l'applicazione dinamicamente.
- **Singleton (Singleton)** - Ogni classe ha una sola istanza (personalmente lo uso in combinazione con la `Dependency injection`, dove ogni servizio ha una sola istanza che viene passata attraverso l'applicazione).
- **Lazy Initialization (Inizializzazione ritardata)** - Creiamo un'istanza di un oggetto quando è necessario.
- **Adattatore** - Se abbiamo bisogno di fornire una comunicazione tra due classi incompatibili, l'adattatore converte i dati da un tipo all'altro (tipicamente convertendo i tipi di dati nativi di PHP in tipi di dati di database e viceversa).
- **Comando** - Invece di eseguire direttamente un compito specifico (per esempio, inviare un'email), ci ricordiamo solo che doveva accadere. Un altro o lo stesso processo prenderà poi la richiesta e la elaborerà. Il vantaggio principale è che possiamo elaborare l'applicazione in modo pigro (e quindi molto veloce), riprovare azioni fallite e semplicemente memorizzare i parametri della chiamata. Allo stesso tempo, questo ci permetterà di ricevere richieste da diversi clienti, tramite API e così via. Le azioni inviate possono anche essere accodate, prioritarie ed eventualmente cancellate prima della valutazione.
- **Strategia** - Incapsula alcuni tipi di algoritmi o oggetti da modificare in modo che siano intercambiabili per il cliente.

Ci sono molti altri design pattern, questi sono i più importanti che dovreste conoscere.

Anti-pattern
------------

Alcune tecniche di sviluppo della programmazione sono considerate `anti-pattern`, che è l'esatto contrario di un design pattern. Di solito è una tecnica che produce codice strano che non può essere facilmente debuggato, mantenuto e che si comporta `magicamente`.

Un esempio tipico è l'uso di <a href="/global-variable">variabili globali</a>.
