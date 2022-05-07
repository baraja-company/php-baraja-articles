Moduli, elaborazione di moduli in PHP
=====================================

> id: d1871a8d-bcfe-408d-ac12-6b827633f77e
> slug:
> 	cs: formulare-2
> 	it: moduli-elaborazione-di-moduli-in-php
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: a23332c0-a233-4093-abd7-85b1b00a383b
> sourceContentHash: '91f43ee06328e88a6a1058ecbf028222'

Supponiamo di aver creato un modulo HTML, che inviamo e ora vogliamo elaborare i dati. C'è un <a href="/formulare">articolo separato</a> sulla creazione di un modulo HTML.

Ricevere dati - modi diversi
----------------------------

Il modo in cui il modulo viene inviato è impostato direttamente nell'HTML

Ci sono 2 opzioni:

- **GET** - È visibile nella barra degli indirizzi dopo il punto interrogativo
 Per esempio: `php.baraja.cz/search.php?query=formulare`.
- **POST** - Nascosto (non visibile), la maggior parte dei moduli sono inviati per posta

Dobbiamo poi leggerli in PHP usando lo stesso metodo.

Ottenere i dati dall'utente e trasferirli allo script
------------------------------------------------------

La base è un modulo HTML, come farlo si può leggere <a href="/formulare">in un articolo separato</a>.

Per cominciare, supponiamo un semplice modulo per inserire il nome dell'utente:

```html
<form action="welcome.php" method="GET">
   Zadejte jméno: <input type="text" name="username">
   <input type="submit" value="odeslat">
</form>
```

Apparirà una casella di testo per inserire un nome e cliccare per inviare. Quando il pulsante viene cliccato, il contenuto del campo viene inviato allo script `welcome.php`.

Ora per l'elaborazione effettiva nel file `welcome.php`:

```php
$username = $_GET['nome utente'];

echo 'Il nome inserito è:' . $username;
```

Notate la variabile speciale `$_GET`. Questa è una **variabile sovraglobale** che contiene i dati del modulo e a cui si può accedere come un array.

Il problema con questa soluzione, tuttavia, è che i dati ricevuti non sono **sicuri** e un modulo simile può essere facilmente attaccato. Per esempio, un potenziale attaccante può inserire del codice javascript nel campo invece di un nome, che sarà scritto nella pagina ed eseguito.

Pertanto, dobbiamo sempre sanitizzare tutti i dati dell'utente prima di emetterli nel codice HTML:

```php
$username = $_GET['nome utente'] ?? 'Sconosciuto';

echo 'Il nome inserito è:' . htmlspecialchars($username);
```

Ulteriore elaborazione
----------------

Possiamo fare qualsiasi cosa con i dati ricevuti e trattarli come qualsiasi variabile ordinaria.

Per esempio, aggiungete il valore in due campi:

```php
echo $_GET['x'] + $_GET['y'];
```

O salvare in un file, database, e-mail, ...

Le seguenti funzioni sono utili per questo:

- <a href="/file-put-contents">file_put_contents</a> - funzione per salvare i dati in un file
- <a href="/hashovani">MD5</a> - calcolo del checksum, per esempio per le password
- <a href="/cookies">Cookies</a> - salva i dati nei **cookies** (piccoli file all'interno del browser web)
