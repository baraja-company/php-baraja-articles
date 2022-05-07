PHP funkcia stream_socket_recvfrom()
====================================

> id: '1f707e7f-06e7-4a89-8931-60d12e052726'
> slug:
> 	cs: funkce-stream-socket-recvfrom
> 	sk: php-funkcia-stream-socket-recvfrom
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: d4cb3a669cf3dab37f6122cabed413c6

Dostupnosť v `PHP 5.0`

Prijíma údaje zo zásuvky, či už je pripojená alebo nie


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$socket` | `resource` | *not* | Vzdialená zásuvka. |
| `$length` | `int` | *not* | Počet bajtov, ktoré sa majú prijať zo zásuvky. |
| `$flags` | `int` | null, | Hodnota príznakov môže byť ľubovoľná kombinácia nasledujúcich hodnôt: <table> Možné hodnoty príznakov <tr valign="top"> <td>STREAM_OOB</td> <td> Spracovať OOB (out-of-band) údaje. </td> </tr> <tr valign="top"> <td>STREAM_PEEK</td> <td> Získajte údaje zo zásuvky, ale nespotrebujte vyrovnávaciu pamäť. Následné volania fread alebo stream_socket_recvfrom zobrazia rovnaké údaje. </td> </tr> </table> |
| `$address` | `string` | null | Ak je zadaná adresa, vyplní sa adresou vzdialenej zásuvky. |


Vrátené hodnoty
----------------

`string`

načítané údaje ako reťazec

Ďalšie zdroje
------------

[Oficiálna dokumentácia pre stream-socket-recvfrom](https://www.php.net/manual/en/function.stream-socket-recvfrom.php)
