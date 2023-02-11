Uzyskanie adresu IP użytkownika w PHP
=====================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	pl: uzyskanie-adresu-ip-uzytkownika-w-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Uzyskanie adresu IP użytkownika w PHP, zapisanie adresu IP i zbanowanie użytkownika. Badanie VPN i użytkownika za Proxy lub NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '284c4642c3fa98a026ce5a9e6625bb16'

W PHP bardzo łatwo jest wykryć adres IP na podstawowym poziomie:

```php
echo 'Wiesz, twój adres IP to' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Ostrzeżenie:** Uzyskanie adresu IP jako klucza pola `$_SERVER['REMOTE_ADDR']` jest możliwe tylko wtedy, gdy PHP zostało wywołane z przeglądarki. W trybie CLI (na przykład uruchomionym z Terminala za pomocą crona), adres IP nie jest dostępny (ma to sens, ponieważ nie jest wykonywane żadne żądanie sieciowe).

Niezawodne wykrywanie adresów IP
-----------------------------

Po wielu latach rozwoju, w końcu utknąłem z tą implementacją:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Wsparcie Cloudflare
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

O wiele lepiej:

```php
echo 'Wiesz, twój adres IP to' . getIp() . '?';
```

Jeśli IP może być wykryte bezpośrednio, lub jest tylko IPv6, lub jest w trybie CLI (np. cron), zwraca `127.0.0.1` (localhost).

Implementacje uwzględniające nagłówki `X-Forwarded-For` i `X-Real-IP` są niezwykle niebezpieczne bezpośrednio w PHP, ponieważ dane mogą być łatwo modyfikowane, a atakujący może spoofować fałszywy adres IP, aby np. przejrzeć administrację lub aktywować tryb Debug strony (Nette Tracy). Z drugiej strony, musimy zaakceptować niektóre proxy (na przykład, gdy proxujemy ruch przez Cloudflare, lub gdy uruchamiamy Apache i Ngnix na tej samej maszynie, gdy są one wywoływane lokalnie zaraz po sobie).

W przypadku bezpośredniego dostępu użytkownika do serwera istnieje tylko jedno poprawne rozwiązanie, a jest nim zapewnienie na Apache (poprzez rozszerzenie `RemoteIP`) oraz na Nginx poprzez rozszerzenie `remote_ip`, że `X-Forwarded-For` jest ustawiony z rzeczywistego adresu IP odwiedzającego, oraz że adres IP nie może być ustawiony za pomocą nagłówka HTTP.

Pole `$_SERVER['REMOTE_ADDR']` automatycznie pobiera prawidłowy adres IP (czyli adres IP, z którego żądanie przyszło bezpośrednio do PHP) i nie musimy się tym zajmować.

Dostęp użytkownika przez proxy
----------------------------

Często zdarza się, że użytkownik uzyskuje dostęp przez proxy. Następnie rzeczywisty adres IP jest przechowywany w zmiennej `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Przypadek ten może wystąpić np. gdy routing na serwerze rozwiązywany jest metodą `Ngnix -> Apache -> PHP`, gdzie `Ngnix` służy jako reverse proxy przed `Apache`. W tym przypadku PHP widzi tylko adres IP w obrębie sieci wewnętrznej (zwykle o postaci `127.0.0.*`).

Na przykład usługa **Cloudflare** może zachowywać się w ten sposób i należy zwrócić uwagę, czy pracujemy z adresem IP rzeczywistego użytkownika czy proxy. Dla mnie najlepszym sposobem jest użycie funkcji `getIp()` wspomnianej na początku tego artykułu. Wykrywanie Cloudflare możemy zapewnić poprzez weryfikację istnienia klucza `$_SERVER['HTTP_CF_CONNECTING_IP']`, który jest automatycznie przekazywany w każdym żądaniu proxy.

Wykrywanie VPN / Proxy
-------------------

Nie ma niezawodnego wykrywania użycia proxy lub VPN, ale w realnym środowisku możemy odfiltrować przynajmniej część ruchu.

Można to zrobić na kilka sposobów: Weź zakres adresów IP i porównaj adres IP, z którego przyszło żądanie.

U niektórych dostawców VPN listy adresów IP są dostępne nieoficjalnie (patrz np. https://gist.github.com/JamoCA/eedaf4f7cce1cb0aeb5c1039af35f0b7), w przypadku węzłów wyjściowych Tor oficjalnie (https://blog.torproject.org/changes-tor-exit-list-service, ale mostów Tor tam nie ma).

Inną opcją jest złożenie gdzieś wniosku online, co może zarówno opóźnić ładowanie strony, jeśli usługa nie działa, jak i "wyciekać" adresy IP odwiedzających do strony trzeciej. Od 2023 r. zdecydowanie odradzam takie podejście, ponieważ zaczyna ono polegać bardziej na ochronie i manipulowaniu danymi użytkowników.

To zapytanie online może być "naiwne" i wystarczy sprawdzić kto jest właścicielem zakresu lub czy jest to proxy/VPN (niektóre usługi mogą to zwrócić, ale domyślnie nie jest to częścią "IP info" np. z usługi whois).

(Najczęściej) stosuje się jakiś rodzaj rankingu reputacji, gdzie niektóre adresy IP "śmiecą" bardziej niż inne. Statystycznie więcej syfu pochodzi z różnych proxy, VPNów i Tora niż z domowych adresów IP (no może poza "zainfekowanymi" domowymi adresami IP). Taka ocena reputacji jest oferowana przez niektóre DNS Block Lists, zobacz jakąś losową listę, https://en.m.wikipedia.org/wiki/Comparison_of_DNS_blacklists i kolumnę "Listing goal", lub jest dostarczana bezpośrednio przez firmy takie jak Cloudflare w postaci "bot management" itp.

Wiele zależy od tego, jaki jest cel detekcji.

Przechowywanie adresów IP
------------------

To zależy od tego jaki adres IP masz dostępny.

- Adres IPv4 może być przechowywany w 4 bajtach, służy do tego funkcja `ip2long`,
- Jednak dla adresu IPv6 musimy użyć 16 bajtów i nie ma funkcji konwersji.

Jeśli twój serwer bazy danych nie obsługuje bezpośrednio typu danych dla adresu IP, polecam przechowywanie adresu IP jako `varchar(39)`, gdzie obie wersje będą pasować jako ciąg i będą czytelne dla człowieka.

> Przy przechowywaniu adresu IP należy rozważyć, czy ma sens przechowywanie również nazwy domeny wykrytej przez funkcję `gethostbyaddr`. Przy notowaniu i wyszukiwaniu nie można poznać nazwisk, bo trwa to bardzo długo, a one z czasem mogą się zmieniać.

Blokowanie adresu IP odwiedzającego
-----------------------------

Idealnym rozwiązaniem jest stworzenie listy zablokowanych adresów IP i porównanie tej listy z aktualnym adresem IP na każdym żądaniu. Jeśli adresy się zgadzają, żądanie zostanie natychmiast zatrzymane.

```php
$blackList = [
    'first-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Niestety twój adres ip jest zablokowany :-(.';
    die; // Zakończ żądanie
}
```

Przykład zakłada implementację funkcji `getIp()` jak w przykładzie powyżej.

Bardziej wydajnym rozwiązaniem jest sprawdzenie wystąpienia indeksu w tablicy:

```php
$blackList = [
    'first-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Niestety twój adres ip jest zablokowany :-(.';
    die; // Zakończ żądanie
}
```

Adres IP serwera i nazwa serwera
---------------------------------

Adres IP serwera jest zwykle przechowywany w polu `$_SERVER['SERVER_ADDR']`, a jego nazwę można uzyskać za pomocą konstrukcji `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Jeśli jednak wykorzystywana jest koncepcja `Ngnix -> Apache -> PHP`, a `Ngnix` występuje w roli reverse proxy, prawdziwy adres IP serwera nie jest wyświetlany.

W tym przypadku nazwę serwera można znaleźć w polu `$_SERVER['SERVER_NAME']` lub za pomocą funkcji `php_uname('n')`. [Oficjalna dokumentacja funkcji uname](https://www.php.net/manual/en/function.php-uname.php).

Możemy wtedy użyć tej sztuczki, aby poznać publiczny adres IP serwera: `gethostbyname(php_uname('n'))`.
