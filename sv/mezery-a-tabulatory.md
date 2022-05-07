Indentera kod med hjälp av mellanslag och tabulatorer
=====================================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	sv: indentera-kod-med-hjaelp-av-mellanslag-och-tabulatorer
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

För att koden ska vara lätt att läsa för andra programmerare och för att den ska vara elegant måste vi lära oss att formatera den på ett enhetligt sätt. I den här artikeln diskuteras användningen av mellanslag och tabulatorer.

Är mellanslag eller tabulatorer bättre för indentering av kod? Detta är ofta ett oändligt ämne för debatt, och om du vill ha ett snabbt och entydigt svar föredrar de flesta bra programmerare att använda tabbar, men låt oss dela upp det på ett snyggt sätt.

Utrymmen
----------------------

Varje programmerare och redigerare använder olika antal mellanslag för indragning (oftast 4), vilket leder till inkonsekvent kod som kan vara svårare att läsa när man läser någon annans kod. Dessutom behövs fler tecken för indragning (vilket ökar datastorleken).

Men mellanslag har en fördel när kod återges i en webbläsare (där HTML-entiteten `&nbsp;` används för indragning), så det är ett format som är relativt lätt att överföra och som endast får en fördel som en stabil och pålitlig återgivningsmetod (4 mellanslag kommer alltid att visas som 4 mellanslag).

Tabulatorer
----------------------

De är den bredd som programmeraren ställer in i redigeringsverktyget (om redigeringsverktyget kan göra det), så om du gillar en viss indragning är det inga problem - vi kan titta på samma kod med olika tabbbredder. Samtidigt är det en mycket ekonomisk karaktär som inte behöver upprepas så ofta som bara utrymmen.

När kod med tabulatur i en HTML-sida återges är det vanligt att ersätta tabulaturer med fasta mellanslag för att säkerställa korrekt visning i alla webbläsare:

```php
$code = "<?php
    $a = 5+3;
    $b = 4;
    if ($a > $b) {
        echo $a . " > " . $b;
    } annars {
        echo $b . " <= " . $a;
    }
?>';

echo str_replace("\t", '&nbsp;', $code);
```
