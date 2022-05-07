Zahrnúť (skladanie stránok z kusov)
===================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	sk: zahrnut-skladanie-stranok-z-kusov
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP je pôvodne šablónovací jazyk, ktorý bol vytvorený na jednoduché spájanie častí stránok.

Podporované formáty
-------------------

Skladanie funguje v textovej forme, preto je vhodné používať príslušné formáty, ako napríklad `.html` alebo `.md`.

Keď je súbor PHP vložený, jeho obsah sa vykoná, ako keby fyzicky existoval na vloženom mieste.

Skladanie stránok a vkladanie spoločného obsahu
---------------------------------------------

Často potrebujeme vytvoriť niekoľko stránok, ktoré majú spoločný obsah - napríklad menu.

V obyčajnom jazyku HTML by sme najprv vytvorili stránku s ponukou a potom ju mnohokrát skopírovali. V PHP však môžeme celý proces automatizovať.

Majme súbor `menu.html`, kde je obsah menu, a `index.php`, kam umiestnime obsah a menu.

Jednoduchý príklad:

```php
<div class="stránka">
    <div class="obsah">
        <?php
            include __DIR__
            . '/article/' . ($_GET['page'] ?? 'index') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menu.html';
        ?>
    </div>
</div>
```

Tento skript automaticky vloží obsah stránky z adresára `/article` a prečíta názov súboru podľa vstupu používateľa (parameter URL `?page=...`). Ak nie je odovzdaný žiadny parameter, použije sa `index.html`.

Takže adresa URL môže vyzerať napríklad takto: `example.com?page=contacts` a načítať `/article/contacts.html`.
