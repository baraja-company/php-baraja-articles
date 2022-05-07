Echo - output al codice sorgente
================================

> id: d6a1a7bf-03bf-49ea-9947-d4d6753ca6e4
> slug:
> 	cs: echo
> 	it: echo---output-al-codice-sorgente
> 
> perex:
> 	- Echo - výpis řetězce do stránky a na standardní výstup. Možnosti escapování a HTTP hlaviček.
> 	- Echo - esegue il dump della stringa sulla pagina e sullo standard output. Opzioni per escaping e intestazioni HTTP.
> 
> publicationDate: '2020-02-16 18:40:31'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: '7d611691825ec537f58f387696d13e28'

Il costrutto `echo` è usato per scaricare una variabile o una stringa nel codice sorgente.

| Supporto: | Tutte le versioni
|----------------|------
| Breve descrizione: | Output di una o più stringhe
| Tipo: | <a href="/comandi-e-funzioni">comando, costruzione</a> (non una funzione)

Descrizione
-----

```php
echo 'Ciao, mondo';
```

Dice "ciao mondo".

```php
$var = 'Testo';
echo $var;
```

Stampa il valore della variabile `$var`, cioè "Text".

**Echo** non è una funzione (è un <a href="/comandi-e-funzioni">comando</a>), quindi potete usare o meno una parentesi. Così, scrivere `echo ('hello world');` è anche corretto.

> **Nota aggiuntiva:** PHP tratta **Echo** come un comando (un costrutto) e quindi lo tratta come un'espressione. La parentesi è facoltativa in questo caso. Se diamo la notazione: `echo ('qualcosa');`, l'istruzione **Echo** non diventa una funzione e non viene trattata come tale. La parentesi in questo caso significa racchiudere il valore esatto dell'espressione, simile a come funziona in matematica.

Virgolette
--------

Le stringhe possono essere racchiuse tra virgolette e apostrofi.

Quindi questo:

```php
echo "Ciao";
```

È lo stesso di questo:

```php
echo 'Ciao';
```

Ma fate attenzione che ogni stringa deve iniziare e finire con lo stesso tipo di carattere di citazione e il carattere di citazione non deve essere usato nella stringa.

Per esempio, se volete emettere un link HTML (o qualsiasi codice HTML), dovete far precedere le virgolette da una barra. Uno slash significa "esattamente questo carattere", quindi non è compreso come un'espressione nella lingua.

```php
echo "<a href="index.php\">link testuale</a>";
```

Nota tecnica: le virgolette hanno un <a href="/quotation-meaning">significato speciale</a> in PHP.

Parametri
---------

- `arg` Parametro di uscita.

Valori di ritorno
-----------------

Non viene restituito alcun valore.

Non può essere usato come variabile.

Nota
--------

Nota: poiché questo è un **costrutto linguistico** (costrutto = comando) (non una funzione), non può essere caricato in una variabile.

Esempio
-------

```php
echo "Ciao, mondo";

echo "echo può emettere più righe di testo.
Ma attenzione al tag HTML <br>, non viene stampato. Ecco a cosa serve la funzione nl2br()".;

$a = "php";				// definizione della variabile

echo "Mi piace" . $a;	// Egli scrive: Mi piace il php
```

**Echo** ha anche una sintassi abbreviata, dove è possibile usare solo il segno di uguale dopo il tag php di apertura.

```php
Ahoj <?=$jmeno;?>!
```

Questo è utile se abbiamo bisogno di scrivere qualche informazione veloce nella pagina. Per esempio, l'anno corrente:

```php
Píše Jan Barášek © <?=date('Y');?>
```

> Questa sintassi abbreviata funziona solo se i tag di apertura abbreviati in php sono abilitati, cioè la direttiva `short_open_tag` è impostata su `on`.

Operazione
-------

Tutte le operazioni matematiche comuni possono essere eseguite all'interno del comando **echo**.

Per una discussione dettagliata della matematica, vedere <a href="/matematica">un articolo separato</a>.

```php
echo 5 + 3 * 2; // stampa 11
```
