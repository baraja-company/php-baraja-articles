Superglobálne premenné
======================

> id: bc2107ee-6551-4559-8c02-9cecdf98d33b
> slug:
> 	cs: superglobalni-promenna
> 	sk: superglobalne-premenne
> 
> publicationDate: '2019-11-01 09:29:46'
> mainCategoryId: '0e39aee9-2818-480c-8081-e0c2d039bb24'
> sourceContentHash: '792e217f1331f38ed0719d7abeb05e67'

Superglobálne premenné sa používajú na odovzdávanie globálneho stavu aplikácie a komunikácie HTTP.

Hlavnou výhodou týchto premenných je, že sú vždy a všade k dispozícii. V praxi ide o polia hodnôt, v ktorých pristupujeme ku konkrétnym informáciám pomocou indexu. V rôznych kontextoch sa dostupnosť kľúčov môže líšiť (vysvetlené nižšie).

Typy superglobálnych premenných
--------------------------------

Všetky superglobály v PHP sú polia a označujú sa znakom dolára, za ktorým nasleduje podčiarkovník (okrem `$GLOBALS`) a veľké písmená.

V `PHP 7` sú najmä tieto možnosti:

| Premenná | Popis |
|-------------|-------|
| `$_GET` | Parametre URL <a href="/methods-odesilani-dat">zasielané metódou GET</a>
| `$_POST` | Údaje formulára <a href="/methods-odesilani-dat">odoslané prostredníctvom POST</a>. Všimnite si, že <a href="/ajax-post">môže sa v ajaxe správať inak</a>.
| `$_REQUEST` | Údaje formulára odoslané ľubovoľnou metódou (`$_GET`, `$_POST` a `$_REQUEST`).
| `$_FILES` | Technické informácie o aktuálne nahraných súboroch, napríklad prostredníctvom konštrukcie `<input type="file">`
| `$_SERVER` | <a href="/info">Nastavenia webového servera</a>, IP adresa, konfigurácia... líši sa v závislosti od prostredia (pri volaní PHP skriptu z Terminálu bude obsahovať iné hodnoty a napríklad informácia o aktuálnej požiadavke bude chýbať).
| `$_COOKIE` | Nakonfigurované <a href="/cookies">cookies</a>.
| `$_SESSION` | Údaje relácie (<a href="/sessions">relácia</a>), ak existujú a boli nastavené v minulosti.
| `$GLOBALS` | **Upozornenie, neobsahuje podčiarkovník v názve!** Ide o takzvanú <a href="global-variable">global-variable</a> a alternatívny zápis pre kľúčové slovo `global`. Ak máte v aplikácii globálnu premennú `$variable`, môžete k nej pristupovať aj pomocou konštrukcie `$GLOBALS["variable"]`. Používanie globálnych premenných je však zlé a nečisté riešenie, takže to radšej nerobte.
| `$_ENV` | Informácie o aktuálnom prostredí, v ktorom je PHP spustené.

Zoznam všetkých existujúcich hodnôt je jednoduchý:

```php
foreach ($_SERVER as $key => $value {
	echo $key . ': ' . $value . '<br>';
}
```

> Poznámka: Nie všetky indexy musia vždy existovať (napríklad ak skript spustí cron v režime CLI, index s adresou URL stránky alebo IP adresou požiadavky nebude existovať).

Prístup k premenným
-------------------

Odporúčam, aby všetky globálne premenné (okrem `$_SESSION`) boli len na čítanie. Je to preto, lebo obsahujú globálne údaje aplikácie a iný kód ich môže brať do úvahy (napríklad iná nainštalovaná knižnica).

Ďalšou nevýhodou globálneho stavu je, že sa nemôžete vždy spoľahnúť na presné hodnoty, aj keď existujú, takže by ste mali vždy kontrolovať ich kľúče pomocou konštrukcie `isset()`.

Ak chcete uložiť nový súbor cookie, použite funkciu `setcookie()` a nevkladajte hodnotu priamo. Je to preto, že je určený len na čítanie.

Získané skúsenosti
-------

Nikdy slepo nedôverujte hodnotám superglobálnych premenných!

Používateľ môže pomocou adresy URL a odoslaných hlavičiek ovplyvniť spôsob nastavenia hodnôt. Všetky vstupy by sa mali vždy starostlivo overiť.

Register globals - problémy so starou verziou PHP
------------------------------------------

V starej verzii PHP (do verzie `5.4.0`) existovala špeciálna direktíva `register-globals` (nastaviteľná v súbore `php.ini`), ktorá spôsobila, že všetky odovzdané parametre v URL boli automaticky zaregistrované ako premenné.

Napríklad:

Používateľ prišiel na adresu URL: `https://example.com/script.php?var=24`

PHP automaticky vytvorilo v skripte premennú `$var` s hodnotou `24`.

Takže to fungovalo klasicky:

```php
echo $var;
```

Ktokoľvek by teda mohol do skriptu vložiť akúkoľvek premennú a zmeniť jej obsah. Samozrejme, bezpečnosť nebola vždy prioritou. Nie celkom.

Iné zdroje
------------

Podrobnejší popis nájdete v <a href="https://www.php.net/manual/en/language.variables.superglobals.php">oficiálnej príručke</a>.
