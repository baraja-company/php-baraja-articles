Includere (piegare le pagine dai pezzi)
=======================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	it: includere-piegare-le-pagine-dai-pezzi
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP è originariamente un linguaggio di template, che è stato creato per rendere facile mettere insieme pezzi di pagine.

Formati supportati
-------------------

La piegatura funziona come testo, quindi è consigliabile usare formati rilevanti come `.html` o `.md`.

Quando un file PHP viene incollato, il suo contenuto viene eseguito come se esistesse fisicamente nella posizione incollata.

Piegare le pagine e inserire contenuti comuni
---------------------------------------------

Spesso abbiamo bisogno di creare diverse pagine che hanno un contenuto comune - per esempio, un menu.

In semplice HTML, creeremmo prima una pagina con un menu e poi la copieremmo molte volte. Ma in PHP possiamo automatizzare l'intero processo.

Abbiamo un file `menu.html` dove si trova il contenuto del menu e `index.php` dove mettiamo il contenuto e il menu.

Un semplice esempio:

```php
<div class="pagina">
    <div class="contenuto">
        <?php
            include __DIR__
            . '/articolo/' . ($_GET['pagina'] ?? 'Indice') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menu.html';
        ?>
    </div>
</div>
```

Questo script inserisce automaticamente il contenuto della pagina dalla directory `/article` e legge il nome del file secondo l'input dell'utente (parametro URL `?page=...`). Se non viene passato alcun parametro, viene usato `index.html`.

Quindi l'URL potrebbe essere come, per esempio, `example.com?page=contacts` e caricare `/article/contacts.html`.
