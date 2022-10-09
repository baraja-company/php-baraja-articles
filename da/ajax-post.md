Behandling af ajax POST-forespørgsler i PHP
===========================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	da: behandling-af-ajax-post-foresporgsler-i-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

Da jeg udviklede ajax Vue.js applikationer, fandt jeg efter flere år endelig ud af hvordan man bruger ajax i PHP og hvordan man modtager data ved hjælp af POST-metoden.

Den superglobale variabel `$_POST` er kun tilgængelig for formularer
-------------------------------------------------------------

I PHP er <a href="/superglobal-variabel">superglobalvariablen `$_POST`</a> almindeligvis tilgængelig til at opbevare de indsendte data fra en formular.

Det er **relativt** nemt at bruge.

På HTML-siden skal du oprette en formular:

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

Og i filen `process.php` kan værdierne så tilgås som array-elementer:

```php
echo htmlspecialchars($_POST['brugernavn'] ?? '');
```

> **Varsling:**
>
> Med denne enkle fremgangsmåde vil alle måske føle, at POSTed data automatisk defineres som array-indekser i variablen `$_POST`. Men det er ikke sandt!

Grunden til, at data, der sendes fra en formular med POST-metoden, skrives til variablen `$_POST`, er faktisk, at browseren automatisk sender HTTP-headeren `'Content-Type': 'application/x-www-form-urlencoded'`, når HTML-formularen sendes.

Uden en korrekt indstillet header kan værdierne simpelthen ikke tilgås, og vi er nødt til at bruge en trickløsning.

Afsendelse af data via ajax
-------------------

Når vi forsøger at sende data ved hjælp af ajax, skal vi ændre fremgangsmåden en smule på PHP-siden. Du kan <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">læse diskussionen på Facebook</a> for at få flere oplysninger.

I javascript kan du f.eks. bruge <a href="https://github.com/axios/axios">axios</a>-biblioteket til at sende data ved hjælp af ajax. Du kan nemt bruge det ved at linke javascript fra CDN-serveren og bruge det med det samme:

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

I dette enkle tilfælde kaldes URL-adressen `'/api/form-process'` ved hjælp af ajax, og POST-metoden overfører objektet `{ brugernavn: 'Brugernavn' }`. Biblioteket selv håndterer allerede logistikken i forbindelse med overførslen af dataene automatisk, så de sendes serialiseret som Json. På frontend-udviklersprog kaldes dette **json payload**.

På PHP-siden ville jeg forvente at bruge det som en formular (det var trods alt en POST-metode):

```php
echo htmlspecialchars($_POST['brugernavn'] ?? '');
```

I dette tilfælde vil feltet `$_POST` dog være tomt, og der vil ikke blive overført data. Variablen `$_POST` bruges kun til data, der hentes fra formularer (det kan du se på HTTP-headeren, som vi ikke smed væk).

Så i dette tilfælde skal vi hente dataene direkte fra HTTP-forespørgslen, og derfor bruges **trick-løsningen** til det. Den er derefter nem at bruge:

```php
$data = json_decode(file_get_contents('php://input'), true);

echo htmlspecialchars($data['brugernavn'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    'besked' => 'Serveren krybende',
]);
die;
```

Som et eksempel giver jeg også et simpelt javascript-svar. Det vigtige er at angive HTTP-headeren `'Content-Type: application/json'` korrekt og afslutte scriptet, når alle data er blevet sendt.

Håndhævelse af brugen af `$_POST`
-------------------------

Hvis du stadig ønsker at behandle de indsendte data direkte som en formular, er der en måde at overføre dem på. I dette tilfælde skal du ændre oprettelsen af selve ajax-forespørgslen og sende HTTP-headerne korrekt:

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

I dette tilfælde vil behandlingen på PHP-siden dog ikke være behagelig, fordi vi stadig skal rette dataene og konvertere dem til et array.

Det lykkedes mig at finde på denne gyser:

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

echo htmlspecialchars($_POST['brugernavn'] ?? '');
```

Det er dog langt bedre at holde sig til den første metode og bruge metoden `'php://input'` til at hente data.
