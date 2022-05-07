Informacje o konfiguracji PHP i serwera (phpinfo(), php.ini)
============================================================

> id: '6f4a78c1-ca06-4619-9a28-a716a65a12a8'
> slug:
> 	cs: info
> 	pl: informacje-o-konfiguracji-php-i-serwera-phpinfo-php.ini
> 
> perex:
> 	- 'PHP info, informace o nastavení a konfiguraci webového serveru. Změna konfigurace přes php.ini'
> 	- 'Informacje o PHP, informacje o ustawieniu i konfiguracji serwera WWW. Zmiana konfiguracji za pomocą php.ini'
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '4f1d7d70-c5b0-45f1-b1d2-d03c22aa4154'
> sourceContentHash: '9ad0961571b298fb1f30ea3e67b2973e'

Często potrzebujemy dowiedzieć się jak najwięcej informacji o serwerze, natywna funkcja `phpinfo()` jest do tego świetna:

```php
phpinfo();

die;	// po zapisaniu konfiguracji zakończ działanie skryptu
```

Dzięki temu można łatwo sprawdzić zainstalowaną wersję, rozszerzenia, biblioteki i wiele innych informacji.

> Informacje na temat konfigurowania i zmiany ustawień znajdują się na końcu tego artykułu.

Znajdowanie określonej sekcji konfiguracji
-------------------------------------

Czasami przydatne jest wylistowanie tylko określonych informacji, dlatego możemy ustawić pierwszy parametr, aby dokładnie określić, co nas interesuje:

```php
phpinfo(INFO_MODULES);
```

Do ustawiania używane są stałe predefiniowane:

| Nazwa stałej | Wartość | Opis
|-------------------|-----------|------
| INFO_GENERAL | 1 | Konfiguracja ogólna, lokalizacja php.ini, data ostatniej aktualizacji, serwer WWW, informacje o systemie i inne.
| INFO_CREDITS | 2 | Kredyty PHP, więcej informacji znajdziesz w `phpcredits()`.
| INFO_CONFIGURATION| 4 | Aktualna lokalizacja i dyrektywy dotyczące ustawień. Więcej informacji dostarczy funkcja `ini_get()`.
| INFO_MODULES | 8 | Informacje o zainstalowanych modułach. Więcej informacji na ten temat można znaleźć w funkcji `get_loaded_extensions()`.
| INFO_ENVIRONMENT | 16 | Informacja o zmiennej `Environment`, dostępnej jako `$_ENV`.
| INFO_VARIABLES | 32 | Przegląd ustawień zmiennych superglobalnych, znanych jako `EGPCS` (Environment, GET, POST, Cookie, Server).
| INFO_LICENSE | 64 | Informacja o licencji na używanie PHP i innych warunkach użytkowania.
| INFO_ALL | -1 | Wyświetlanie wszystkich informacji (wartość domyślna)

Zmienna superglobalna `$_SERVER`.
---------------------------------

Wiele informacji o ustawieniach serwera możemy uzyskać bezpośrednio podczas działania skryptu (np. e-mail do webmastera, aktualny adres IP odwiedzającego czy aktualnie wywoływany adres URL).

Wypisanie wszystkich istniejących wartości jest proste:

```php
foreach ($_SERVER as $key => $value) {
    echo $key . ':' . $value . '<br>.';
}
```

> **Ostrzeżenie:** Nie wszystkie indeksy muszą istnieć (na przykład jeśli skrypt uruchamia crona w trybie CLI, indeks z adresem URL strony lub adresem IP żądania nie będzie istniał).

Odczytywanie konkretnych dyrektyw konfiguracyjnych
-----------------------------------------

Znaczna część konfiguracji jest przechowywana w pliku `php.ini` i nie jest bezpośrednio dostępna z poziomu PHP w normalny sposób. Na przykład maksymalny rozmiar wysyłanego pliku.

Aby bezpośrednio odczytać konfigurację, należy użyć funkcji `ini_get()` (uwaga: funkcja ta może nie być włączona na wszystkich serwerach, dotyczy to zwłaszcza hostów).

Na przykład, jeśli chcemy dowiedzieć się, jaki jest maksymalny rozmiar pliku, który możemy przesłać, musimy napisać własną implementację:

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

Zwraca maksymalną wartość `upload_max_filesize` i `post_max_size` w MB.

Konfiguracja serwera i zmiana ustawień
-------------------------------------

Same ustawienia są przechowywane w pliku `php.ini`. Jego położenie można łatwo znaleźć za pomocą funkcji `phpinfo()` lub wywołując komendę `php --ini`.

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

Alternatywnie ścieżkę można sprytnie przetworzyć (działa w systemach Linux):

```shell
php -r "phpinfo();" | grep php.ini
```

Wróci:

```shell
Configuration File (php.ini) Path => /etc/php/7.1/cli
Loaded Configuration File => /etc/php/7.1/cli/php.ini
```

> **Ważne:** Zazwyczaj konfiguracja jest podzielona na kilka plików w zależności od środowiska i pakietów, gdzie `php.ini` jest globalnie ważny dla wszystkich, podczas gdy np. konfiguracja `CLI` jest ważna tylko dla trybu CLI, tj. wywołania crona lub komendy z Terminala.

Konfigurowanie limitu rozmiaru przesyłanego pliku
----------------------------------------------

Przykładem właściwości, która jest często konfigurowana bezpośrednio w `php.ini`, jest maksymalny rozmiar wysyłanego pliku (domyślnie `2 MB`, co w 2018 roku jest już niską wartością).

W pliku konfiguracyjnym zapisuje się to w następujący sposób, np:

```shell
; Maximum allowed size for uploaded files.
upload_max_filesize = 40M

; Must be greater than or equal to upload_max_filesize
post_max_size = 40M
```

*Punkt środkowy oznacza komentarz, po którym następują konkretne dyrektywy konfiguracyjne.
