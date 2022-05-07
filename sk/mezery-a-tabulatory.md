Odsadenie kódu pomocou medzier a tabulátorov
============================================

> id: '116f19ed-3753-498d-bb9e-e0f93b88c347'
> slug:
> 	cs: mezery-a-tabulatory
> 	sk: odsadenie-kodu-pomocou-medzier-a-tabulatorov
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0d2bed76-dcd2-4ceb-a4d7-174b74d96cc1'
> sourceContentHash: c3af36d7df67cf36b940c9702cb71549

Aby bol kód pre ostatných programátorov ľahko čitateľný a elegantný, musíme sa ho naučiť jednotne formátovať. Tento článok sa zaoberá používaním medzier a tabulátorov.

Sú na odsadenie kódu lepšie medzery alebo tabulátory? Toto je často nekonečná téma na diskusiu, ak hľadáte rýchlu a jednoznačnú odpoveď, väčšina dobrých programátorov uprednostňuje používanie tabulátorov, ale poďme si to pekne rozobrať.

Priestory
----------------------

Každý programátor a editor používa iný počet medzier pre odsadenie (najčastejšie však 4), čo vedie k nekonzistentnému kódu, ktorý môže byť ťažšie čitateľný pri čítaní cudzieho kódu. Okrem toho je potrebných viac znakov na odsadenie (čo zvyšuje jeho dátovú veľkosť).

Medzery však majú výhodu pri vykresľovaní kódu vo webovom prehliadači (kde sa na odsadenie používa HTML entita `&nbsp;`), takže ide o pomerne ľahko prenosný formát, ktorý získava výhodu len ako stabilná a spoľahlivá metóda vykresľovania (4 medzery sa vždy zobrazia ako 4 medzery).

Tabulátory
----------------------

Ich šírka je taká, akú nastaví programátor v editore (ak to editor dokáže), takže ak sa vám páči konkrétne odsadenie, žiadny problém - každý z nás sa môže pozrieť na ten istý kód s inou šírkou tabulátora. Zároveň je to veľmi úsporný znak, ktorý sa nemusí opakovať tak často ako medzery.

Pri vykresľovaní kódu s tabulátorom do stránky HTML je zvykom nahradiť tabulátory pevnými medzerami, aby sa zabezpečilo správne zobrazenie vo všetkých prehliadačoch:

```php
$code = '<?php
    $a = 5+3;
    $b = 4;
    ak ($a > $b) {
        echo $a . " > " . $b;
    } inak {
        echo $b . " <= " . $a;
    }
?>';

echo str_replace("\t", '&nbsp;', $code);
```
