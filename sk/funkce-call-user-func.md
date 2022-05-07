PHP funkcia call_user_func()
============================

> id: '994791e4-966a-4f32-b72d-c236177cbe0d'
> slug:
> 	cs: funkce-call-user-func
> 	sk: php-funkcia-call-user-func
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: d2414e4704bf03408ee9cc6307742bf8

Dostupnosť v `PHP 4.0`

Volanie používateľskej funkcie zadanej prvým parametrom


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$function` | `callback` | *not* | Funkcia, ktorá sa má zavolať. Metódy triedy je možné zavolať aj staticky pomocou tejto funkcie tak, že do tohto parametra odovzdáte array($classname, $methodname). Metódy triedy inštancie objektu možno navyše volať odovzdaním array($objectinstance, $methodname) tomuto parametru.
| `$parameter` | `mixed` | null, | Nula alebo viac parametrov, ktoré sa majú odovzdať funkcii. |
| `$_` | `mixed` | null |


Vrátené hodnoty
----------------

`zmiešané`

výsledok funkcie alebo false pri chybe.

Ďalšie zdroje
------------

[Oficiálna dokumentácia pre call-user-func](https://www.php.net/manual/en/function.call-user-func.php)
