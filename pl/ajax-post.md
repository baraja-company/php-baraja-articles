Przetwarzanie żądań ajax POST w PHP
===================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	pl: przetwarzanie-zadan-ajax-post-w-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

Podczas tworzenia aplikacji ajaxowych Vue.js, po latach w końcu dowiedziałem się, jak używać ajaxa w PHP i jak odbierać dane metodą POST.

Zmienna superglobalna `$_POST` jest dostępna tylko dla formularzy
-------------------------------------------------------------

W PHP, <a href="/superglobal-variable">zmienna superglobalna `$_POST`</a> jest powszechnie dostępna do przechowywania danych przesłanych z formularza.

Jest on **względnie** łatwy w użyciu.

Po stronie HTML należy utworzyć formularz:

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

Następnie w pliku `process.php` wartości te mogą być dostępne jako elementy tablicy:

```php
echo htmlspecialchars($_POST['nazwa użytkownika'] ?? '');
```

> **Ostrzeżenie:**
>
> Dzięki temu prostemu podejściu każdy może odnieść wrażenie, że POSTowane dane są automatycznie definiowane jako indeksy tablicy w zmiennej `$_POST`. Ale to nie jest prawda!

W rzeczywistości powodem, dla którego dane wysłane z formularza przy użyciu metody POST są zapisywane do zmiennej `$_POST`, jest to, że przeglądarka automatycznie wysyła nagłówek HTTP `'Content-Type': 'application/x-www-form-urlencoded'`, gdy formularz HTML jest wysyłany.

Bez odpowiednio ustawionego nagłówka nie można uzyskać dostępu do wartości i trzeba zastosować inne rozwiązanie.

Wysyłanie danych przez ajax
-------------------

Gdy próbujemy wysyłać dane za pomocą ajaxa, musimy nieco zmienić podejście po stronie PHP. Możesz <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">przeczytać dyskusję na Facebooku</a>, aby poznać szczegóły.

W języku javascript można na przykład użyć biblioteki <a href="https://github.com/axios/axios">axios</a> do wysyłania danych przy użyciu technologii ajax. Aby łatwo z niego korzystać, wystarczy połączyć javascript z serwera CDN i od razu go używać:

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

W tym prostym przypadku adres URL `'/api/form-process'` jest wywoływany za pomocą ajaxa, a metoda POST przekazuje obiekt `{ username: 'User name' }`. Sama biblioteka zajmuje się już logistyką automatycznego przekazywania danych, więc są one przesyłane w postaci zserializowanej jako Json. W języku programistów frontendowych nazywa się to **json payload**.

Po stronie PHP spodziewałbym się użyć go jako formularza (w końcu była to metoda POST):

```php
echo htmlspecialchars($_POST['nazwa użytkownika'] ?? '');
```

Jednak w tym przypadku pole `$_POST` będzie puste i żadne dane nie zostaną przekazane. Zmienna `$_POST` jest używana tylko dla danych pobieranych z formularzy (można to poznać po nagłówku HTTP, którego nie wyrzuciliśmy).

W tym przypadku musimy więc pobrać dane bezpośrednio z żądania HTTP, do czego służy **trick solution**. Jest wtedy łatwy w użyciu:

```php
$data = json_decode(file_get_contents('php://wejście'), true);

echo htmlspecialchars($data['nazwa użytkownika'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    'wiadomość' => 'Pnącze serwera',
]);
die;
```

Jako przykład podaję również prostą odpowiedź w języku javascript. Ważne jest, aby poprawnie wyrzucić nagłówek HTTP `'Content-Type: application/json'` i zakończyć działanie skryptu po wysłaniu wszystkich danych.

Wymuszenie użycia `$_POST`.
-------------------------

Jeśli nadal chcesz traktować przesłane dane bezpośrednio jako formularz, istnieje sposób na ich przeniesienie. W takim przypadku należy zmodyfikować sposób tworzenia samego zapytania ajaxowego i poprawnie przekazać nagłówki HTTP:

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

W tym przypadku jednak przetwarzanie danych po stronie PHP nie będzie przyjemne, ponieważ musimy jeszcze poprawić dane i przekonwertować je na tablicę.

Udało mi się wymyślić ten horror:

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

echo htmlspecialchars($_POST['nazwa użytkownika'] ?? '');
```

Jednak o wiele lepiej jest trzymać się pierwszego podejścia i użyć metody `'php://input'` do uzyskania danych.
