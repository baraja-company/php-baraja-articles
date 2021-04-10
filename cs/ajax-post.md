Zpracování ajaxových POST požadavků v PHP
=========================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 
> publicationDate: "2019-11-01 09:56:02"
> mainCategoryId: "2a1ef8bc-14aa-438a-87e7-5b3f9643f325"

Při vývoji ajaxových Vue.js aplikací jsem po letech konečně definitivně zjistil, jak to s ajaxem v PHP přesně je a jak přijímat data metodou POST.

Superglobální proměnná `$_POST` je dostupná jen pro formuláře
-------------------------------------------------------------

V PHP je běžně dostupná <a href="/superglobalni-promenna">supeglobální promněnná `$_POST`</a>, ve které jsou k dispozici odeslaná data z formuláře.

Použití je **relativně** snadné.

Na straně HTML je potřeba vytvořit formulář:

```html
<form action="process.php" method="post">
    Jméno: <input type="text" name="username">
    <input type="submit" value="Odeslat">
</form>
```

A následně v souboru `process.php` lze k hodnotám přistupovat jako k prvkům pole:

```php
<?php

echo htmlspecialchars($_POST['username'] ?? '');
```

> **Pozor:**
>
> Díky tomuto jednoduchému přístupu by mohl mít každý pocit, že se POSTem odeslaná data automaticky definují jako indexy pole v proměnné `$_POST`. To ale není pravda!

Ve skutečnosti důvod, proč se data odeslaná z formuláře metodou POST propisují do proměnné `$_POST` je ten, že prohlížeč při odeslání HTML formuláře automaticky odesílá HTTP hlavičku `'Content-Type': 'application/x-www-form-urlencoded'`.

Bez správně nastavené hlavičky se k hodnotám jednoduše nedá dostat a musíme použít trikové řešení.

Odeslání dat ajaxem
-------------------

Při pokusech o odesílání dat ajaxem je potřeba na straně PHP trochu změnit přístup. Detaily si můžete <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">přečíst v diskusi na Facebooku</a>.

V javascriptu lze pro odeslání dat ajaxem použít například knihovnu <a href="https://github.com/axios/axios">axios</a>. Pro jednoduché použití stačí nalinkovat javascript z CDN serveru a rovnou použít:

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

V tomto jednoduchém případu se ajaxem zavolá URL `'/api/form-process'` a metodou POST předá objekt `{ username: 'Jméno uživatele' }`. Samotná knihovna už řeší automaticky logistiku předávání dat, takže se odesílají serializované jako Json. V řeči frontend vývojářů se tomu říká **json payload**.

Na straně PHP bych očekával použití jako v případě formuláře (vždyť to bylo metodou POST):

```php
<?php

echo htmlspecialchars($_POST['username'] ?? '');
```

Ve skutečnosti ale v takovém případě bude pole `$_POST` prázdné a nepředají se žádná data. Proměnná `$_POST` slouží pouze pro data získaná z formulářů (pozná se to podle HTTP hlavičky, kterou jsme nevyhodili).

V tomto případě tedy musíme data získat přímo z HTTP requestu, k tomu slouží **trikové řešení**. Použití je pak snadné:

```php
<?php

$data = json_decode(file_get_contents('php://input'), true);

echo htmlspecialchars($data['username'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    'message' => 'Serverová hlíáška',
]);
die;
```

V rámci příkladu uvádím i jednoduchou odpověď pro javascript. Důležité je správně vyhodit HTTP hlavičku `'Content-Type: application/json'` a po odeslání veškerých dat ukončit script.

Vynucení použití `$_POST`
-------------------------

Pokud i přesto chcete s odeslanýmy daty pracovat rovnou jako s formulářem, existuje způsob, jak je přenést. V takovém případě je potřeba upravit vytvoření samotného ajaxového dotazu a správně předat HTTP hlavičky:

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

V takovém případě ale zpracování na straně PHP nebude nic příjemného, protože si musíme data ještě opravit a převést na pole.

Podařilo se mi na to vymyslet tuto hrůzu:

```php
<?php

if (\count($_POST) === 1
    && preg_match('/^\{.*\}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['username'] ?? '');
```

Mnohem lepší je však zůstat u prvního přístupu a data získávat metodou `'php://input'`.
