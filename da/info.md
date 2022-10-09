PHP- og serverkonfigurationsoplysninger (phpinfo(), php.ini)
============================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	da: php-og-serverkonfigurationsoplysninger-phpinfo-php.ini
> 
> perex:
> 	- 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> 	- 'PHP info, oplysninger om opsætning og konfiguration af webservere. Ændre konfiguration via php.ini'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

Ofte har vi brug for at finde ud af så mange oplysninger om serveren som muligt, og den native funktion `phpinfo()` er fantastisk til dette formål:

```php
phpinfo();

die;	// efter at have skrevet konfigurationen, afslutter du scriptet
```

Dette gør det nemt at se den installerede version, udvidelser, biblioteker og meget mere.

> Se slutningen af denne artikel for at få oplysninger om konfiguration og ændring af indstillinger.

Finde et specifikt konfigurationsafsnit
-------------------------------------

Nogle gange er det nyttigt kun at få vist specifikke oplysninger, så vi kan indstille den første parameter til at angive præcis det, vi er interesseret i:

```php
phpinfo(INFO_MODULES);
```

Der anvendes foruddefinerede konstanter til indstilling:

| Konstant navn | Værdi | Beskrivelse
|-------------------|-----------|------
| INFO_GENERAL | 1 | Generel konfiguration, placering af php.ini, dato for sidste opdatering, webserver, systemoplysninger og meget mere.
| INFO_CREDITS | 2 | PHP Credits, for mere se `phpcredits()`.
| INFO_CONFIGURATION| 4 | Direktiver om nuværende placering og indstillinger. Yderligere oplysninger fås ved hjælp af funktionen `ini_get()`.
| INFO_MODULES | 8 | Oplysninger om installerede moduler. Du kan finde flere oplysninger i funktionen `get_loaded_extensions()`.
| INFO_ENVIRONMENT | 16 | Oplysninger om variablen `Environment`, der er tilgængelig som `$_ENV`.
| INFO_VARIABLES | 32 | En oversigt over indstillinger for superglobale variabler, kendt som `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Oplysninger om PHP-brugslicens og andre brugsbetingelser.
| INFO_ALL | -1 | Viser alle oplysninger (standardværdi)

Superglobal variabel `$_SERVER`
---------------------------------

Vi kan finde ud af en hel del oplysninger om serverindstillingerne direkte under kørslen (f.eks. e-mailen til webmasteren, den besøgendes aktuelle IP-adresse eller den aktuelle URL-adresse).

Det er nemt at opregne alle eksisterende værdier:

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>';
}
```

> **Advarsel:** Ikke alle indeks behøver at eksistere (hvis scriptet f.eks. kører cron i CLI-tilstand, vil indekset med sidens URL eller IP-adressen for anmodningen ikke eksistere).

Læsning af specifikke konfigurationsdirektiver
-----------------------------------------

En stor del af konfigurationen er gemt i `php.ini` og er ikke direkte tilgængelig fra PHP på den normale måde. F.eks. den maksimale filstørrelse for upload.

Hvis du vil læse konfigurationen direkte, skal du bruge funktionen `ini_get()` (bemærk: denne funktion er muligvis ikke aktiveret på alle servere, dette gælder især for værter).

Hvis vi f.eks. ønsker at finde ud af, hvor stor en fil vi maksimalt kan uploade, skal vi skrive vores egen implementering:

```php
/**
 * @forfatter Jan Barášek
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

Returnerer den maksimale værdi af `upload_max_filesize` og `post_max_size` i MB.

Konfigurere serveren og ændre indstillinger
-------------------------------------

Selve indstillingerne er gemt i filen `php.ini`. Dens placering kan nemt findes ved at bruge funktionen `phpinfo()` eller ved at kalde kommandoen `php --ini`.

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

Alternativt kan stien analyseres på en smart måde (virker på Linux-systemer):

```shell
php -r "phpinfo();" | grep php.ini
```

Han kommer tilbage:

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Vigtigt:** Normalt er konfigurationen opdelt i flere filer i henhold til miljø og pakker, hvor `php.ini` er globalt gyldig for alle, mens f.eks. `CLI`-konfigurationen kun er gyldig for CLI-tilstand, dvs. til at kalde en cron eller en kommando fra Terminal.

Konfigurering af grænsen for uploadfilstørrelse
----------------------------------------------

Et eksempel på en egenskab, der ofte konfigureres direkte i `php.ini`, er den maksimale uploadfilstørrelse (standard er `2 MB`, hvilket allerede er lavt i 2018).

I konfigurationsfilen skrives dette f.eks. således:

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Midtpunkt betyder kommentar, efterfulgt af specifikke konfigurationsdirektiver.*
