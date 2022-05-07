Funkcia PHP setcookie()
=======================

> id: f3d57a13-921b-4d45-b2ac-8dbd44cf1a89
> slug:
> 	cs: funkce-setcookie
> 	sk: funkcia-php-setcookie
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '3503fb26bfe7553f056efc4b68386f64'

Dostupnosť v `PHP 4.0`

Odoslanie súboru cookie


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$name` | `string` | *not* | Názov súboru cookie. |
| `$value` | `string` | null, | Hodnota súboru cookie. Táto hodnota je uložená v počítači klienta; neukladajte citlivé informácie. Za predpokladu, že názov je 'cookiename', táto hodnota sa získa prostredníctvom $_COOKIE['cookiename'] |
| `$expire` | `int` | null, | Čas vypršania platnosti súboru cookie. Ide o unixovú časovú značku, takže je uvedená v počte sekúnd od epochy. Inými slovami, s najväčšou pravdepodobnosťou ju nastavíte pomocou funkcie čas plus počet sekúnd pred uplynutím platnosti. Alebo môžete použiť mktime. time()+60*60*24*30 nastaví platnosť súboru cookie na 30 dní. Ak je nastavená hodnota 0 alebo je vynechaná, platnosť súboru cookie vyprší na konci relácie (po zatvorení prehliadača).
| `$path` | `string` | null, | Cesta na serveri, na ktorom bude cookie k dispozícii. Ak je nastavená hodnota "/", súbor cookie bude k dispozícii v rámci celej domény. Ak je nastavená na '/foo/', súbor cookie bude k dispozícii len v adresári /foo/ a vo všetkých podadresároch, ako napríklad /foo/bar/ domény. Predvolená hodnota je aktuálny adresár, v ktorom sa súbor cookie nastavuje. |
| `$domain` | `string` | null, | Doména, v ktorej je cookie k dispozícii. Ak chcete, aby bol súbor cookie dostupný na všetkých subdoménach example.com, nastavte ho na '.example.com'. Táto funkcia nie je potrebná, ale umožňuje kompatibilitu s viacerými prehliadačmi. Nastavenie na www.example.com spôsobí, že súbor cookie bude k dispozícii len na subdoméne www. Podrobnosti nájdete v špecifikácii v časti o párovaní chvostov.
| `$secure` | `bool` | null, | Označuje, že súbor cookie by sa mal prenášať len cez zabezpečené pripojenie HTTPS od klienta. Ak je nastavená na hodnotu true, súbor cookie sa nastaví len vtedy, ak existuje zabezpečené pripojenie. Na strane servera je na programátorovi, aby tento druh cookie odosielal len pri zabezpečenom pripojení (napr. s ohľadom na $_SERVER["HTTPS"]).
| `$httponly` | `bool` | null | Ak je hodnota true, súbor cookie bude prístupný len prostredníctvom protokolu HTTP. To znamená, že súbor cookie nebude prístupný skriptovacím jazykom, ako je napríklad JavaScript. Toto nastavenie môže účinne pomôcť obmedziť krádeže identity prostredníctvom útokov XSS (hoci ho nepodporujú všetky prehliadače). Pridané v PHP 5.2.0. true alebo false |


Vrátené hodnoty
----------------

`bool`

Ak výstup existuje pred volaním tejto funkcie,
setcookie zlyhá a vráti false. Ak
setcookie úspešne spustí, vráti hodnotu true.
To neznamená, či používateľ prijal súbor cookie.

Ďalšie zdroje
------------

[Oficiálna dokumentácia setcookie](https://www.php.net/manual/en/function.setcookie.php)
