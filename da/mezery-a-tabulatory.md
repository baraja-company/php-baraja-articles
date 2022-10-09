Indryk kode ved hjælp af mellemrum og tabulatorer
=================================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	da: indryk-kode-ved-hjaelp-af-mellemrum-og-tabulatorer
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

For at gøre koden let at læse for andre programmører og for at holde den elegant, skal vi lære at formatere den ensartet. Denne artikel omhandler brugen af mellemrum og tabulatorer.

Er mellemrum eller tabulatorer bedre til indrykning af kode? Dette er ofte et uendeligt diskussionsemne, og hvis du leder efter et hurtigt og entydigt svar, foretrækker de fleste gode programmører at bruge faneblade, men lad os skille det pænt ad.

Rum
----------------------

Hver programmør og editor bruger et andet antal mellemrum til indrykning (men oftest 4), hvilket fører til inkonsekvent kode, som kan være sværere at læse, når man læser andres kode. Desuden er der brug for flere tegn til indrykning (hvilket øger datastørrelsen).

Mellemrum har dog en fordel ved gengivelse af kode i en webbrowser (hvor HTML-entiteten `&nbsp;` bruges til indrykning), så det er et format, der er relativt let at overføre, og som kun får en fordel som en stabil og pålidelig gengivelsesmetode (4 mellemrum vil altid blive vist som 4 mellemrum).

Tabulatorer
----------------------

De er den bredde, som programmøren indstiller i editoren (hvis editoren kan gøre det), så hvis du kan lide en bestemt indrykning, er det ikke noget problem - vi kan hver især se på den samme kode med forskellige tabulatorbredder. Samtidig er det en meget økonomisk karakter, som ikke behøver at blive gentaget så ofte som bare mellemrum.

Når kode med tabulatorindenteret kode gengives i en HTML-side, er det almindeligt at erstatte tabulatorerne med faste mellemrum for at sikre korrekt visning i alle browsere:

```php
$code = "<?php
    $a = 5+3;
    $b = 4;
    if ($a > $b) {
        echo $a . " > " . $b;
    } else {
        echo $b . " <= " . $a;
    }
?>';

echo str_replace("\t", '&nbsp;', $code);
```
