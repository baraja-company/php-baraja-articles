Få fram användarens IP-adress i PHP
===================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	sv: fa-fram-anvaendarens-ip-adress-i-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Ta fram användarens IP-adress i PHP, lagra IP-adressen och förbjuda användaren. Undersökning av VPN och användare bakom proxy eller NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '54a77b5e4cc431b354903e42a27682a6'

I PHP är det mycket enkelt att upptäcka en IP-adress på en grundläggande nivå:

```php
echo 'Du vet, din IP-adress är' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Varning:** Att få IP-adressen som nyckel i fältet `$_SERVER['REMOTE_ADDR']` är endast möjligt om PHP anropades från webbläsaren. I CLI-läget (till exempel när du kör från Terminal med cron) är IP-adressen inte tillgänglig (det är logiskt eftersom ingen nätverksförfrågan görs).

Pålitlig identifiering av IP-adresser
-----------------------------

Efter många år av utveckling har jag slutligen fastnat för denna implementering:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Stöd för Cloudflare
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

Mycket bättre, då:

```php
echo 'Du vet, din IP-adress är' . getIp() . '?';
```

Om IP-adressen kan identifieras direkt, eller om det bara är IPv6, eller om den är i CLI-läge (t.ex. cron), returneras `127.0.0.0.1` (localhost).

Implementationer som tar hänsyn till headers som `X-Forwarded-For` och `X-Real-IP` är extremt farliga direkt i PHP, eftersom data lätt kan ändras och en angripare kan förfalska en falsk IP-adress för att till exempel se administrationen eller aktivera debug-läget på webbplatsen (Nette Tracy). Å andra sidan måste vi acceptera en del proxyförfrågningar (t.ex. när vi proxierar trafik via Cloudflare eller när Apache och Ngnix körs på samma maskin, när de anropas lokalt direkt efter varandra).

Vid direkt användaråtkomst till servern finns det bara en korrekt lösning, och det är att se till att Apache (via tillägget `RemoteIP`) och Nginx via tillägget `remote_ip` att `X-Forwarded-For` sätts från besökarens faktiska IP-adress, och att IP-adressen inte kan sättas med ett HTTP-huvud.

Fältet `$_SERVER['REMOTE_ADDR']` hämtar automatiskt den korrekta IP-adressen (dvs. den IP-adress från vilken begäran kom direkt till PHP) och vi behöver inte hantera den.

Användaråtkomst via proxy
----------------------------

Det händer ofta att en användare går in via en proxy. Därefter lagras den faktiska IP-adressen i variabeln `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Detta kan till exempel inträffa när routning på servern löses med hjälp av metoden `Ngnix -> Apache -> PHP`, där `Ngnix` fungerar som en omvänd proxy före `Apache`. I det här fallet ser PHP bara IP-adressen inom det interna nätverket (vanligtvis i formen `127.0.0.0.*`).

Tjänsten **Cloudflare** kan till exempel uppträda på detta sätt, och man bör vara försiktig med om man arbetar med den faktiska användarens IP-adress eller med proxytjänsten. För mig är det bästa sättet att använda funktionen `getIp()` som nämns i början av den här artikeln. Vi kan säkerställa att Cloudflare upptäcks genom att kontrollera att nyckeln `$_SERVER['HTTP_CF_CONNECTING_IP']` finns, som automatiskt överförs i varje proxyförfrågan.

Lagra IP-adressen
------------------

Det beror på vilken IP-adress du har tillgänglig.

- IPv4-IP-adresser kan lagras i 4 bytes, funktionen `ip2long` används för detta,
- För en IPv6-IP-adress måste vi dock använda 16 bytes och det finns ingen konverteringsfunktion.

Om din databasserver inte har direkt stöd för en datatyp för IP-adressen rekommenderar jag att IP-adressen lagras som `varchar(39)`, där båda versionerna passar som en sträng och är lättlästa.

> När du lagrar IP-adressen bör du överväga om det är meningsfullt att även lagra domännamnet som hittas med funktionen `gethostbyaddr`. När du listar och söker kan du inte ta reda på namnen eftersom det tar mycket lång tid och de kan ändras med tiden.

Blockera besökarens IP-adress
-----------------------------

Den idealiska lösningen är att skapa en lista över blockerade IP-adresser och jämföra denna lista med den aktuella IP-adressen vid varje begäran. Om adresserna stämmer överens stoppas begäran omedelbart.

```php
$blackList = [
    'första-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Tyvärr är din IP-adress blockerad :-(';
    die; // Avsluta begäran
}
```

Exemplet förutsätter en implementering av funktionen `getIp()` som i exemplet ovan.

En mer kraftfull lösning är att kontrollera om indexet förekommer i matrisen:

```php
$blackList = [
    'första-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Tyvärr är din IP-adress blockerad :-(';
    die; // Avsluta begäran
}
```

Serverns IP-adress och servernamn
---------------------------------

Serverns IP-adress lagras vanligtvis i fältet `$_SERVER['SERVER_ADDR']` och dess namn kan erhållas med konstruktionen `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Om konceptet `Ngnix -> Apache -> PHP` används och `Ngnix` fungerar som en omvänd proxy visas dock inte serverns verkliga IP-adress.

I det här fallet kan servernamnet hittas i fältet `$_SERVER['SERVER_NAME']` eller genom att använda funktionen `php_uname('n')`. [Officiell dokumentation om uname-funktionen] (https://www.php.net/manual/en/function.php-uname.php).

Vi kan sedan använda detta trick för att ta reda på serverns offentliga IP-adress: `gethostbyname(php_uname('n'))`.
