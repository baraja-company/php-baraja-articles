Zjištění IP adresy uživatele v PHP
==================================

> id: "1d6d761e-c139-4624-8c32-b0f2c131a831"
> slug:
> 	cs: ip-adresa
> 
> perex: "Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem."
> publicationDate: "2020-02-28 10:30:21"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

V PHP lze IP adresu na základní úrovni detekovat velmi jednoduše:

```php
echo 'Víte že, vaše IP adresa je ' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Pozor:** Zjištění IP adresy jako klíče pole `$_SERVER['REMOTE_ADDR']` je možné pouze v případě, že bylo PHP voláno z prohlížeče. V CLI režimu (například spuštění z Terminálu cronem) není IP adresa k dispozici (dává to smysl, protože se nevykonává žádný síťový request).

Spolehlivé zjištění IP adresy
-----------------------------

Po dlouhých letech vývoje jsem nakonec zůstal u této implementace:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Cloudflare support
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

Mnohem lepší tedy je:

```php
echo sprintf('Víte že, vaše IP adresa je %s?', getIp());
```

Pokud se IP podaří zjistit rovnou, nebo je pouze IPv6, nebo jde o CLI režim (například cron), vrací se `127.0.0.1` (localhost).

Implementace zohledňující hlavičky `X-Forwarded-For` a `X-Real-IP` je přímo v PHP extrémně nebezpečná, protože může snadno dojít k modifikaci dat a útočník může podstrčit falešnou IP adresu a tím například zobrazit administraci, nebo aktivovat Debug mód webu (Nette Tracy). Na druhou stranu některé proxované requesty musíme přijímat (například při proxování provozu přes službu Cloudflare, nebo při běhu Apache a Ngnix na jednom stroji, kdy se volají lokálně hned po sobě).

V případě příméh přístupu uživatele k serveru existuje jen jediné správné řešení, a to zajistit na Apache (pomocí `RemoteIP` rozšíření) a v Nginx přes `remote_ip` rozšíření, aby se `X-Forwarded-For` nastavovalo ze skutečné IP adresy návštěvníka a nebylo možné IP adresu nastavit HTTP hlavičkou.

Do pole `$_SERVER['REMOTE_ADDR']` se automaticky dostává správná IP adresa (tedy IP adresa, ze které přišel request přímo na PHP) a nemusíme to řešit.

Přístup uživatele přes proxy
----------------------------

Často se stává, že uživatel přistupuje přes proxy. Poté je skutečná IP adresa uložena v proměnné `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Tento případ může nastat například v okamžiku, kdy je routing na serveru vyřešen metodou `Ngnix -> Apache -> PHP`, kdy `Ngnix` slouží jako reverzní proxy před `Apache`. V takovém případě PHP vidí jen IP adresu v rámci interní sítě (obvykle ve tvaru `127.0.0.*`).

Takto se může chovat například služba **Cloudflare** a je potřeba si dávat pozor, jestli pracujeme s IP adresou skutečného uživatele, nebo proxy serveru. Za mě je nejlepší způsob stáhle použití funkce `getIp()`, která je uvedena na začátku článku. Detekci Cloudflare můžeme zajistit ověřením existence klíče `$_SERVER['HTTP_CF_CONNECTING_IP']`, který se v každém proxovaném requestu automaticky přenáší.

Detekce VPN / Proxy
-------------------

Spolehlivá detekce použití proxy nebo VPN neexistuje, nicméně v reálném prostředí můžeme aspoň část provozu odfiltrovat.

Je několik možností, jak to udělat: Vzít rozsah IP adres a porovnat IP adresu, ze které přišel požadavek.

Od některých poskytovatelů VPN jsou seznamy IP adres dostupné neoficiálně (viz např. https://gist.github.com/JamoCA/eedaf4f7cce1cb0aeb5c1039af35f0b7), v případě Tor exit nodů oficiálně (https://blog.torproject.org/changes-tor-exit-list-service, ale Tor bridges tam nejsou).

Další možností je udělat online dotaz někam, což jednak může zdržet načítání stránky pokud daná služba nefunguje a taky to "leakuje" IP adresy návštěvníků třetí straně. Od roku 2023 bych tento přístup výrazně nedoporučil, protože se začíná víc řešit ochrana dat uživatelů a manipulace s nimi.

Tohle online dotazování může být "naivní" a jen se třeba podívat kdo daný rozsah vlastní nebo jestli je to proxy/VPN (některé služby to mohou vracet, standardně to ale součástí "IP info" např. z whois služby nebývá).

(Nej)Častěji se používá nějaké reputační hodnocení, kdy z některých IP adres "chodí bordel" víc, než z jiných. Z různých proxy, VPN a Torů chodí statisticky bordel víc, než z domácích IP adres (tedy snad kromě "zavirovaných" domácích IP adres). Takové reputační hodnocení nabízí některé DNS Block Listy, viz nějaký random seznam, https://en.m.wikipedia.org/wiki/Comparison_of_DNS_blacklists a sloupec "Listing goal", nebo to rovnou poskytují firmy jako Cloudflare v podobě "bot managementu" apod.

Hodně záleží co je cílem detekce.

Ukládání IP adresy
------------------

Záleží na tom, jakou IP adresu máte k dispozici.

- IPv4 IP adresu lze uložit do 4 bajtů, k tomu slouží funkce `ip2long`,
- Pro IPv6 IP adresu však musíme použít 16 bajtů a převodní funkce neexistuje.

Pokud váš databázový server přímo nepodporuje datový typ pro IP adresu, doporučuji IP adresu ukládat jako `varchar(39)`, kam se vejdou obě verze jako řetězec a zároveň bude čitelná člověkem.

> Při ukládání IP adresy zvažte, jestli dává smysl ukládat i doménové jméno zjištěné funkcí `gethostbyaddr`. Při výpisu a vyhledávání zjišťovat jména nelze, protože to trvá velmi dlouho a navíc se mohou v čase změnit.

Blokace IP adresy návštěvníka
-----------------------------

Ideální řešení je vytvoření seznamu blokovaných IP adres a při každém requestu tento seznam porovnat s aktuální IP adresou. Pokud se adresy shodují, bude request ihned zastaven.

```php
$blackList = [
    'prvni-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Bohužel máte blokovanou ip adresu :-(';
    die; // Ukončíme request
}
```

Ukázka předpokládá implementaci funkce `getIp()` jako je v ukázce výše.

Výkonnější řešení je ověřovat výskyt indexu v poli:

```php
$blackList = [
    'prvni-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Bohužel máte blokovanou ip adresu :-(';
    die; // Ukončíme request
}
```

IP adresa serveru a název serveru
---------------------------------

IP adresa serveru je obvykle uložena v poli `$_SERVER['SERVER_ADDR']` a jeho název lze tedy získat konstrukcí: `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Pokud se však používá koncepce `Ngnix -> Apache -> PHP` a `Ngnix` je v roli reverzní proxy, tak se reálná IP adresa serveru nezobrazuje.

Název serveru lze v takovém případě zjistit v poli `$_SERVER['SERVER_NAME']`, nebo funkcí `php_uname('n')`. [Oficiální dokumentace funkce uname](https://www.php.net/manual/en/function.php-uname.php).

Tímto trikem potom můžeme zjistit veřejnou IP adresu serveru: `gethostbyname(php_uname('n'))`.
