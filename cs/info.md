Informace o PHP a konfigurace serveru (phpinfo(), php.ini)
================================

> id: 6f4a78c1-ca06-4619-9a28-a716a65a12a8
> slugCS: info
> perex: PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini
> publicationDate: 2019-08-22 20:48:46
> mainCategoryId: 4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154

Často potřebujeme o serveru zjistit co nejvíc informací, k tomu se výborně hodí nativní funkce `phpinfo()`:

```php
phpinfo();

die;	// po vypsání konfigurace script ukončíme
```

Snadno tak zjistíme nainstalovanou verzi, rozšíření, knihovny a mnoho dalšího.

> Informace o konfiguraci a změně nastavení se dozvíte na konci článku.

Zjištění konkrétní konfigurační sekce
-------------------------------------

Někdy se hodí vypsat jen konkrétní informaci, proto můžeme prvním parametrem nastavit, co nás přesně zajímá:

```php
phpinfo(INFO_MODULES);
```

K nastavení se používají předdefinované konstanty:

| Název konstanty	| Hodnota	| Popis
|-------------------|-----------|------
| INFO_GENERAL		| 1			| Obecná konfigurace, umístění php.ini, datum poslední aktualizace, Web Server, informace o systému a další.
| INFO_CREDITS		| 2			| PHP Credits, více poskytne funkce `phpcredits()`.
| INFO_CONFIGURATION| 4			| Aktuální umístění a nastavení direktiv. Více poskytne funkce `ini_get()`.
| INFO_MODULES		| 8			| Informace o nainstalovaných modulech. Více poskytne funkce `get_loaded_extensions()`.
| INFO_ENVIRONMENT	| 16		| Informace o `Environment` proměnné, dostupná jako `$_ENV`.
| INFO_VARIABLES	| 32		| Přehled nastavení superglobálních proměnných, známých jako `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE		| 64		| Informace o licenci použití PHP a další podmínky použití.
| INFO_ALL			| -1		| Zobrazí veškeré informace (výchozí hodnota)

Superglobální proměnná `$_SERVER`
---------------------------------

Poměrně hodně informací o nastavení serveru můžeme zjistit přímo za běhu scriptu (třeba e-mail na webmastera, aktuální IP adresu návštěvníka nebo aktuálně zavolanou URL).

Vypsání všech existujících hodnot uděláme jednoduše:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ': ' . $value . '<br>';
}
```

> **Pozor:** Ne všechny indexy musí vždy existovat (například pokud script spustí cron v CLI režimu, tak nebude existovat index s URL adresou stránky nebo IP adresa requestu).

Čtení konkrétních konfiguračních direktiv
-----------------------------------------

Velká část konfigurace je uložena v `php.ini` a není běžným způsobem z PHP přímo k dispozici. Například maximální velikost uploadovaného souboru.

Pro přímé čtení konfigurace slouží funkce `ini_get()` (pozor: funkce nemusí být na všech serverech povolena, to platí zejména pro hostingy).

Pokud chceme například zjistit, jak maximálně velký soubor můžeme uploadovat, musíme si napsat vlastní implementaci:

```php
/**
 * @return int
 * @author Jan Barášek
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

Vrátí maximální hodnotu `upload_max_filesize` a `post_max_size` v MB.

Konfigurace serveru a změna nastavení
-------------------------------------

Samotné nastavení je uloženo v souboru `php.ini`. Jeho umístění lze snadno zjistit funkcí `phpinfo()` nebo zavoláním příkazu `php --ini`.

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

Případně lze cestu šikovně vyparsovat (funguje na linuxových systémech):

```shell
php -r "phpinfo();" | grep php.ini
```

Vrátí:

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Důležité:** Obvykle se konfigurace dělí na více souborů podle prostředí a balíků, přičemž `php.ini` platí globálně pro všechny, zatímco třeba `CLI` konfigurace platí jen pro CLI režim, tedy zavolání cronu nebo příkazu z Terminálu.

Konfigurace limitu velikosti nahraného souboru
----------------------------------------------

Příkladem vlastnosti, která se často konfiguruje přímo v `php.ini` je maximální velikost uploadovaného souboru (defaultně bývá `2 MB`, což je v roce 2018 už málo).

V rámci konfiguračního souboru to je zapsáno třeba takto:

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Středník znamená komentář, poté následují konkrétní konfigurační direktivy.*
