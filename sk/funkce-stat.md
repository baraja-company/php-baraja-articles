PHP funkcia stat()
==================

> id: '1239c03d-1724-4b24-9be8-57186c019217'
> slug:
> 	cs: funkce-stat
> 	sk: php-funkcia-stat
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '6f0fab99ec05c8c2b799020ca3dc5fbc'

Dostupnosť v `PHP 4.0`

Poskytuje informácie o súbore


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$filename` | `string` | *not* | Cesta k súboru. |


Vrátené hodnoty
----------------

`array`

<tabuľka>
výsledok stat a fstat
formát
<tr valign="top">
<td>Číselný</td>
<td>Asociatívne (od PHP 4.0.6)</td>
<td>Popis</td>
</tr>
<tr valign="top">
<td>0</td>
<td>dev</td>
<td>číslo zariadenia</td>
</tr>
<tr valign="top">
<td>1</td>
<td>ino</td>
<td>číslo uzla*</td>
</tr>
<tr valign="top">
<td>2</td>
<td>režim</td>
<td>režim ochrany uzla</td>
</tr>
<tr valign="top">
<td>3</td>
<td>odkaz</td>
<td>počet odkazov</td>
</tr>
<tr valign="top">
<td>4</td>
<td>uid</td>
<td>id používateľa vlastníka *</td>
</tr>
<tr valign="top">
<td>5</td>
<td>gid</td>
<td>groupid vlastníka *</td>
</tr>
<tr valign="top">
<td>6</td>
<td>rdev</td>
<td>typ zariadenia, ak ide o inode zariadenie</td>
</tr>
<tr valign="top">
<td>7</td>
<td>veľkosť</td>
<td>veľkosť v bajtoch</td>
</tr>
<tr valign="top">
<td>8</td>
<td>čas</td>
<td>čas posledného prístupu (časová značka Unix)</td>
</tr>
<tr valign="top">
<td>9</td>
<td>mtime</td>
<td>čas poslednej úpravy (časová značka Unix)</td>
</tr>
<tr valign="top">
<td>10</td>
<td>čas</td>
<td>čas poslednej zmeny inódu (časová značka Unixu)</td>
</tr>
<tr valign="top">
<td>11</td>
<td>blksize</td>
<td>veľkosť bloku IO súborového systému**</td>
</tr>
<tr valign="top">
<td>12</td>
<td>bloky</td>
<td>počet pridelených 512-bajtových blokov **</td>
</tr>
</tabuľka>
* V systéme Windows bude táto hodnota vždy 0.
</p>
<p>
** Platí len pre systémy podporujúce typ st_blksize - ostatné
systémy (napr. Windows) vrátia -1.
</p>
<p>
V prípade chyby stat vráti false.

Ďalšie zdroje
------------

[Oficiálna štatistika](https://www.php.net/manual/en/function.stat.php)
