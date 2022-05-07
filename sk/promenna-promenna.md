Premenné Premenné
=================

> id: a0baeb3a-ab6e-4dd9-b1bc-b1872a12ee08
> slug:
> 	cs: promenna-promenna
> 	sk: premenne-premenne
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '590515cc711ec27cc0f0bfe190c8fbff'

> **Upozornenie:** Tento článok bol napísaný pred mnohými rokmi a niektoré informácie môžu byť zastarané alebo nesprávne. Majte to na pamäti pri čítaní.

**Premenné** nie sú určené na bežné nasadenie (riešia problémy, ktoré sa dajú vyriešiť iným spôsobom), používajú sa najmä na skrátenie zápisu a skomplikovanie prístupu do pamäte.

Uveďme si nasledujúci príklad:

```php
$x = 25;                  // obsahuje 25
$nacitana_promenna = 'x'; // obsahuje "x"
$y = $$nacitana_promenna; // obsahuje 25
echo $y;                  // vytlačí 25
```

Všimnite si dva doláre nasledujúce za sebou. V tomto prípade sa hodnota premennej $y načíta do premennej, ktorej názov je uvedený v premennej $nacitana_variable.

Trochu mätúce, čo? Preto radšej nepoužívajte premenné.
> **Poznámka:** Premenné sú špecialitou PHP kvôli znaku dolára. V iných jazykoch nie je začiatok názvu premennej označený žiadnym znakom, takže nemôžete používať premenné, pretože by bolo nejednoznačné, kedy ide o klasickú premennú a kedy nie.
