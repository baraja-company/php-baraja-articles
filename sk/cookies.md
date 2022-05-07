Súbory cookie v jazyku PHP
==========================

> id: '392dd88b-d2f5-4943-a993-01aaad7ccd32'
> slug:
> 	cs: cookies
> 	sk: subory-cookie-v-jazyku-php
> 
> perex: 'Cookies je krátká textová informace v paměti prohlížeče, kam můžeme v PHP zapisovat a označit si uživatele.'
> publicationDate: '2019-09-11 10:18:29'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '78f9aad0278acd51b98179e7d29fe3d8'

> **Upozornenie:** Tento článok bol napísaný pred mnohými rokmi a niektoré informácie môžu byť zastarané alebo nesprávne. Majte to na pamäti pri čítaní.

Súbory cookie sú malé textové informácie uložené v prehliadači návštevníka webovej lokality. Prenášajú sa vždy s každou znovu načítanou stránkou a používateľ ich môže kedykoľvek vymazať, zmeniť a prečítať, takže nie sú vhodné na ukladanie osobných údajov.

*Upozornenie: ak vaša webová stránka používa súbory cookie na sledovanie používateľov alebo doplnkov tretích strán (napr. tlačidlo Facebook like, merač návštevnosti Google Analytics, reklamné bannery), musíte o tom používateľa informovať.*

> "Ešte jedna poznámka: vaša stránka by nemala obsahovať reklamné alebo meracie kódy, kým nezískate súhlas. A to je na nič."
>
> -- <a href="https://phpfashion.com/jak-na-souhlas-s-cookie-ve-zkurvene-eu">David Grudl</a>

Čítanie hodnoty zo súboru cookie
--------------------------

Všetky súbory cookie sú uložené v superglobálnej premennej `$_COOKIE`, ktorá ukladá každý kľúč ako pole.

Ak sme napríklad uložili meno aktuálne prihláseného používateľa pod kľúčom `user` v súbore cookie, môžeme ho ľahko získať:

```php
echo $_COOKIE['používateľ'];
```

Upozornenie: Súbory cookie nemusia vždy existovať (napríklad ak ste nový používateľ). Preto by sme mali pred každým výpisom vždy skontrolovať existenciu súborov cookie a v prípade potreby ponúknuť alternatívne chybové hlásenie.

```php
if (isset($_COOKIE['používateľ']) && $_COOKIE['používateľ']) {
    echo 'Prihlásený používateľ:' . $_COOKIE['používateľ'];
} else {
    echo 'Nikto nie je prihlásený.';
}
```

Získanie všetkých dostupných súborov cookie
--------------------------------

Keďže všetky súbory cookie sú uložené v superglobálnej premennej `$_COOKIE`, možno ich jednoducho vypísať:

```php
var_dump($_COOKIE);
```

Prípadne prejdite cyklus a získajte všetky kľúče a hodnoty:

```php
foreach($_COOKIE as $key => $value) {
    echo $key . ':' . $value;	// vypísať kľúč a hodnotu
    echo '<br>';				// zabalenie riadku
}
```

Uloženie hodnoty do súboru cookie
--------------------------

Funkcia `setcookie()` sa používa na ukladanie údajov do súborov cookie.

Prvým parametrom je kľúč cookie, ktorý sa použije na načítanie z poľa `$_COOKIE`, a druhým parametrom sú samotné údaje vo forme reťazca.

Pomocou tretieho parametra môžeme (voliteľne) nastaviť obdobie platnosti, počas ktorého bude súbor cookie k dispozícii. Čas dostupnosti je uvedený ako <a href="/date">timestamp</a>, takže ak chceme nastaviť cookie s platnosťou 1 hodinu od tohto momentu, stačí napísať `time() + 3600`.

```php
$data = 'Niektorý obsah chceme uložiť.';

setcookie('TestCookie', $data);
setcookie('TestCookie', $data, time() + 3600);
```

Ukladanie väčších údajov
-------------------

Súbory cookie nie sú vhodné na ukladanie väčších údajov (prehliadače zvyčajne umožňujú uložiť len 4 kB a maximálne 20 súborov cookie, pričom veľkosť zahŕňa aj názvy súborov cookie, nastavenia platnosti atď.).

Je lepšie ukladať väčšie údaje na server a do súboru cookie vložiť len identifikátor, podľa ktorého vieme určiť, ktorému používateľovi patria. Táto metóda sa nazýva `$_SESSION` a venujeme sa jej v samostatnom článku.

Ak nepotrebujete nevyhnutne ukladať údaje vždy synchrónne na server, môžete použiť **<a href="https://jecas.cz/localstorage">localstorage</a>** úložisko dostupné v javascripte. Jeho kapacita je rádovo MB a údaje nepodliehajú exspirácii.
