Cosa fare quando l'anno prossimo l'intelligenza artificiale toglierà il lavoro ai programmatori?
================================================================================================

> id: '0eccb5c1-9ab4-42bf-b27e-13917d616b7f'
> slug:
> 	- null
> 	it: cosa-fare-quando-l-anno-prossimo-l-intelligenza-artificiale-togliera-il-lavoro-ai-programmatori
> 
> cs: openai-php-kod
> publicationDate: '2022-12-23 13:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '573e4173dd9e7a2e7068c404fecf9b72'

Le possibilità del modello linguistico OpeanAI sono incredibili. Basta inserire il codice sorgente, chiederne l'analisi, la correzione e la riscrittura in un'altra lingua, e tutto avviene automaticamente.

Quando l'ho provato per la prima volta, sono rimasto scioccato da ciò che si può ottenere e da quanto siano potenti gli strumenti a disposizione del pubblico.

Vediamo un esempio:

**Domanda:** Individuare la vulnerabilità di sicurezza in questa funzione e fornire il codice corretto:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Gennaio',
		'Febbraio',
		'Marzo',
		'Aprile',
		'Maggio',
		'Giugno',
		'Luglio',
		'Agosto',
		'Settembre',
		'Ottobre',
		'Novembre',
		'Dicembre',
	];

	return $czMonths[(int) date('m')];
}
```

**Risposta:**

Una delle vulnerabilità di sicurezza di questa funzione è il pericolo di sfruttare l'attacco SQL injection. Quando un input mal formattato o dannoso viene passato alla funzione, il database può essere compromesso o sfruttato.

Una soluzione consiste nell'utilizzare costanti di mese predefinite invece di input ottenuti da una chiamata alla funzione date(). Possiamo anche utilizzare la funzione filter_var() con un filtro appropriato per garantire che l'input sia numerico e non contenga codice dannoso.

Ecco il codice corretto:

```php
function getThisMonthName(): string
{
	$czMonths = [
		'',
		'Gennaio',
		'Febbraio',
		'Marzo',
		'Aprile',
		'Maggio',
		'Giugno',
		'Luglio',
		'Agosto',
		'Settembre',
		'Ottobre',
		'Novembre',
		'Dicembre',
	];
	$month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if ($month === false || $month < 1 || $month > 12) {
		return "";
	}

	return $czMonths[$month];
}
```

**Domanda:** Convertire l'implementazione della funzione in dattiloscritto.

**Risposta:**

Ecco l'implementazione riscritta della funzione in TypeScript:

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = filter_var((int) date('m'), FILTER_VALIDATE_INT);

	if (month === false || month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

**Richiesta:** Questo non è un dattiloscritto valido!

**Risposta:**

Mi scuso per l'errore. Ecco la versione corretta della funzione in TypeScript:

```js
function getThisMonthName(): string {
	const czMonths = [
	'',
	'Leden',
	'Únor',
	'Březen',
	'Duben',
	'Květen',
	'Červen',
	'Červenec',
	'Srpen',
	'Září',
	'Říjen',
	'Listopad',
	'Prosinec',
	];

	const month = Number(new Date().getMonth()) + 1;

	if (month < 1 || month > 12) {
		return "";
	}

	return czMonths[month];
}
```

Nota: non esiste una funzione filter_var() in TypeScript, quindi l'ho sostituita richiamando la funzione getMonth() dalla classe Date e poi convertendola in numero con la funzione Number(). Questa versione della funzione dovrebbe essere valida in TypeScript.

**Domanda:** La vostra implementazione di Typescript contiene una vulnerabilità di sicurezza?

**Risposta:**

La mia implementazione corretta della funzione in TypeScript non dovrebbe contenere la vulnerabilità di sicurezza dell'attacco SQL injection presente nell'implementazione originale in PHP. La funzione getMonth() della classe Date restituisce un valore numerico del mese (0 per gennaio, 1 per febbraio, ecc.), quindi non abbiamo bisogno di passare l'input dalla funzione date() e possiamo evitare il pericolo dell'attacco SQL injection.

Tuttavia, qualsiasi applicazione può contenere vulnerabilità di sicurezza, quindi è importante esaminare periodicamente il codice per individuare potenziali problemi di sicurezza. Se si sospetta una vulnerabilità di sicurezza nel codice, è possibile contattare esperti di sicurezza o utilizzare strumenti di scansione automatica delle vulnerabilità di sicurezza.
