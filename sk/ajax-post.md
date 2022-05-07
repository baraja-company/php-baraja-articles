Spracovanie požiadaviek ajax POST v jazyku PHP
==============================================

> id: c9c8fdb4-020d-4361-b425-4f4406a090ba
> slug:
> 	cs: ajax-post
> 	sk: spracovanie-poziadaviek-ajax-post-v-jazyku-php
> 
> publicationDate: '2019-11-01 09:56:02'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '89254da0b9b22b1926410b3ef7e511ec'

Pri vývoji ajaxových aplikácií Vue.js som po rokoch konečne zistil, ako používať ajax v PHP a ako prijímať údaje metódou POST.

Superglobálna premenná `$_POST` je k dispozícii len pre formuláre
-------------------------------------------------------------

V PHP je bežne k dispozícii <a href="/superglobal-variable">superglobálna premenná `$_POST`</a>, ktorá uchováva odoslané údaje z formulára.

Jeho používanie je **relatívne** jednoduché.

Na strane HTML je potrebné vytvoriť formulár:

```html
<form action="process.php" method="post">
    Meno: <input type="text" name="užívateľské meno">
    <input type="submit" value="Odoslať">
</form>
```

A potom v súbore `process.php` je možné k hodnotám pristupovať ako k prvkom poľa:

```php
<?php

echo htmlspecialchars($_POST['username'] ?? '');
```

> **Upozornenie:**
>
> Pri tomto jednoduchom prístupe môže mať niekto pocit, že údaje odoslané prostredníctvom POST sú automaticky definované ako indexy poľa v premennej `$_POST`. Ale to nie je pravda!

Dôvodom, prečo sa údaje odoslané z formulára pomocou metódy POST zapisujú do premennej `$_POST`, je skutočnosť, že prehliadač pri odosielaní formulára HTML automaticky odosiela hlavičku HTTP `'Content-Type': 'application/x-www-form-urlencoded'`.

Bez správne nastavenej hlavičky sa k hodnotám jednoducho nedá dostať a musíme použiť trikové riešenie.

Odosielanie údajov pomocou ajaxu
-------------------

Pri pokuse o odosielanie údajov pomocou ajaxu musíme na strane PHP trochu zmeniť prístup. Podrobnosti si môžete <a href="https://www.facebook.com/groups/frontendisti/permalink/2372671669611010/">prečítať v diskusii na Facebooku</a>.

V javascripte môžete napríklad použiť knižnicu <a href="https://github.com/axios/axios">axios</a> na odosielanie údajov pomocou ajaxu. Ak ho chcete jednoducho používať, stačí prepojiť javascript zo servera CDN a hneď ho používať:

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script>
    axios.post('/api/form-process', {
        užívateľské meno: 'user name'
    })
    .then(response => {
        // Spracovanie odpovede z rozhrania API
        alert(response.data.message); // Vyhodí správu
    });
</script>
```

V tomto jednoduchom prípade sa URL adresa `'/api/form-process'` zavolá pomocou ajaxu a metóda POST odovzdá objekt `{ meno používateľa: 'Meno používateľa' }`. Samotná knižnica sa už o logistiku odovzdávania údajov stará automaticky, takže sa odosielajú serializované ako Json. V reči frontendových vývojárov sa to nazýva **json payload**.

Na strane PHP by som očakával, že sa použije ako formulár (koniec koncov, bola to metóda POST):

```php
<?php

echo htmlspecialchars($_POST['username'] ?? '');
```

V skutočnosti však v tomto prípade bude pole `$_POST` prázdne a nebudú odovzdané žiadne údaje. Premenná `$_POST` sa používa len pre údaje získané z formulárov (môžete to zistiť podľa hlavičky HTTP, ktorú sme nevyhodili).

V tomto prípade teda potrebujeme získať údaje priamo z požiadavky HTTP, na čo sa používa **trick solution**. Potom sa ľahko používa:

```php
<?php

$data = json_decode(file_get_contents('php://input'), true);

echo htmlspecialchars($data['username'] ?? '');

header('Content-Type: application/json');
echo json_encode([
    'message' => 'Chyba servera',
]);
zomrieť;
```

Súčasťou príkladu je aj jednoduchá javascriptová odpoveď. Dôležité je správne vyhodiť hlavičku HTTP `'Content-Type: application/json'` a ukončiť skript po odoslaní všetkých údajov.

Vynucovanie používania `$_POST`
-------------------------

Ak aj napriek tomu chcete s odoslanými údajmi zaobchádzať priamo ako s formulárom, existuje spôsob, ako ich preniesť. V tomto prípade je potrebné upraviť vytvorenie samotného dotazu ajax a správne odovzdať hlavičky HTTP:

```js
axios.post(
    '/api/form-process',
    {
        username: 'Meno používateľa'
    },
    {
        hlavičky: { 'content-type': 'application/x-www-form-urlencoded'}
    }
).then(response => {
    // Niektoré spracovania
});
```

V tomto prípade však spracovanie na strane PHP nebude príjemné, pretože údaje musíme ešte opraviť a previesť na pole.

Podarilo sa mi na to vymyslieť tento horor:

```php
<?php

ak (\count($_POST) === 1
    && preg_match('/^{.*}$/', $post = array_keys($_POST)[0])
    && ($json = json_decode($post)) instanceof \stdClass
) {
    foreach ($json as $key => $value) {
        $_POST[$key] = $value;
        unset($_POST[$post]);
    }
}

echo htmlspecialchars($_POST['username'] ?? '');
```

Je však oveľa lepšie zostať pri prvom prístupe a získať údaje pomocou metódy `'php://input'`.
