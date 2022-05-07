PHP funkcia stripos()
=====================

> id: '93bcf683-be6f-4cfa-84ee-69b04af69604'
> slug:
> 	cs: funkce-stripos
> 	sk: php-funkcia-stripos
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '537c382b24fc953f7b6ae59273e2d110'

Dostupnosť v `PHP 5.0`

Vyhľadá pozíciu prvého výskytu reťazca bez rozlíšenia veľkosti znaku.

> **TIP:**
>
> Funkcia `stripos` sa v minulosti používala na kontrolu, či reťazec obsahuje podreťazec.
> Od verzie PHP 8.0 existuje na tento účel natívna funkcia `str_contains()`.

Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` | *not* | String to search |
| `$needle` | `string` | *not* | Všimnite si, že ihla môže byť reťazec jedného alebo viacerých znakov. |
| `$offset` | `int` | null | Nepovinný parameter offset umožňuje určiť, ktorý znak v kope sena sa má začať hľadať. Vrátená pozícia je stále relatívna voči začiatku zásobníka.


Vrátené hodnoty
----------------

`int` - vráti pozíciu, na ktorej začína nájdený podreťazec.

Ak sa podreťazec v hľadanom reťazci nenachádza, vráti sa hodnota `false`.

Ďalšie zdroje
------------

[Oficiálna dokumentácia Stripos](https://www.php.net/manual/en/function.stripos.php)
