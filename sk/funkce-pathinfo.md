Funkcia PHP pathinfo()
======================

> id: b3827a32-edc1-4057-8212-4d550b7a2fda
> slug:
> 	cs: funkce-pathinfo
> 	sk: funkcia-php-pathinfo
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '0bdb7c62f0ec0dc9737488c550bfef96'

Dostupnosť vo verziách: `PHP 4.0.3`

Vracia informácie o ceste k súboru


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$path` | `string` | *not* | Kontrolovaná cesta. |
| `$options` | `int` | null | Môžete určiť, ktoré prvky sa vrátia pomocou voliteľných parametrov. Skladá sa z PATHINFO_DIRNAME, PATHINFO_BASENAME, PATHINFO_EXTENSION a PATHINFO_FILENAME. V predvolenom nastavení vráti všetky prvky.


Vrátené hodnoty
----------------

`zmiešané`

Vrátia sa nasledujúce asociatívne prvky poľa:
dirname, basename,
príponu (ak existuje) a názov súboru.
</p>
<p>
Ak sa použijú možnosti, táto funkcia vráti
reťazec, ak nie sú požadované všetky prvky.

Ďalšie zdroje
------------

[Oficiálna dokumentácia pathinfo](https://www.php.net/manual/en/function.pathinfo.php)
