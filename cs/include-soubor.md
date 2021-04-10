Include (skládání stránek z kousků)
===================================

> id: "4984832e-c11f-4e9e-8d3b-60561685389d"
> slug:
> 	cs: include-soubor
> 
> publicationDate: "2019-08-23 15:06:33"
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f

PHP je původně šablonovací jazyk, který vznikl pro možnost jednoduchého skládání kusů stránek dohromady.

Podporované formáty
-------------------

Skládání funguje formou textu, proto je vhodné používat relevantní formáty, jako je například `.html` nebo `.md`.

Při vložení PHP souboru se jeho obsah vykoná, jako kdyby na vkládaném místě fyzicky existoval.

Skládání stránek a vkládání společného obsahu
---------------------------------------------

Často potřebujeme vytvořit několik stránek, které mají společný obsah - například menu.

V čistém HTML bychom nejprve vytvořili stránku s menu a tu následně mnohokrát rozkopírovali. V PHP ale můžeme celý proces automatizovat.

Mějme soubor `menu.html`, kde je obsah menu a `index.php`, kam vkládáme obsah a menu.

Jednoduchý příklad:

```php
<div class="page">
    <div class="content">
        <?php
            include __DIR__
            . '/article/' . ($_GET['page'] ?? 'index') . '.html';
        ?>
    </div>
    <div class="menu">
        <?php
            include 'menu.html';
        ?>
    </div>
</div>
```

Tento script automaticky vloží obsah stránky z adresáře `/article` a načte název souboru podle uživatelského vstupu (URL parametr `?page=...`). Pokud nebyl předán žádný parametr, použije se `index.html`.

Takže URL může vypadat například: `example.com?page=kontakty` a načte se `/article/kontakty.html`.
