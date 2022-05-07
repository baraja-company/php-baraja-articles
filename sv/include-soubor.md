Inkludera (sätta ihop sidor från delar)
=======================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	sv: inkludera-saetta-ihop-sidor-fran-delar
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP är ursprungligen ett mallspråk, som skapades för att göra det enkelt att sätta ihop delar av sidor.

Format som stöds
-------------------

Folding fungerar i textform, så det är lämpligt att använda relevanta format som `.html` eller `.md`.

När en PHP-fil klistras in exekveras dess innehåll som om det fanns fysiskt på den plats där den klistras in.

Vikning av sidor och insättning av gemensamt innehåll
---------------------------------------------

Ofta behöver vi skapa flera sidor med gemensamt innehåll - till exempel en meny.

I vanlig HTML skulle vi först skapa en sida med en meny och sedan kopiera den många gånger. Men i PHP kan vi automatisera hela processen.

Vi har en fil `menu.html` där innehållet i menyn finns och `index.php` där vi lägger innehållet och menyn.

Ett enkelt exempel:

```php
<div class="sidan">
    <div class="innehåll">
        <?php
            include __DIR__
            . '/artikel/' . ($_GET['sidan'] ?? 'Index') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menu.html';
        ?>
    </div>
</div>
```

Det här skriptet infogar automatiskt sidans innehåll från katalogen `/article` och läser filnamnet enligt användarens inmatning (URL-parametern `?page=...`). Om ingen parameter anges används `index.html`.

URL:en kan till exempel se ut som `example.com?page=contacts` och ladda `/article/contacts.html`.
