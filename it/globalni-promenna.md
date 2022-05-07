Variabili globali in PHP
========================

> id: '1cdfc51a-31f4-4a5d-81f6-cfd92f86a9d4'
> slug:
> 	cs: globalni-promenna
> 	it: variabili-globali-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '31e78a9abf19252ef62315230a3731b5'

Le variabili globali sono disponibili in qualsiasi momento in qualsiasi parte dell'applicazione e non hanno bisogno di essere passate.

> **Attenzione:** Un'applicazione ben progettata non dovrebbe usare variabili globali perché violano il principio di incapsulamento e possono causare errori difficili da rilevare se gestiti in modo disattento.

Esempio di utilizzo:

```php
$a = 1;
$b = 2;

function suma(): void
{
	global $a, $b;
	$b = $a + $b;
}

suma();

echo $b; // stampa il numero 3 perché la variabile $b è globale
```

Notate che abbiamo ottenuto le variabili `$a` e `$b` fuori dal loro contesto naturale. Questo comportamento è chiamato "magico" perché se un'altra funzione sovrascrive le variabili attualmente in uso, l'applicazione sperimenterà una condizione inaspettata.

Correttamente, l'applicazione dovrebbe **incapsulare** e passare le variabili ogni volta:

```php
$a = 1;
$b = 2;

function suma(int $a, int $b): int
{
	return $a + $b;
}

echo suma($a, $b); // stampa 3
```

Grazie a questo, possiamo chiamare la funzione dinamicamente con diversi parametri di input e il suo output dipenderà solo dagli input, non dall'ambiente.

Ottenere i parametri di input dall'URL
---------------------------------

Forse l'unico uso ragionevole delle variabili globali è nel parsing dell'input dell'utente, nel qual caso stiamo parlando di <a href="/superglobal-variable">variabili superglobali</a>.

In questo caso, è un design pulito perché la variabile dovrebbe essere di sola lettura, non di sola scrittura, e inoltre è la stessa in tutta l'applicazione:

```php
function getNameFromUrl(): string
{
    return isset($_GET['nome'])
    	? htmlspecialchars($_GET['nome'])
    	: '';
}

echo getNameFromUrl();
```
