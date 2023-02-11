Získanie IP adresy používateľa v jazyku PHP
===========================================

> id: '1d6d761e-c139-4624-8c32-b0f2c131a831'
> slug:
> 	cs: ip-adresa
> 	sk: ziskanie-ip-adresy-pouzivatela-v-jazyku-php
> 
> perex:
> 	- 'Zjištění IP adresy uživatele v PHP, ukládání IP adresy a ban uživatele. Ošetření VPN a uživatele za Proxy nebo NATem.'
> 	- 'Získanie IP adresy používateľa v PHP, uloženie IP adresy a zakázanie používateľa. Skúmanie VPN a používateľa za proxy serverom alebo NAT.'
> 
> publicationDate: '2020-02-28 10:30:21'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '284c4642c3fa98a026ce5a9e6625bb16'

V jazyku PHP je veľmi jednoduché zistiť IP adresu na základnej úrovni:

```php
echo 'Viete, vaša IP adresa je' . $_SERVER['REMOTE_ADDR'] . '?';
```

> **Upozornenie:** Získanie IP adresy ako kľúča poľa `$_SERVER['REMOTE_ADDR']` je možné len vtedy, ak bolo PHP volané z prehliadača. V režime CLI (napríklad pri spustení z terminálu pomocou cronu) nie je IP adresa k dispozícii (to je logické, pretože sa nevykonáva žiadna sieťová požiadavka).

Spoľahlivé zisťovanie adries IP
-----------------------------

Po mnohých rokoch vývoja som nakoniec zostal pri tejto implementácii:

```php
function getIp(): string
{
    if (isset($_SERVER['HTTP_CF_CONNECTING_IP'])) { // Podpora Cloudflare
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

Potom je to oveľa lepšie:

```php
echo 'Viete, vaša IP adresa je' . getIp() . '?';
```

Ak je možné IP zistiť priamo, alebo je to len IPv6, alebo je v režime CLI (napr. cron), vráti `127.0.0.1` (localhost).

Implementácie, ktoré zohľadňujú hlavičky `X-Forwarded-For` a `X-Real-IP`, sú priamo v jazyku PHP veľmi nebezpečné, pretože údaje sa dajú ľahko upraviť a útočník môže podvrhnúť falošnú IP adresu, napríklad na zobrazenie administrácie alebo aktiváciu režimu Debug na stránke (Nette Tracy). Na druhej strane musíme akceptovať niektoré proxované požiadavky (napríklad pri proxovaní prevádzky cez Cloudflare alebo pri spustení Apache a Ngnix na tom istom počítači, keď sú volané lokálne hneď po sebe).

V prípade priameho prístupu používateľa k serveru existuje len jedno správne riešenie, a to zabezpečiť v Apache (prostredníctvom rozšírenia `RemoteIP`) a v Nginx prostredníctvom rozšírenia `remote_ip`, aby sa `X-Forwarded-For` nastavil zo skutočnej IP adresy návštevníka a aby IP adresa nemohla byť nastavená pomocou hlavičky HTTP.

Pole `$_SERVER['REMOTE_ADDR']` automaticky získava správnu IP adresu (t. j. IP adresu, z ktorej prišla požiadavka priamo do PHP) a my sa ňou nemusíme zaoberať.

Prístup používateľa cez proxy server
----------------------------

Často sa stáva, že používateľ pristupuje cez proxy server. Potom sa skutočná IP adresa uloží do premennej `$_SERVER['HTTP_X_FORWARDED_FOR']`.

Tento prípad môže nastať napríklad vtedy, keď je smerovanie na serveri riešené pomocou metódy `Ngnix -> Apache -> PHP`, kde `Ngnix` slúži ako reverzný proxy server pred `Apache`. V tomto prípade PHP vidí iba IP adresu vo vnútornej sieti (zvyčajne v tvare `127.0.0.*`).

Takto sa môže správať napríklad služba **Cloudflare** a treba dbať na to, či pracujeme s IP adresou skutočného používateľa alebo proxy servera. Pre mňa je najlepším spôsobom použitie funkcie `getIp()` uvedenej na začiatku tohto článku. Detekciu Cloudflare môžeme zabezpečiť overením existencie kľúča `$_SERVER['HTTP_CF_CONNECTING_IP']`, ktorý sa automaticky prenáša v každej proxovanej požiadavke.

Detekcia VPN / Proxy servera
-------------------

Neexistuje spoľahlivá detekcia používania proxy serverov alebo VPN, ale v reálnom prostredí môžeme odfiltrovať aspoň časť prevádzky.

Existuje niekoľko spôsobov, ako to urobiť: Vezmite rozsah IP adries a porovnajte IP adresu, z ktorej prišla požiadavka.

Od niektorých poskytovateľov VPN sú zoznamy IP adries dostupné neoficiálne (pozri napr. https://gist.github.com/JamoCA/eedaf4f7cce1cb0aeb5c1039af35f0b7), v prípade výstupných uzlov Tor oficiálne (https://blog.torproject.org/changes-tor-exit-list-service, ale mosty Tor tam nie sú).

Ďalšou možnosťou je podať niekde online žiadosť, čo môže spôsobiť oneskorenie načítania stránky, ak služba nefunguje, a tiež "únik" IP adries návštevníkov tretej strane. Od roku 2023 by som tento prístup dôrazne neodporúčal, pretože sa začína viac zaoberať ochranou a manipuláciou s údajmi používateľov.

Toto online dopytovanie môže byť "naivné" a stačí zistiť, kto vlastní rozsah alebo či ide o proxy/VPN (niektoré služby to môžu vrátiť, ale štandardne to nie je súčasťou "informácií o IP", napr. zo služby whois).

(Najčastejšie) sa používa určitý druh hodnotenia reputácie, kde niektoré IP adresy "odpadávajú" viac ako iné. Podľa štatistík prichádza viac svinstva z rôznych proxy serverov, VPN a Tor ako z domácich IP adries (snáď okrem "infikovaných" domácich IP adries). Takéto posúdenie reputácie ponúkajú niektoré zoznamy blokovania DNS, pozri niektorý náhodný zoznam, https://en.m.wikipedia.org/wiki/Comparison_of_DNS_blacklists a stĺpec "Cieľ výpisu", alebo ho poskytujú priamo spoločnosti ako Cloudflare vo forme "správy botov" atď.

Veľa závisí od toho, aký je cieľ detekcie.

Ukladanie adries IP
------------------

Záleží na tom, akú IP adresu máte k dispozícii.

- IP adresa IPv4 môže byť uložená v 4 bajtoch, na čo sa používa funkcia `ip2long`,
- Pre IP adresu IPv6 však musíme použiť 16 bajtov a neexistuje žiadna konverzná funkcia.

Ak váš databázový server nepodporuje priamo dátový typ pre IP adresu, odporúčam ukladať IP adresu ako `varchar(39)`, pričom obe verzie sa zmestia do reťazca a budú čitateľné pre človeka.

> Pri ukladaní IP adresy zvážte, či má zmysel ukladať aj názov domény zistený funkciou `gethostbyaddr`. Názvy sa nedajú zistiť pri listovaní a vyhľadávaní, pretože to trvá veľmi dlho a časom sa môžu meniť.

Blokovanie IP adresy návštevníka
-----------------------------

Ideálnym riešením je vytvorenie zoznamu blokovaných IP adries a porovnanie tohto zoznamu s aktuálnou IP adresou pri každej požiadavke. Ak sa adresy zhodujú, žiadosť sa okamžite zastaví.

```php
$blackList = [
    'first-ip',
    'druha-ip',
];

if (\in_array(getIp(), $blackList, true) === true) {
    echo 'Bohužiaľ, vaša IP adresa je zablokovaná :-(';
    die; // Žiadosť o ukončenie
}
```

Príklad predpokladá implementáciu funkcie `getIp()` ako v príklade vyššie.

Výkonnejším riešením je kontrola výskytu indexu v poli:

```php
$blackList = [
    'first-ip' => true,
    'druha-ip' => true,
];

if (isset($blackList[getIp()]) === true) {
    echo 'Bohužiaľ, vaša IP adresa je zablokovaná :-(';
    die; // Žiadosť o ukončenie
}
```

IP adresa a názov servera
---------------------------------

IP adresa servera je zvyčajne uložená v poli `$_SERVER['SERVER_ADDR']` a jeho názov možno získať pomocou konštrukcie `gethostbyaddr($_SERVER['SERVER_ADDR'])`.

Ak sa však používa koncept `Ngnix -> Apache -> PHP` a `Ngnix` je v úlohe reverzného proxy servera, skutočná IP adresa servera sa nezobrazuje.

V tomto prípade možno názov servera nájsť v poli `$_SERVER['SERVER_NAME']` alebo pomocou funkcie `php_uname('n')`. [Oficiálna dokumentácia funkcie uname](https://www.php.net/manual/en/function.php-uname.php).

Na zistenie verejnej IP adresy servera môžeme použiť tento trik: `gethostbyname(php_uname('n'))`.
