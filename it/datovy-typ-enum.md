Tipo di dati oggetto Enum in PHP
================================

> id: '5b96765c-16c2-4f76-8e31-6de6e9f59365'
> slug:
> 	cs: datovy-typ-enum
> 	de: type-php
>
> publicationDate: '2022-05-29 15:30:00'
> mainCategoryId: e45491db-b548-471d-97b4-3e23610c5da9
> sourceContentHash: '43735e926db87d2cc6c538ddb67e0a69'

A partire da PHP 8.1, il tipo di dati Enum può essere usato per definire valori enumerativi esatti per un elenco. È utile nei casi in cui sappiamo che il valore di una variabile può assumere solo alcuni valori specifici.

Ad esempio, ecco come memorizzo i tipi di notifica:

```php
enum OrderNotificationType: string
{
    case Email = 'e-mail';
    case Sms = 'testo';
}
```

In PHP, il tipo di dati Enum è un oggetto classico che si comporta come un tipo speciale di costante, ma ha anche un'istanza che può essere passata. Tuttavia, a differenza di un oggetto normale, è soggetto a una serie di restrizioni.

Differenze tra Enum e oggetti
-----------------------

Sebbene gli enum siano costruiti sopra le classi e gli oggetti, non supportano tutte le funzionalità legate agli oggetti. In particolare, agli oggetti enum è vietato avere uno stato interno (devono sempre essere una classe statica).

Un elenco specifico di differenze:

- I costruttori e i distruttori sono vietati.
- L'ereditarietà non è supportata. Gli enum non possono essere estesi o ereditati da un'altra classe.
- Non sono ammesse proprietà statiche o di oggetti.
- La clonazione di valori specifici di Enum (istanze) non è supportata; ogni singola istanza deve essere un'istanza singola.
- I metodi magici, ad eccezione di quanto indicato di seguito, sono vietati.

Le seguenti caratteristiche dell'oggetto sono disponibili e si comportano come qualsiasi altro oggetto:

- Metodi pubblici, privati e protetti.
- Metodi statici pubblici, privati e protetti.
- Costanti pubbliche, private e protette.
- Gli enum possono implementare un numero qualsiasi di interfacce.
- Gli attributi possono essere collegati a enum e casi. Il filtro di destinazione `TARGET_CLASS` include gli Enum stessi. Il filtro di destinazione `TARGET_CLASS_CONST` include i casi Enum.
- Metodi magici `__call`, `__callStatic` e `__invoke`.
- Le costanti `__CLASS__` e `__FUNCTION__` si comportano come le normali costanti
- La costante magica `::class` sul tipo Enum viene valutata come il nome completo del tipo di dati, incluso qualsiasi spazio dei nomi, esattamente come per un oggetto. La costante magica `::class` su un'istanza di tipo `Case` viene valutata anche come tipo Enum, poiché è un'istanza di un tipo diverso.

Utilizzo di Enum come tipo di dati
-----------------------------

Immaginiamo di avere un enum che rappresenta i tipi di abiti. In questo caso, è sufficiente definire il tipo `Suit` e memorizzare i singoli valori validi.

Si ottiene quindi un'istanza dell'opzione particolare in modo classico attraverso una quadratica, come quando si lavora con una costante statica.

Esempio di definizione di un Enum, richiamo di un tipo specifico e passaggio a una funzione:

```php
enum Suit
{
	case Hearts;
	case Diamonds;
	case Clubs;
	case Spades;
}

function doStuff(Suit $s)
{
	// ...
}

doStuff(Suit::Spades);
```

Confronto tra due valori
---------------------

Il vantaggio fondamentale degli enum rispetto agli oggetti e alle costanti è che è facile confrontare i loro valori.

Il confronto di base che si fa con un valore specifico può essere fatto come segue:

```php
$a = Suit::Spades;
$b = Suit::Spades;

$a === $b; // vero
```

Molto spesso dobbiamo anche decidere se un particolare valore appartiene a un'enumerazione di valori Enum valida. Ciò può essere facilmente verificato come segue:

```php
$a = Suit::Spades;

$a instanceof Suit;  // vero
```

Lettura del valore del tipo
---------------------

Possiamo leggere un valore di tipo specifico sia come nome di una costante chiamante sia direttamente come valore reale definito (se esiste):

```php
enum Colors
{
	case Red;
	case Blue;
	case Green;

	public function getColor(): string
	{
	    return $this->name;
	}
}

function paintColor(Colors $colors): void
{
	echo "Vernice:" . $colors->getColor();
}
```

Il valore della costante chiamante viene letto tramite la proprietà `name`. È inoltre importante che una funzione personalizzata possa essere implementata direttamente nel tipo di dati Enum, che può essere richiamato su ciascun Enum.

Se un particolare Enum implementa anche valori reali (che sono nascosti sotto ogni costante), è possibile leggere anche il loro valore:

```php
enum OrderNotificationType: string
{
    case Email = 'e-mail';
    case Sms = 'testo';
}

$type = OrderNotificationType::Email;

echo $type->value;
```

Tutti i valori Enum validi
-----------------------------

Spesso è necessario elencare (ad esempio, all'utente in un messaggio di errore) tutti i possibili valori che Enum può assumere. Quando si usavano le costanti questo non era possibile, mentre Enum lo permette facilmente:

```php
Suit::cases();
```

Restituisce `[Suit::Hearts, Suit::Diamonds, Suit::Clubs, Suit::Spades]`.

Verificare che la variabile sia di tipo Enum
---------------------------------

Possiamo facilmente verificare che una particolare variabile sconosciuta contenga Enum tramite una condizione:

```php
if ($haystack instanceof \BackedEnum) {
```

Ogni oggetto Enum è automaticamente un figlio dell'interfaccia generica ``BackedEnum''.

Per ulteriori informazioni, vedere la discussione su PhpStan GitHub: https://github.com/phpstan/phpstan/issues/7304
