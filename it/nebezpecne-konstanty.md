Uso non sicuro delle costanti in PHP
====================================

> id: b4d0f45f-c059-4a47-9b63-8980d0d465ed
> slug:
> 	cs: nebezpecne-konstanty
> 	it: uso-non-sicuro-delle-costanti-in-php
> 
> publicationDate: '2021-06-22 10:49:46'
> mainCategoryId: '561b0614-e152-4a62-a634-1f2d605d39d9'
> sourceContentHash: '58d4ea2e4bf106402386506b023ba4d2'

Ci sono due cose complicate da tenere a mente quando si usano le costanti in PHP.

Costanti dinamiche e statiche
------------------------------

Una costante può essere definita in PHP sia staticamente direttamente nella classe (la soluzione migliore), per esempio come segue:

```php
class Region
{
	public const PREFIX = 420;
}
```

E l'uso è abbastanza chiaro. Al momento della compilazione della classe, il valore della costante è deciso e possiamo accedervi chiamando il nome della classe e la costante stessa. Il più delle volte scrivendo `Region::PREFIX`.

L'altro modo (molto peggiore) è quello di definire la costante dinamicamente a runtime (più spesso da qualche parte in uno script di configurazione), dove è poi qualcosa come:

```php
define('BASE_DIR', __DIR__ . '/../');
```

Lo svantaggio principale di definire una costante tramite la funzione `define` è che lo script che definisce la costante potrebbe non essere stato chiamato, quindi la costante non esisterà quando si cercherà di leggerla.

Se combinato con l'uso di una costante dinamica all'interno della definizione di una statica in una classe, questo può anche risultare in un errore di riflessione fatale:

```php
class InvoiceGenerator
{
	// Questo è completamente sbagliato!
	public const DATA_DIR = BASE_DIR . '/dati/fattura';
}
```

> **Spiegazione:**
>
> L'uso di una costante dinamica all'interno di una costante statica ha il grande svantaggio che il valore della costante dinamica non può essere letto a tempo di compilazione. Questo script deve quindi essere elaborato di nuovo in ogni richiesta (cioè non può essere memorizzato nella OPCache per l'ottimizzazione della velocità), e se la costante non esiste affatto, viene lanciato un errore fatale di compilazione e l'applicazione non può essere eseguita affatto.

Se usi PhpStan, può avvisarti automaticamente di questo problema:

```txt
Reflection error: Could not locate constant "BASE_DIR" while
evaluating expression in InvoiceGenerator at line 6
```

> **Apprendimento:**
>
> Il valore di tutte le costanti dovrebbe essere sempre costante.


Ereditarietà delle costanti quando si usa static
-------------------------------------

In alcuni casi ha senso usare l'ereditarietà per sovrascrivere il valore di una costante. Ma in questo caso, l'antenato non può leggere il valore dal discendente (o non dovrebbe).

Un esempio è quando si definiscono i paesi e le regioni:

```php
abstract class Region
{
	public function getPrefix(): int
	{
		// Errore fatale!
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

Il paradosso è che il codice di cui sopra non lancia necessariamente un errore, ma può essere lanciato da un uso inappropriato dell'ereditarietà.

Se chiamiamo il metodo `getPrefix()` su un discendente di `CzechRepublic`, tutto sarà corretto perché il valore della costante sarà letto correttamente. Tuttavia, se il discendente non ha impostato il valore della costante, verrebbe lanciato un errore fatale di costante inesistente. La parte peggiore di tutta la faccenda è che si tratta di una **dipendenza nascosta** che viene creata nell'implementazione del metodo, e lo sviluppatore che eredita la classe potrebbe anche non sapere di questa dipendenza.

La soluzione migliore in questo caso è definire una costante direttamente nell'antenato con un valore predefinito (in modo che la logica passi sempre), o almeno lanciare un'eccezione nel getter.

```php
abstract class Region
{
	public const REGION = null;

	public function getPrefix(): int
	{
		if (static::REGION === null) {
			throw new \LogicException('La regione non è stata definita.');
		}
		return static::REGION;
	}
}

final class CzechRepublic extends Region
{
	public const REGION = 420;
}
```

PhpStan risponde a questo errore come segue:

```txt
Access to undefined constant static(Region):REGION.
```
