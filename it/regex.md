Espressioni regolari in PHP
===========================

> id: '32142161-6ace-4cfd-b472-b48031219e9b'
> slug:
> 	cs: regex
> 	it: espressioni-regolari-in-php
> 
> perex:
> 	- 'Regulární výrazy a jejich kompletní vysvětlení. Hledání podřetězce, pokročilé nahrazování a generování řetězců.'
> 	- 'Le espressioni regolari e la loro spiegazione completa. Ricerca di sottostringhe, sostituzione avanzata e generazione di stringhe.'
> 
> publicationDate: '2020-03-08 13:37:38'
> mainCategoryId: f1b0be9b-de09-4c8a-8338-dc285bed95ec
> sourceContentHash: '392c8f14e6943dd8f345a9a134e0de92'

Le espressioni regolari sono strumenti che permettono di cercare, convalidare, confrontare, dividere, comprimere e sostituire facilmente le stringhe secondo una maschera (pattern). È uno strumento molto potente ed elegante per la manipolazione avanzata delle stringhe.

Maschera
-----

All'inizio, dobbiamo prima trovare l'espressione regolare effettiva che stiamo per eseguire. Viene inserito come una stringa di testo, che ha un mucchio di regole e opzioni di configurazione (questa è una tecnica molto complessa).

Per cominciare, è importante notare che l'espressione regolare viene valutata in modo sequenziale da sinistra a destra, e se ci sono più modi di interpretare la stringa, viene sempre usata la più grande corrispondenza possibile (si comporta in modo **affamato**, cercando di processare più caratteri possibili).

Il comportamento e la strategia di elaborazione di un'espressione regolare possono essere influenzati da interruttori, che sono molti.

Semplice verifica che la stringa sia un'e-mail valida
-----------------------------------------------

Come possiamo semplicemente controllare che la stringa `jan@barasek.com` sia un indirizzo email valido senza doverla dividere in parti complesse o esaminarla carattere per carattere?

Le espressioni regolari danno la risposta (l'espressione di cui sopra è molto semplificata ai fini dell'esempio, e una vera implementazione della validazione degli indirizzi e-mail dovrebbe essere un po' più complicata):

```php
$mail = 'jan@barasek.com';
$regex = '/^.+@.+.(en|en|com)$/';

if (preg_match($regex, $mail)) {
   echo 'L'e-mail è valida';
} else {
   echo 'L'e-mail non è valida';
}
```

Esaminiamo l'espressione `/^.+@.+\.(en|en|com)$/` in modo più dettagliato:

Per prima cosa, abbiamo bisogno di avvolgere l'intera espressione in una coppia di caratteri `/` (all'inizio e alla fine) che dicono dove inizia e finisce l'espressione. Il `/` alla fine dell'espressione è seguito da qualsiasi modificatore (impostazioni della modalità di elaborazione dell'espressione).

Quando si elabora un'espressione, si procede dal lato sinistro carattere per carattere. Ognuno ha il suo significato, che è riportato nella tabella seguente:

| Carattere | Significato | Descrizione | Esempio |
|------|-----------------|-------|-------------
| `^` | Inizio della stringa | Forza che la stringa deve iniziare in questo punto. | Forza che la stringa deve iniziare con la sequenza `+420` (utile per la validazione dei numeri, per esempio): `/^+420/`. |
| `$` | Fine della stringa o della linea | Forza che la stringa o la linea deve finire qui. La fine della linea è poi sfuggita con ``z``. <a href="https://phpfashion.com/vite-co-znamena-v-regularnim-vyrazu">spiegazione dettagliata</a>. | Il nome del file deve essere un file di testo (che termina con un punto e poi la stringa "txt"): `/\.txt$/`. |
| `.` | Qualsiasi carattere | Cattura assolutamente qualsiasi carattere. | Verifica che la stringa contenga esattamente uno di qualsiasi carattere: `/^.$/`.
| Rileva i caratteri "0-9". Rileva un numero di telefono che non contiene spazi e ha 9 cifre: "^(\420)?\d{9}$/".
| Permette spazi tra le cifre in un numero di telefono in tripla cifra: `/^(\d{3}\s?){3}$/`.
| `+` | Più caratteri, ma almeno uno | Ripete la sottoespressione precedente e cerca di prenderne il più possibile. La sottoespressione deve essere ripetuta almeno una volta. | Cattura quante più cifre possibili, ma almeno una: `/\d+/`. |
| Funziona come `+`, ma permette di catturare una stringa vuota (il valore non deve essere presente). Cattura quante più cifre possibili, se nessuna esiste, cattura una stringa vuota: `/\d*/`.
| `(` `)` | Parentesi graffe | Indica una sottoespressione. Questo può essere usato per racchiudere diversi tag e poi richiedere, per esempio, la ripetizione su di essi, o per intrappolare il loro contenuto in una variabile. | Dividiamo il codice postale in 2 parti secondo lo spazio, che è opzionale e può anche essere più di uno: `/^(\d{3})\s*(\d{2})$/` |
| Numero che inizia con `+420` o `+421`: `/^+(420|421)\s*\d+$/`.
| Se vogliamo catturare un carattere in un'espressione che altrimenti ha un significato speciale, dobbiamo fare l'escape nello stesso modo, per esempio, delle stringhe in PHP. Cattura una coppia di numeri separati da un punto (se non avessimo fatto l'escape del punto, sarebbe stato inteso come "qualsiasi carattere"): `/\d+.\d+/`.

Solo per completezza, darò la forma completa della regola di convalida per le e-mail come implementata da Nette:

```php
/**
 * Trova se una stringa è un indirizzo email valido.
 */
public static function isEmail(string $value): bool
{
   $atom = "[-a-z0-9!#$%&'*+/=?^_`{|}~]"; // RFC 5322 caratteri non quotati nella parte locale
   $alpha = "a-z\x80-\xFF"; // superset di IDN
   return (bool) preg_match("(^
      (\"([ !#-[\\]-~]*|\\\\[ -~])+\"|$atom+(\\.$atom+)*)  # quoted or unquoted
      @
      ([0-9$alpha]([-0-9$alpha]{0,61}[0-9$alpha])?\\.)+    # domain - RFC 1034
      [$alpha]([-0-9$alpha]{0,17}[$alpha])?                # top domain
   \\z)ix", $value);
}
```

`preg_match()` - convalida per modello
------------------------------------

La funzione di base per la validazione e l'analisi del formato è `preg_match()`, ha 2 parametri obbligatori e il terzo può essere usato per specificare il campo di output.

Esempio:

```php
$psc = '272 01'; // Kladno

if (preg_match('/^(\d{3})\s*(\d{2})$/', $psc, $parser)) {
   echo 'Il codice postale è valido [' . $parser[1] . ','. $parser[2] . ']';
} else {
   echo 'Il codice postale non è valido';
}
```

Il codice restituirà: `Codice valido [272, 01]`.

Notate le singole parentesi, che abbiamo usato per dividere l'espressione in diverse parti più piccole. Questo rende poi possibile ottenere le singole sottoespressioni come voci di array. L'intera funzione restituisce poi `true` o `false` a seconda che la stringa sia stata catturata con successo.

A volte, però, navigare nell'ordine numerico delle parentesi è molto impegnativo, poiché il numero può cambiare, o semplicemente ce ne possono essere troppe. In questo caso, è possibile nominare le parentesi individualmente e poi accedere alle chiavi usando i loro nomi.

Per esempio:

```php
$phone = '777 123 456';

preg_match('/^(?<operatore>\d{3})\s*(?<numero>[0-9 ]+)$/', $phone, $parser);

echo $parser['operatore']; // restituito 777
```

`preg_replace()` - sostituzione per modello
----------------------------------------

È anche possibile sostituire le stringhe usando la regex, che è particolarmente utile per varie correzioni di formato post-utente.

Supponiamo di voler memorizzare un numero di telefono inserito dall'utente in un intero, poiché questo è richiesto da una libreria di terze parti, ma gli utenti possono inserirlo in alcuni formati piuttosto selvaggi.

In questo caso, mi attengo al dictum:

> "Sii generoso in quello che ricevi e severo in quello che mandi".

Ecco perché regoliamo automaticamente il formato. Prima usiamo il parsing per rompere la stringa nelle sue singole parti, e poi la ripieghiamo secondo i numeri delle parentesi:

```php
function formatPhoneNumber(string $phoneNumber): int
{
   return (int) preg_replace(
      '/^(\+\d{3})\s*(\d{3})\s*(\d{3})\s*(\d{3})$/',
      '$2$3$4',
      $phoneNumber
   );
}

echo formatPhoneNumber('+420 777 123 456');
```

Costruire una stringa secondo un'espressione regolare
----------------------------------------

I reex hanno anche molto senso quando si generano nuove stringhe secondo un modello complesso.

Il PHP puro non ha supporto per questo, ma possiamo scaricare una libreria di terze parti <a href="https://github.com/icomefromthenet/ReverseRegex">ReverseRegex</a> che può farlo.

Per esempio, potremmo voler generare un insieme di password basate sulla regex `[a-z]{10}` e niente ci fermerà:

```txt
jmceohykoa
aclohnotga
jqegzuklcv
ixdbpbgpkl
kcyrxqqfyw
jcxsjrtrqb
kvaczmawlz
itwrowxfxh
auinmymonl
dujyzuhoag
vaygybwkfm
```

L'uso è il seguente:

```php
use ReverseRegex\Lexer;
use ReverseRegex\Random\SimpleRandom;
use ReverseRegex\Parser;
use ReverseRegex\Generator\Scope;

require 'venditore/autoload.php';

$lexer = new  Lexer('[a-z]{10}');
$gen   = new SimpleRandom(10007);
$result = '';

$parser = new Parser($lexer, new Scope(), new Scope());
$parser->parse()->getResult()->generate($result, $gen);

echo $result;
```

Genero i miei esempi matematici in Nette in Presenter, per esempio, in questo modo, ed è possibile con vera facilità:

```php
public function actionRegex(): void
{
   $lexer = new Lexer('\d{1,3}[\+\-\*\/]');
   $parser = new Parser($lexer, new Scope(), new Scope());
   for ($i = 0; $i <= 10; $i++) {
      $result = '';
      $gen = new SimpleRandom($i);
      $parser->parse()->getResult()->generate($result, $gen);
      dump($result);
   }
   $this->terminate();
}
```

La cosa importante per la libreria è che genera ancora lo stesso output per lo stesso input (anche se potrebbe sembrare che ci siano molte possibili stringhe da abbinare per ogni espressione regolare). Se vogliamo cambiare l'espressione generata in modo casuale, dobbiamo anche cambiare il "seme" con cui viene generata la stringa di output. Per questo è utile attraversare l'intervallo di semi o forse la funzione `rand(1, 1e6)`.

Cattura ed elaborazione degli errori
-----------------------------

In PHP, catturare gli errori nelle regex è un inferno, ma c'è ancora una soluzione.

Questo è spiegato in dettaglio nell'articolo <a href="https://phpfashion.com/zradne-regularni-vyrazy-v-php">Treachable regular expressions in PHP</a> di David Grudel.
