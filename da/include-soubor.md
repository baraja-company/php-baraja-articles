Inkluderer (samling af sider fra stykker)
=========================================

> id: '4984832e-c11f-4e9e-8d3b-60561685389d'
> slug:
> 	cs: include-soubor
> 	da: inkluderer-samling-af-sider-fra-stykker
> 
> publicationDate: '2019-08-23 15:06:33'
> mainCategoryId: c5cec8c4-2a75-4f51-87c7-4d3acac0616f
> sourceContentHash: aea5867cf788e623dddd3ced4ea24a7a

PHP er oprindeligt et templating-sprog, som blev skabt for at gøre det nemt at sammensætte dele af sider.

Understøttede formater
-------------------

Foldning fungerer i tekstform, så det er tilrådeligt at bruge relevante formater som `.html` eller `.md`.

Når en PHP-fil indsættes, udføres dens indhold, som om det fysisk eksisterede på den indsatte placering.

Foldning af sider og indsættelse af fælles indhold
---------------------------------------------

Ofte har vi brug for at oprette flere sider, der har fælles indhold - f.eks. en menu.

I almindelig HTML ville vi først oprette en side med en menu og derefter kopiere den mange gange. Men i PHP kan vi automatisere hele processen.

Lad os have en fil `menu.html`, hvor indholdet af menuen er, og `index.php`, hvor vi lægger indholdet og menuen.

Et enkelt eksempel:

```php
<div class="side">
    <div class="indhold">
        <?php
            include __DIR__
            . '/article/' . ($_GET['side'] ?? 'Indeks') . '.html';
        ?>
    </div>
    <div class="menu">
                    include 'menu.html';
        ?>
    </div>
</div>
```

Dette script indsætter automatisk sidens indhold fra mappen `/article` og læser filnavnet i overensstemmelse med brugerens input (URL-parameteren `?page=...`). Hvis der ikke er angivet nogen parameter, anvendes `index.html`.

Så URL'en kan f.eks. se ud som `example.com?page=contacts` og indlæse `/article/contacts.html`.
