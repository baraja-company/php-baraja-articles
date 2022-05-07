PHP funkcia stream_socket_client()
==================================

> id: '8cc53341-41bc-4f68-ab07-ac46576f9bfc'
> slug:
> 	cs: funkce-stream-socket-client
> 	sk: php-funkcia-stream-socket-client
> 
> publicationDate: '2019-09-11 10:04:03'
> mainCategoryId: '0eeab3a7-a54b-46db-a253-ca6100145648'
> sourceContentHash: '864ddd14ec7d3de037f11fbc9035b775'

Dostupnosť v `PHP 5.0`

Otvorenie internetového alebo unixového doménového pripojenia


Parametre
--------------

| Parameter | Typ údajov | Predvolená hodnota | Poznámka |
|-----|-----|-----|-----|
| `$remote_socket` | `string` | *not* | Adresa zásuvky, ku ktorej sa chcete pripojiť. |
| `$errno` | `int` | null, | V prípade neúspešného pripojenia sa nastaví na číslo chyby na úrovni systému. |
| `$errstr` | `string` | null, | V prípade neúspešného pripojenia sa nastaví na chybovú správu na úrovni systému. |
| `$timeout` | `float` | null, | Počet sekúnd do uplynutia časového limitu systémového volania connect(). Tento parameter sa uplatňuje len vtedy, keď sa nevykonávajú pokusy o asynchrónne pripojenie. <p>Na nastavenie časového limitu pre čítanie/zápis údajov cez soket použite parameter stream_set_timeout, pretože časový limit sa uplatňuje len počas pripájania soketu. |
| `$flags` | `int` | null, | Pole Bitmask, ktoré môže byť nastavené na ľubovoľnú kombináciu príznakov pripojenia. V súčasnosti je výber príznakov pripojenia obmedzený na STREAM_CLIENT_CONNECT (predvolené), STREAM_CLIENT_ASYNC_CONNECT a STREAM_CLIENT_PERSISTENT. |
| `$context` | `resource` | null | Platný zdroj kontextu vytvorený pomocou stream_context_create. |


Vrátené hodnoty
----------------

`resource`

|bool Pri úspechu sa vráti zdroj prúdu, ktorý môže
používať spolu s ostatnými funkciami súborov (ako napr.
fgets, fgetss,
fwrite, fclose a
feof), false pri zlyhaní.

Ďalšie zdroje
------------

[Oficiálna dokumentácia ku klientovi stream-socket](https://www.php.net/manual/en/function.stream-socket-client.php)
