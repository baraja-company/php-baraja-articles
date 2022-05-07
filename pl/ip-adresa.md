Uzyskiwanie adresu IP użytkownika w PHP
=======================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	pl: uzyskiwanie-adresu-ip-uzytkownika-w-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Uzyskanie adresu IP użytkownika w PHP, zapisanie adresu IP i zablokowanie użytkownika. Badanie sieci VPN i użytkownika za protokołem Proxy lub NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '54a77b5e4cc431b354903e42a27682a6'

W PHP bardzo łatwo jest wykryć adres IP na poziomie podstawowym:

```php
echo 'Wiesz, że twój adres IP jest' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Ostrzeżenie:** Uzyskanie adresu IP jako klucza pola `$_SERVER['REMOTE_ADDR']` jest możliwe tylko wtedy, gdy PHP zostało wywołane z przeglądarki. W trybie CLI (na przykład podczas uruchamiania z terminala za pomocą programu cron) adres IP nie jest dostępny (ma to sens, ponieważ nie jest wykonywane żadne żądanie sieciowe).

Niezawodne wykrywanie adresów IP
-----------------------------

Po wielu latach rozwoju ostatecznie zdecydowałem się na tę implementację:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Obsługa Cloudflare
        $ip = $_SERVER['HTTP_CF_CONNECTING_IP'];
    } elseif (isset($_SERVER['REMOTE_ADDR']) === true) {
        $ip = $_SERVER['REMOTE_ADDR'];
        if (preg_match('/^(?:127|10)\.0\.0\.[12]?\d{1,2}$/', $ip)) {
            if (isset($_SERVER['HTTP_X_REAL_IP'])) {
                $ip = $_SERVER['HTTP_X_REAL_IP'];
            } elseif (isset($_SERVER['HTTP_X_FORWARDED_FOR'])) {
                $ip = $_SERVER['HTTP_X_FORWARDED_FOR'];
            }
        }
    } else {
        $ip = '127.0.0.1';
    }
    if (in_array($ip, ['::1', '0.0.0.0', 'localhost'], true)) {
        $ip = '127.0.0.1';
    }
    $filter = filter_var($ip, FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    if ($filter === false) {
        $ip = '127.0.0.1';
    }

    return $ip;
}
```

A więc znacznie lepiej:

```php
echo 'Wiesz, że twój adres IP jest' . getIp() . '?';
```

Jeśli IP może być wykryte bezpośrednio, lub jest to tylko IPv6, lub jest to tryb CLI (np. cron), zwraca `127.0.0.1` (localhost).

Implementacje uwzględniające nagłówki `X-Forwarded-For` i `X-Real-IP` są bardzo niebezpieczne bezpośrednio w PHP, ponieważ dane mogą być łatwo modyfikowane, a atakujący może wyłudzić fałszywy adres IP, aby na przykład wyświetlić administrację lub włączyć tryb Debug na stronie (Nette Tracy). Z drugiej strony, musimy akceptować niektóre żądania proxy (na przykład podczas proxyowania ruchu przez Cloudflare lub podczas uruchamiania Apache'a i Ngnixa na tym samym komputerze, gdy są one wywoływane lokalnie tuż po sobie).

W przypadku bezpośredniego dostępu użytkownika do serwera istnieje tylko jedno poprawne rozwiązanie, a mianowicie zapewnienie w Apache (poprzez rozszerzenie `RemoteIP`) oraz w Nginx poprzez rozszerzenie `remote_ip`, że `X-Forwarded-For` jest ustawiony z rzeczywistego adresu IP odwiedzającego, oraz że adres IP nie może być ustawiony za pomocą nagłówka HTTP.

Pole `$_SERVER['REMOTE_ADDR']` automatycznie pobiera poprawny adres IP (czyli adres IP, z którego żądanie przyszło bezpośrednio do PHP) i nie musimy się tym zajmować.

Dostęp użytkownika przez serwer proxy
----------------------------

Często zdarza się, że użytkownik uzyskuje dostęp przez serwer proxy. Następnie rzeczywisty adres IP jest zapisywany w zmiennej `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Taki przypadek może wystąpić na przykład wtedy, gdy routing na serwerze jest rozwiązywany metodą `Ngnix -> Apache -> PHP`, gdzie `Ngnix` służy jako odwrotne proxy przed `Apache`. W tym przypadku PHP widzi tylko adres IP w sieci wewnętrznej (zwykle w postaci `127.0.0.*`).

Na przykład usługa **Cloudflare** może zachowywać się w ten sposób i należy zwrócić uwagę na to, czy pracujemy z adresem IP rzeczywistego użytkownika czy serwera proxy. Dla mnie najlepszym sposobem jest użycie funkcji `getIp()` wspomnianej na początku tego artykułu. Możemy zapewnić wykrycie przez Cloudflare, sprawdzając istnienie klucza `$_SERVER['HTTP_CF_CONNECTING_IP']`, który jest automatycznie przesyłany w każdym żądaniu proxy.

Zapisywanie adresu IP
------------------

Zależy to od tego, jaki adres IP masz do dyspozycji.

- Adres IPv4 IP może być przechowywany w 4 bajtach, służy do tego funkcja `ip2long`,
- Jednak w przypadku adresu IPv6 musimy użyć 16 bajtów i nie ma funkcji konwersji.

Jeśli serwer bazy danych nie obsługuje bezpośrednio typu danych dla adresu IP, zalecam przechowywanie adresu IP jako `varchar(39)`, gdzie obie wersje zmieszczą się jako ciąg znaków i będą czytelne dla człowieka.

> Przy przechowywaniu adresu IP należy rozważyć, czy ma sens przechowywanie również nazwy domeny wykrytej przez funkcję `gethostbyaddr`. Podczas notowania i wyszukiwania nie można ustalić nazwisk, ponieważ trwa to bardzo długo, a z czasem mogą one ulec zmianie.

Blokowanie adresu IP odwiedzającego
-----------------------------

Idealnym rozwiązaniem jest utworzenie listy zablokowanych adresów IP i porównywanie tej listy z bieżącym adresem IP przy każdym żądaniu. Jeśli adresy się zgadzają, żądanie zostanie natychmiast zatrzymane.

```php
$blackList = [
    'first-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Niestety Twój adres IP jest zablokowany :-(.';
    die; // Zakończ żądanie
}
```

W przykładzie przyjęto implementację funkcji `getIp()` taką jak w przykładzie powyżej.

Bardziej zaawansowanym rozwiązaniem jest sprawdzenie wystąpienia indeksu w tablicy:

```php
$blackList = [
    'first-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Niestety Twój adres IP jest zablokowany :-(.';
    die; // Zakończ żądanie
}
```

Adres IP serwera i nazwa serwera
---------------------------------

Adres IP serwera jest zwykle przechowywany w polu `$_SERVER['SERVER_ADDR']`, a jego nazwę można uzyskać za pomocą konstrukcji `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Jeśli jednak używana jest koncepcja `Ngnix -> Apache -> PHP`, a `Ngnix` pełni rolę odwrotnego proxy, prawdziwy adres IP serwera nie jest wyświetlany.

W tym przypadku nazwę serwera można znaleźć w polu `$_SERVER['SERVER_NAME']` lub za pomocą funkcji `php_uname('n')`. [Oficjalna dokumentacja funkcji uname](https://www.php.net/manual/en/function.php-uname.php).

Możemy wtedy użyć tej sztuczki, aby poznać publiczny adres IP serwera: `gethostbyname(php_uname('n'))`.
