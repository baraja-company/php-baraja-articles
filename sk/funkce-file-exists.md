PHP funkcia file_exists()
=========================

> id: '49446f2d-4091-420e-b5d2-3023c2861fe0'
> slug:
> 	cs: funkce-file-exists
> 	sk: php-funkcia-file-exists
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: f60333932197f39e71fcdb374d352c98

Dostupnosť v `PHP 4.0`

Skontroluje, či súbor alebo adresár existuje


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Cesta k súboru alebo adresáru. |


Vrátené hodnoty
----------------

`bool`

true, ak súbor alebo adresár zadaný pomocou
názov súboru existuje; inak false.
</p>
<p>
Táto funkcia vráti false pre symlinky smerujúce na neexistujúce
súbory.
</p>
<p>
Táto funkcia vráti false pre súbory nedostupné z dôvodu obmedzení v núdzovom režime. Avšak tieto
súbory môžu byť stále zahrnuté, ak
sú umiestnené v adresári safe_mode_include_dir.
</p>
<p>
Kontrola sa vykonáva pomocou skutočného UID/GID namiesto efektívneho.

Ďalšie zdroje
------------

[Oficiálna dokumentácia k existujúcim súborom](https://www.php.net/manual/en/function.file-exists.php)
