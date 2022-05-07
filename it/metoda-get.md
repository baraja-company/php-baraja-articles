Ottenere parametri dall'URL con il metodo GET
=============================================

> id: bbf2cb2c-f7f7-4be9-a9cd-960014db0f51
> slug:
> 	cs: metoda-get
> 	it: ottenere-parametri-dall-url-con-il-metodo-get
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: caebe20b6b7cdf0b3703ff55dad6f094

Sai, hai una pagina aperta, segui l'URL e vedi un punto di domanda con alcuni parametri. Un programmatore inesperto penserebbe che si tratta di file separati, ma ecco. Provate a creare un file che abbia un punto interrogativo nel suo nome (non funziona). **Questo è il motivo per cui è stato scritto questo articolo**.

Che cos'è?
--------------------------

In realtà, il punto è che si tratta di un singolo file a cui si passano variabili tramite un URL, quindi ho, diciamo, un file **index.php**, e gli passo il nome dell'articolo: **index.php?clanek=o-php**.

Codice + spiegazione
--------------------------

La variabile superglobale `$_GET` contiene chiavi con parametri dall'URL

```php
echo $_GET['Articolo'] ?? '';
```

Limiti di sicurezza e di lunghezza
--------------------------

Il metodo GET non è sicuro, quindi i dati confidenziali non dovrebbero essere inviati su di esso, una delle ragioni principali è che si tratta di una comunicazione non criptata e in secondo luogo viene memorizzata nella storia.

I dati confidenziali o semplicemente tutto dovrebbe essere inviato usando il metodo <a href="/method-post">POST</a>. GET è più adatto per i furmulari dove è bene mostrare dei parametri (come i motori di ricerca, la pagina dell'articolo) in modo che la pagina possa essere collegata.

La lunghezza del GET non è illimitata! Molti principianti pagano per questo. La lunghezza massima è di circa 1024 caratteri (alcuni posti dicono 1088). Quindi, per testi più lunghi, inviate <a href="/metodo-post">POST</a> con.
