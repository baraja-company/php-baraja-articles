Zjištění IP adresy uživatele v PHP
================================

> id: "1d6d761e-c139-4624-8c32-b0f2c131a831"
> slugCS: ip-adresa
> perex: "Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem."
> publicationDate: "2020-02-28 10:30:21"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

V PHP lze IP adresu na základní úrovni detekovat velmi jednoduše:

```php
echo 'Víte že, vaše IP adresa je ' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Pozor:** Zjištění IP adresy jako klíče pole `$_SERVER['REMOTE_ADDR']` je možné pouze v případě, že bylo PHP voláno z prohlížeče. V client režimu (například spuštění z Terminálu cronem) není IP adresa k dispozici (dává to smysl, protože nebyl položen žádný request).

Spolehlivé zjištění IP adresy
-----------------------------

```php
function getIp(): string
{
    $ip = null;

    if (isset($_SERVER['REMOTE_ADDR'])) {
        $ip = filter_var($_SERVER['REMOTE_ADDR'], FILTER_VALIDATE_IP, FILTER_FLAG_IPV4);
    }

    return is_string($ip) ? $ip : '127.0.0.1';
}
```

Mnohem lepší tedy je:

```php
echo 'Víte že, vaše IP adresa je ' . getIp() . '?';
```

Pokud se IP podaří zjistit, nebo je pouze IPv6, nebo jde o CLI režim (například cron), vrací se `127.0.0.1` (localhost).

Implementace zohledňující hlavičky `X-Forwarded-For` a `X-Real-IP` je přímo v PHP extrémně nebezpečná, protože může snadno dojít k modifikaci dat a útočník může podstrčit falešnou IP adresu a tím například zobrazit administraci, nebo aktivovat Debug mód webu (Nette Tracy).

Jediné správné řešení je zajistit na Apache (pomocí `RemoteIP` rozšíření) a v Nginx přes `remote_ip` rozšíření, aby se `X-Forwarded-For` nastavovalo ze skutečné IP adresy návštěvníka a nebylo možné IP adresu nastavit HTTP hlavičkou.

Do pole `$_SERVER['REMOTE_ADDR']` se automaticky dostává správná IP adresa a nemusíme to řešit.

Ošetření výstupu funkcí `is_string($ip)` je velmi zásadní, protože proměnná `$ip` nemusela být definována (v CLI režimu bude proměnná `null`), případně může být IP adresa nevalidní (například na localhostu může být hodnotu `::1` a pak by proměnná byla `false`). Tento zápis podmínky oba případy řeší.

Přístup uživatele přes proxy
----------------------------

Často se stává, že uživatel přistupuje přes proxy. Poté je skutečná IP adresa uložena v proměnné `$_SERVER["HTTP_X_FORWARDED_FOR"]`.

Tento případ může nastat například v okamžiku, kdy je routing na serveru vyřešen metodou `Ngnix -> Apache -> PHP`, kdy `Ngnix` slouží jako reverzní proxy před `Apache`. V takovém případě PHP vidí jen IP adresu v rámci interní sítě (obvykle ve tvaru `127.0.0.*`).

Takto se může chovat například služba `Cloudflare` a je potřeba si dávat pozor, jestli pracujeme s IP adresou skutečného uživatele, nebo proxy serveru. Za mě je nejlepší způsob stáhle použití funkce `getIp()`, která je uvedena na začátku článku.

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
