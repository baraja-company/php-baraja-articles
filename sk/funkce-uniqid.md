PHP funkcia uniqid()
====================

> id: '6871ccc8-590b-4cc9-ba18-d2aeab2b0838'
> slug:
> 	cs: funkce-uniqid
> 	sk: php-funkcia-uniqid
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '655419a65ff128aaa3fd1751c45934d4'

Dostupnosť v `PHP 4.0`

Generuje jedinečný identifikátor.

Používa sa najmä na generovanie ID v tabuľkách, ktoré sú uložené po častiach na viacerých serveroch a pravidelne sa synchronizujú. Ak databáza beží na viacerých miestach súčasne, nie je možné zaručiť jedinečnosť každého číselného ID ako automatického prírastku (pretože by musel existovať centrálny server, ktorý ich prideľuje), preto sa používa UID, reťazec, ktorý má na každom počítači iný prefix a generuje sa len jeho vnútorná štruktúra, ale základ zostáva rovnaký. Počas následnej synchronizácie tak nedochádza ku kolízii identifikátorov.

Vygenerovaný identifikátor obsahuje pomlčky a iné znaky. Ak potrebujeme zaručiť rovnakú dĺžku po sebe idúcich písmen a číslic, môžeme identifikátor zaheslovať: `md5(uniqid())`.

Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$prefix` | `string` | `""` | Môže byť užitočné, napríklad ak generujete identifikátory súčasne na viacerých serveroch, ktoré môžu generovať identifikátor v rovnakej mikrosekunde.
| `$more_entropy` | `bool` | `false` | Pri nastavení na `true` pridá uniqid na koniec návratovej hodnoty dodatočnú entropiu (pomocou kombinovaného lineárneho generátora kongruencie), vďaka čomu by mali byť výsledky unikátnejšie.


Vrátené hodnoty
----------------

`string`

Jedinečný identifikátor ako reťazec.

Ďalšie zdroje
------------

[Oficiálna dokumentácia uniqid](https://www.php.net/manual/en/function.uniqid.php)
