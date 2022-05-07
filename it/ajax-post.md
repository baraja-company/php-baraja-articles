Elaborare richieste ajax POST in PHP
====================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	it: elaborare-richieste-ajax-post-in-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

Durante lo sviluppo di applicazioni ajax Vue.js, dopo anni ho finalmente scoperto come usare ajax in PHP e come ricevere dati con il metodo POST.

La variabile superglobale `$_POST` è disponibile solo per i moduli
-------------------------------------------------------------

In PHP, la <a href="/superglobal-variable">variabile superglobale `$_POST`</a> è comunemente disponibile per contenere i dati inviati da un modulo.

È **relativamente** facile da usare.

Sul lato HTML, è necessario creare un modulo:

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

E poi nel file `process.php` i valori possono essere acceduti come elementi dell'array:

```php
echo htmlspecialchars($_POST['nome utente'] ?? '');
```

> **Attenzione:**
>
> Con questo semplice approccio, tutti potrebbero pensare che i dati POSTed siano automaticamente definiti come indici di array nella variabile `$_POST`. Ma questo non è vero!

Infatti, la ragione per cui i dati inviati da un modulo usando il metodo POST sono scritti nella variabile `$_POST` è perché il browser invia automaticamente l'intestazione HTTP `'Content-Type': 'application/x-www-form-urlencoded'` quando il modulo HTML viene inviato.

Senza un'intestazione correttamente impostata, i valori semplicemente non sono accessibili e dobbiamo usare una soluzione a trabocchetto.

Invio di dati tramite ajax
-------------------

Quando si cerca di inviare dati usando ajax, dobbiamo cambiare un po' l'approccio sul lato PHP. Potete <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">leggere la discussione su Facebook</a> per i dettagli.

In javascript, per esempio, potete usare la libreria <a href="https://github.com/axios/axios">axios</a> per inviare dati usando ajax. Per usarlo facilmente, basta collegare il javascript dal server CDN e usarlo subito:

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    axios.post('/api/form-process', {
        username: 'Jméno uživatele'
    })
    .then(response => {
        // Zpracování odpovědi z API
        alert(response.data.message); // Vyhodí hlášku se zprávou
    });
</script>
```

In questo semplice caso, l'URL `'/api/form-process'` viene chiamato usando ajax e il metodo POST passa l'oggetto `{ username: 'User name' }`. La libreria stessa gestisce già la logistica del passaggio dei dati automaticamente, quindi vengono inviati serializzati come Json. Nel linguaggio degli sviluppatori frontend, questo si chiama **json payload**.

Dal lato PHP, mi aspetterei di usarlo come un modulo (dopo tutto, era un metodo POST):

```php
echo htmlspecialchars($_POST['nome utente'] ?? '');
```

Tuttavia, in questo caso il campo `$_POST` sarà vuoto e nessun dato verrà passato. La variabile `$_POST` è usata solo per i dati recuperati dai form (si capisce dall'intestazione HTTP che non abbiamo buttato via).

Quindi in questo caso abbiamo bisogno di ottenere i dati direttamente dalla richiesta HTTP, la soluzione **trick** è usata per questo. È quindi facile da usare:

```php
$data = json_decode(file_get_contents('php://input'), true);

echo htmlspecialchars($data['nome utente'] ?? '');

header('Tipo di contenuto: application/json');
echo json_encode([
    'messaggio' => 'Il server creeper',
]);
die;
```

Come esempio, fornisco anche una semplice risposta in javascript. L'importante è lanciare correttamente l'intestazione HTTP `'Content-Type: application/json'` e uscire dallo script dopo che tutti i dati sono stati inviati.

Imporre l'uso di `$_POST
-------------------------

Se volete ancora trattare i dati inviati direttamente come un modulo, c'è un modo per trasferirli. In questo caso, è necessario modificare la creazione della query ajax stessa e passare correttamente le intestazioni HTTP:

```js
axios.post(
    '/api/form-process',
    {
        username: 'Jméno uživatele'
    },
    {
        headers: {'Content-Type': 'application/x-www-form-urlencoded'}
    }
).then(response => {
    // Nějaké zpracování
});
```

In questo caso, però, l'elaborazione sul lato PHP non sarà piacevole, perché dobbiamo ancora correggere i dati e convertirli in un array.

Sono riuscito a trovare questo orrore:

```php
if (\count($_POST) === 1
    && preg_match('/^\{.*\}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['nome utente'] ?? '');
```

Tuttavia, è molto meglio attenersi al primo approccio e usare il metodo `'php://input'` per ottenere i dati.
