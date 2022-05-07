Informácie o konfigurácii PHP a servera (phpinfo(), php.ini)
============================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	sk: informacie-o-konfiguracii-php-a-servera-phpinfo-php.ini
> 
> perex: 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

Často potrebujeme zistiť čo najviac informácií o serveri, na čo sa výborne hodí natívna funkcia `phpinfo()`:

```php
phpinfo();

die;	// po zapísaní konfigurácie ukončite skript
```

Vďaka tomu môžete ľahko zistiť nainštalovanú verziu, rozšírenia, knižnice a mnoho ďalšieho.

> Informácie o konfigurácii a zmene nastavení nájdete na konci tohto článku.

Vyhľadanie konkrétnej časti konfigurácie
-------------------------------------

Niekedy je užitočné vypísať len konkrétne informácie, preto môžeme nastaviť prvý parameter tak, aby presne špecifikoval, čo nás zaujíma:

```php
phpinfo(INFO_MODULES);
```

Na nastavenie sa používajú preddefinované konštanty:

| Názov konštanty | Hodnota | Popis
|-------------------|-----------|------
| INFO_GENERAL | 1 | Všeobecná konfigurácia, umiestnenie php.ini, dátum poslednej aktualizácie, webový server, systémové informácie a ďalšie.
| INFO_CREDITS | 2 | PHP Credits, viac informácií nájdete v `phpcredits()`.
| INFO_CONFIGURATION| 4 | Aktuálne umiestnenie a nastavenia smerníc. Viac informácií poskytne funkcia `ini_get()`.
| INFO_MODULES | 8 | Informácie o nainštalovaných moduloch. Viac informácií nájdete vo funkcii `get_loaded_extensions()`.
| INFO_ENVIRONMENT | 16 | Informácie o premennej `Environment`, ktorá je k dispozícii ako `$_ENV`.
| INFO_VARIABLES | 32 | Prehľad nastavení superglobálnych premenných známych ako `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Informácie o licencii na používanie PHP a ďalších podmienkach používania.
| INFO_ALL | -1 | Zobrazenie všetkých informácií (predvolená hodnota)

Superglobálna premenná `$_SERVER`
---------------------------------

Priamo za behu môžeme zistiť pomerne veľa informácií o nastaveniach servera (napríklad e-mail správcovi webu, aktuálnu IP adresu návštevníka alebo aktuálne volanú adresu URL).

Zoznam všetkých existujúcich hodnôt je jednoduchý:

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ': ' . $value . '<br>';
}
```

> **Upozornenie:** Nie všetky indexy musia existovať (napríklad ak skript spustí cron v režime CLI, index s adresou URL stránky alebo IP adresou požiadavky nebude existovať).

Čítanie špecifických konfiguračných smerníc
-----------------------------------------

Veľká časť konfigurácie je uložená v súbore `php.ini` a nie je priamo prístupná z PHP bežným spôsobom. Napríklad maximálna veľkosť odosielaného súboru.

Ak chcete načítať konfiguráciu priamo, použite funkciu `ini_get()` (poznámka: táto funkcia nemusí byť povolená na všetkých serveroch, to platí najmä pre hostiteľov).

Ak chceme napríklad zistiť maximálnu veľkosť súboru, ktorý môžeme nahrať, musíme si napísať vlastnú implementáciu:

```php
/**
 * @autor Jan Barášek
 */
public static function getMaxUploadFileSize(): int
{
    $maxUpload = min(
        ini_get('post_max_size'),
        ini_get('upload_max_filesize')
    );

    if (strncmp($maxUpload, 'M', 1) === 0) {
        return (int) str_replace('M', '', $maxUpload);
    }

    return (int) $maxUpload;
}
```

Vracia maximálnu hodnotu `upload_max_filesize` a `post_max_size` v MB.

Konfigurácia servera a zmena nastavení
-------------------------------------

Samotné nastavenia sú uložené v súbore `php.ini`. Jeho umiestnenie možno ľahko zistiť pomocou funkcie `phpinfo()` alebo príkazom `php --ini`.

> php --ini

Configuration File (php.ini) Path: /etc/php/7.1/cli
Loaded Configuration File:         /etc/php/7.1/cli/php.ini
Scan for additional .ini files in: /etc/php/7.1/cli/conf.d
Additional .ini files parsed:      /etc/php/7.1/cli/conf.d/10-mysqlnd.ini,
/etc/php/7.1/cli/conf.d/10-opcache.ini,
/etc/php/7.1/cli/conf.d/10-pdo.ini,
/etc/php/7.1/cli/conf.d/20-calendar.ini,
/etc/php/7.1/cli/conf.d/20-ctype.ini,
/etc/php/7.1/cli/conf.d/20-exif.ini,
/etc/php/7.1/cli/conf.d/20-fileinfo.ini,
/etc/php/7.1/cli/conf.d/20-ftp.ini,
/etc/php/7.1/cli/conf.d/20-gd.ini,
/etc/php/7.1/cli/conf.d/20-gettext.ini

Prípadne možno cestu šikovne analyzovať (funguje v systémoch Linux):

php -r "phpinfo();" | grep php.ini

Vráti sa:

Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini

> **Dôležité:** Zvyčajne je konfigurácia rozdelená do niekoľkých súborov podľa prostredia a balíkov, pričom `php.ini` je globálne platný pre všetky, zatiaľ čo napríklad konfigurácia `CLI` je platná len pre režim CLI, t. j. volanie cronu alebo príkazu z terminálu.

Konfigurácia limitu veľkosti odosielaného súboru
----------------------------------------------

Príkladom vlastnosti, ktorá sa často konfiguruje priamo v súbore `php.ini`, je maximálna veľkosť nahrávaného súboru (predvolená hodnota je `2 MB`, čo je v roku 2018 už málo).

V konfiguračnom súbore sa to zapíše napríklad takto:

; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M

*Stredný bod znamená komentár, po ktorom nasledujú špecifické konfiguračné smernice.*
