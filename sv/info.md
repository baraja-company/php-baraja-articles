Information om PHP- och serverkonfiguration (phpinfo(), php.ini)
================================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	sv: information-om-php--och-serverkonfiguration-phpinfo-php.ini
> 
> perex:
> 	- 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> 	- 'PHP-info, information om installation och konfiguration av webbservern. Ändra konfigurationen via php.ini'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

Vi behöver ofta ta reda på så mycket information om servern som möjligt, och den inhemska funktionen `phpinfo()` är utmärkt för detta:

```php
phpinfo();

die;	// efter att ha skrivit konfigurationen avslutar du skriptet
```

Detta gör det enkelt att se den installerade versionen, tillägg, bibliotek och mycket mer.

> I slutet av den här artikeln finns information om hur du konfigurerar och ändrar inställningar.

Hitta ett specifikt konfigurationsavsnitt
-------------------------------------

Ibland är det bra att bara lista specifik information, så vi kan ställa in den första parametern för att specificera exakt vad vi är intresserade av:

```php
phpinfo(INFO_MODULES);
```

Fördefinierade konstanter används för inställningen:

| Konstant namn | Värde | Beskrivning
|-------------------|-----------|------
| INFO_GENERAL | 1 | Allmän konfiguration, php.ini plats, datum för senaste uppdatering, webbserver, systeminformation med mera.
| INFO_CREDITS | 2 | PHP Credits, för mer information se `phpcredits()`.
| INFO_CONFIGURATION| 4 | Aktuell plats och inställningsdirektiv. Mer information kommer att ges av funktionen `ini_get()`.
| INFO_MODULES | 8 | Information om installerade moduler. Mer information finns i funktionen `get_loaded_extensions()`.
| INFO_ENVIRONMENT | 16 | Information om variabeln `Environment` som finns tillgänglig som `$_ENV`.
| INFO_VARIABLES | 32 | En översikt över superglobala variabelinställningar, kända som `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Information om PHP:s användningslicens och andra användarvillkor.
| INFO_ALL | -1 | Visa all information (standardvärde)

Superglobal variabel `$_SERVER`.
---------------------------------

Vi kan ta reda på en hel del information om serverinställningarna direkt medan skriptet körs (t.ex. e-post till webmaster, besökarens aktuella IP-adress eller den aktuella URL-adressen).

Det är enkelt att lista alla befintliga värden:

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>';
}
```

> **Varning:** Alla index behöver inte finnas (om skriptet till exempel kör cron i CLI-läge kommer indexet med sidans URL eller IP-adressen för begäran inte att finnas).

Läsa specifika konfigurationsdirektiv
-----------------------------------------

En stor del av konfigurationen lagras i `php.ini` och är inte direkt tillgänglig från PHP på vanligt sätt. Till exempel den maximala filstorleken för uppladdning.

Om du vill läsa konfigurationen direkt använder du funktionen `ini_get()` (observera: den här funktionen kanske inte är aktiverad på alla servrar, detta gäller särskilt för värdar).

Om vi till exempel vill ta reda på den maximala filstorleken som vi kan ladda upp måste vi skriva vår egen implementering:

```php
/**
 * @författare Jan Barášek
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

Återger det högsta värdet för `upload_max_filesize` och `post_max_size` i MB.

Konfigurera servern och ändra inställningar
-------------------------------------

Själva inställningarna lagras i filen `php.ini`. Dess plats kan enkelt hittas genom att använda funktionen `phpinfo()` eller genom att använda kommandot `php --ini`.

```shell
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
```

Alternativt kan sökvägen analyseras på ett smart sätt (fungerar på Linuxsystem):

```shell
php -r "phpinfo();" | grep php.ini
```

Han kommer tillbaka:

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Viktigt:** Vanligtvis är konfigurationen uppdelad i flera filer beroende på miljö och paket, där `php.ini` är globalt giltig för alla, medan till exempel `CLI` konfigurationen endast är giltig för CLI-läge, dvs. för att kalla en cron eller ett kommando från terminalen.

Konfigurera gränsen för storleken på den uppladdade filen
----------------------------------------------

Ett exempel på en egenskap som ofta konfigureras direkt i `php.ini` är den maximala storleken på den uppladdade filen (standardvärdet är vanligtvis `2 MB`, vilket redan är lågt 2018).

I konfigurationsfilen skrivs detta till exempel på följande sätt:

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Midpunkt betyder kommentar, följt av specifika konfigurationsdirektiv.*
