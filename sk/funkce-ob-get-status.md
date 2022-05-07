PHP funkcia ob_get_status()
===========================

> id: '2eb99e32-7be9-49e2-a306-4c9c314bb7c4'
> slug:
> 	cs: funkce-ob-get-status
> 	sk: php-funkcia-ob-get-status
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '2d182a2cc04f5a672c2164c9018f6d8e'

Dostupnosť vo verziách: `PHP 4.2.0`

Získanie stavu výstupných vyrovnávacích pamätí


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$full_status` | `bool` | null | true pre vrátenie všetkých aktívnych úrovní výstupnej vyrovnávacej pamäte. Ak je false alebo nie je nastavená, vráti sa iba výstupný buffer najvyššej úrovne.


Vrátené hodnoty
----------------

`array`

Ak sa volá bez parametra full_status
alebo s full_status = false jednoduché pole
s nasledujúcimi prvkami:
2
[typ] => 0
[stav] => 0
[name] => URL-Rewriter
[del] => 1
)
]]>
Jednoduché výsledky ob_get_status
KeyValue
levelVýstupná úroveň vnorenia
typePHP_OUTPUT_HANDLER_INTERNAL (0) alebo PHP_OUTPUT_HANDLER_USER (1)
statusJedna z možností PHP_OUTPUT_HANDLER_START (0), PHP_OUTPUT_HANDLER_CONT (1) alebo PHP_OUTPUT_HANDLER_END (2)
nameNázov aktívneho výstupného obslužného programu alebo ' predvolený výstupný obslužný program', ak nie je nastavený
delErase-flag podľa nastavenia ob_start
</p>
<p>
Ak sa zavolá s full_status = true, zobrazí sa pole
s jedným prvkom pre každú aktívnu úroveň výstupného buffera.
Výstupná úroveň sa používa ako kľúč poľa najvyššej úrovne a každé pole
samotný prvok je ďalšie pole, ktoré obsahuje stavové informácie
na jednej aktívnej výstupnej úrovni.
Pole
(
[chunk_size] => 0
[veľkosť] => 40960
[block_size] => 10240
[typ] => 1
[stav] => 0
[name] => predvolený výstupný manipulátor
[del] => 1
)
[1] => pole
(
[chunk_size] => 0
[veľkosť] => 40960
[block_size] => 10240
[typ] => 0
[buffer_size] => 0
[stav] => 0
[name] => URL-Rewriter
[del] => 1
)
)
]]>
</p>
<p>
Úplný výstup obsahuje tieto ďalšie prvky:
Úplné výsledky ob_get_status
KeyValue
chunk_sizeVeľkosť chunk podľa nastavenia ob_start
veľkosť...
veľkosť bloku...

Ďalšie zdroje
------------

[Oficiálna dokumentácia ob-get-status](https://www.php.net/manual/en/function.ob-get-status.php)
