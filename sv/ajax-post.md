Behandling av ajax POST-förfrågningar i PHP
===========================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	sv: behandling-av-ajax-post-foerfragningar-i-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

När jag utvecklade ajax Vue.js-applikationer kom jag efter flera år äntligen på hur man använder ajax i PHP och hur man tar emot data med POST-metoden.

Den superglobala variabeln `$_POST` är endast tillgänglig för formulär.
-------------------------------------------------------------

I PHP är <a href="/superglobal-variabel">superglobalvariabeln `$_POST`</a> allmänt tillgänglig för att lagra de skickade uppgifterna från ett formulär.

Det är **relativt** lätt att använda.

På HTML-sidan måste du skapa ett formulär:

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

I filen `process.php` kan värdena sedan nås som array-element:

```php
echo htmlspecialchars($_POST['användarnamn'] ?? '');
```

> **Varning:**
>
> Med detta enkla tillvägagångssätt kan alla känna att POSTed data automatiskt definieras som arrayindex i variabeln `$_POST`. Men det är inte sant!

Anledningen till att data som skickas från ett formulär med POST-metoden skrivs till variabeln `$_POST` är att webbläsaren automatiskt skickar HTTP-huvudet `'Content-Type': 'application/x-www-form-urlencoded'` när HTML-formuläret skickas.

Utan en korrekt inställd rubrik kan värdena helt enkelt inte nås och vi måste använda en knepig lösning.

Skicka data via Ajax
-------------------

När vi försöker skicka data med hjälp av Ajax måste vi ändra vår strategi lite på PHP-sidan. Du kan <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">läsa diskussionen på Facebook</a> för mer information.

I javascript kan du till exempel använda biblioteket <a href="https://github.com/axios/axios">axios</a> för att skicka data med hjälp av ajax. För att använda den enkelt kan du bara länka javascript från CDN-servern och använda den direkt:

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

I det här enkla fallet anropas URL-adressen `'/api/form-process'` med ajax och POST-metoden skickar objektet `{ username: 'User name' }`. Biblioteket självt hanterar redan logistiken för att skicka data automatiskt, så de skickas serialiserade som Json. På frontendutvecklarspråk kallas detta **json payload**.

På PHP-sidan skulle jag förvänta mig att det används som ett formulär (det var ju trots allt en POST-metod):

```php
echo htmlspecialchars($_POST['användarnamn'] ?? '');
```

I det här fallet kommer dock fältet `$_POST` att vara tomt och inga data kommer att skickas. Variabeln `$_POST` används endast för data som hämtas från formulär (du kan se det på HTTP-huvudet som vi inte slängde).

I det här fallet måste vi alltså hämta data direkt från HTTP-förfrågan, och för det används **trick-lösningen**. Den är sedan lätt att använda:

```php
$data = json_decode(file_get_contents('php://input'), true);

echo htmlspecialchars($data['användarnamn'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    'meddelande' => 'Serverkryparen',
]);
die;
```

Som exempel ger jag också ett enkelt javascript-svar. Det viktiga är att HTTP-huvudet `'Content-Type: application/json'` slängs korrekt och att skriptet avslutas efter att all data har skickats.

Genomdriva användningen av `$_POST`.
-------------------------

Om du fortfarande vill behandla de inlämnade uppgifterna direkt som ett formulär finns det ett sätt att överföra dem. I det här fallet måste du ändra skapandet av själva ajax-frågan och skicka HTTP-huvudena korrekt:

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

I det här fallet blir dock bearbetningen på PHP-sidan inte trevlig, eftersom vi fortfarande måste korrigera uppgifterna och konvertera dem till en array.

Jag lyckades komma på denna skräck:

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

echo htmlspecialchars($_POST['användarnamn'] ?? '');
```

Det är dock mycket bättre att hålla sig till det första tillvägagångssättet och använda metoden `'php://input'` för att hämta data.
