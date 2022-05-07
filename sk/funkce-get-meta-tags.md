PHP funkcia get_meta_tags()
===========================

> id: d1dbc4f7-44ef-458e-b9d3-d88690b9b733
> slug:
> 	cs: funkce-get-meta-tags
> 	sk: php-funkcia-get-meta-tags
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: c4cceb46f22e4b9d482bf6731eb8375b

Dostupnosť v `PHP 4.0`

Extrahuje všetky atribúty obsahu meta tagov zo súboru a vráti pole


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Cesta k súboru HTML ako reťazec. Môže to byť miestny súbor alebo adresa URL. |
| `$use_include_path` | `bool` | null | Nastavenie use_include_path na hodnotu true spôsobí, že PHP sa pokúsi otvoriť súbor podľa štandardnej cesty include podľa smernice include_path. Používa sa pre miestne súbory, nie pre adresy URL.


Vrátené hodnoty
----------------

`array`

pole so všetkými analyzovanými meta značkami.
</p>
<p>
Hodnota vlastnosti name sa stáva kľúčom, hodnota vlastnosti content
sa stane hodnotou vráteného poľa, takže môžete ľahko použiť
štandardné funkcie na prechádzanie poľom alebo prístup k jednotlivým hodnotám.
Špeciálne znaky v hodnote vlastnosti name sú nahradené
'_', zvyšok sa prevedie na malé písmená. Ak majú dve meta značky rovnaký
meno, vráti sa len posledné.

Ďalšie zdroje
------------

[Oficiálna dokumentácia pre get-meta-tags](https://www.php.net/manual/en/function.get-meta-tags.php)
