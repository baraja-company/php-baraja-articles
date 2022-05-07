PHP funkcia strrpos()
=====================

> id: c2d989c9-94bc-4f21-838a-b9465065d4a9
> slug:
> 	cs: funkce-strrpos
> 	sk: php-funkcia-strrpos
> 
> publicationDate: '2019-09-11 10:04:04'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: a0a027c0d9858f855a6bc85c7d1aa120

Dostupnosť v `PHP 4.0`

Vyhľadanie pozície posledného výskytu podreťazca v reťazci


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$haystack` | `string` | *not* | Reťazec na vyhľadávanie. |
| `$needle` | `string` | *not* | Ak <b>needle</b> nie je reťazec, prevedie sa na celé číslo a použije sa ako poradová hodnota znaku. |
| `$offset` | `int` | 0 | Ak je zadaný, vyhľadávanie začne tento počet znakov počítaný od začiatku reťazca. Ak je hodnota záporná, vyhľadávanie sa začne od tohto počtu znakov od konca reťazca a bude sa vyhľadávať dozadu.


Vrátené hodnoty
----------------

`int`

|boolean <p>
Vracia polohu, v ktorej sa nachádza ihla vzhľadom na začiatok
reťazec <b>haystack</b> (nezávisle od smeru vyhľadávania)
alebo posunutie).
Všimnite si tiež, že pozície reťazca začínajú od 0 a nie od 1.
</p>
<p>
Vráti <b>FALSE</b>, ak sa ihla nenašla.
</p>

Ďalšie zdroje
------------

[Oficiálna dokumentácia strrpos](https://www.php.net/manual/en/function.strrpos.php)
