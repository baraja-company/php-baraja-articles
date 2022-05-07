Włączenie (składanie stron z kawałków)
======================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	pl: wlaczenie-skladanie-stron-z-kawalkow
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP jest pierwotnie językiem szablonów, który został stworzony, aby ułatwić składanie fragmentów stron w całość.

Obsługiwane formaty
-------------------

Składanie działa w formie tekstowej, dlatego zaleca się używanie odpowiednich formatów, takich jak `.html` lub `.md`.

Gdy plik PHP jest wklejany, jego zawartość jest wykonywana tak, jakby fizycznie istniała w miejscu wklejenia.

Składanie stron i wstawianie wspólnej zawartości
---------------------------------------------

Często zachodzi potrzeba utworzenia kilku stron o wspólnej zawartości - na przykład menu.

W zwykłym języku HTML należałoby najpierw utworzyć stronę z menu, a następnie wielokrotnie ją skopiować. W PHP możemy jednak zautomatyzować cały proces.

Miejmy plik `menu.html`, w którym znajduje się zawartość menu, oraz `index.php`, w którym umieścimy zawartość i menu.

Prosty przykład:

```php
<div class="strona">
    <div class="treść">
        <?php
            include __DIR__
            . '/artykul/' . ($_GET['strona'] ?? 'Indeks') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menu.html';
        ?>
    </div>
</div>
```

Ten skrypt automatycznie wstawia zawartość strony z katalogu `/article` i odczytuje nazwę pliku zgodnie z danymi wprowadzonymi przez użytkownika (parametr URL `?page=...`). Jeśli nie zostanie przekazany żaden parametr, zostanie użyty plik `index.html`.

Tak więc adres URL może wyglądać na przykład tak: `example.com?page=contacts` i załadować `/article/contacts.html`.
