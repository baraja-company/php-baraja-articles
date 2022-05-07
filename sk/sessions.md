Relácie - súbory cookie servera v jazyku PHP
============================================

> id: ab782ff0-d5ba-4115-9ebb-ab37978a6527
> slug:
> 	cs: sessions
> 	sk: relacie---subory-cookie-servera-v-jazyku-php
> 
> publicationDate: '2019-11-06 17:06:18'
> mainCategoryId: '2a1ef8bc-14aa-438a-87e7-5b3f9643f325'
> sourceContentHash: '3c84a09e5d180f6a47a58bea70b7ab21'

Často potrebujeme uložiť viac informácií do <a href="/cookies">cookies</a>, ale maximálny limit pre cookies je 4 kB, čo nie je veľa. Relácie tento problém riešia tak, že údaje ukladajú na webovom serveri a v prehliadači klienta ukladajú len krátky identifikátor, ktorý určuje, ktoré údaje patria ktorému klientovi.

Spustenie relácie
---------------------

Predtým, ako začneme pracovať s reláciami, musíme ich najprv začať. To sa vykoná zavolaním funkcie `session_start()` hneď na začiatku skriptu:

```php
session_start();
```

> Dôrazné upozornenie: pred volaním funkcie `session_start()` sa nesmie vykonať žiadny výstup do HTML kódu!

Zabezpečenie relácie
-------------------

Obsah relácie je uložený na serveri a do klientskeho prehliadača sa posiela iba identifikátor, takže používateľ nemá možnosť zistiť, čo je v relácii uložené. Jediný spôsob, ako môže skript ovplyvniť používateľa, je vymazanie identifikátora (po ktorom skript vygeneruje nový).

Získavanie údajov z relácie
----------------------

Všetky relácie sú uložené v superglobálnej premennej `$_SESSION` a možno ich prechádzať ako pole.

Napríklad meno aktuálne prihláseného používateľa možno získať zápisom:

```php
echo $_SESSION['používateľ'];
```

Poznámka: Relácia nemusí vždy existovať (napríklad ak ste nový používateľ). Preto by sme mali vždy pred každým výpisom skontrolovať, či existuje, a v prípade potreby ponúknuť alternatívne chybové hlásenie.

```php
if (isset($_SESSION['používateľ']) && $_SESSION['používateľ']) {
    echo 'Prihlásený používateľ:' . $_SESSION['používateľ'];
} else {
    echo 'Nikto nie je prihlásený.';
}
```

Ukladanie údajov do relácie
----------------------

Ukladanie sa vykonáva ako jednoduché uloženie údajov do premennej:

```php
$_SESSION['používateľ'] = 'Honzik';
```

Webový server sa postará o technické zabezpečenie správneho uloženia na serveri a odoslanie identifikátora používateľovi.

Odstránenie relácií
----------------

Jednotlivé hodnoty možno vymazať samostatne podľa kľúča:

```php
unset($_SESSION['používateľ']);
```

Prípadne všetky dostupné relácie:

```php
unset($_SESSION);
```

> Poznámka: Odstránením konkrétnej relácie sa nevyprázdni hodnota kľúča, ale kľúč sa úplne odstráni. Preto sa pri pokuse o načítanie neexistujúceho kľúča vyhodí varovanie o chybe. Existenciu kľúča môžeme vždy ľahko overiť pomocou funkcie `isset()`.

Maximálna platnosť relácie
---------------------------------

Každá uložená relácia má limit, ako dlho bude uložená na serveri. PHP priamo obsahuje skript cron, ktorý pravidelne odstraňuje staré relácie.

Predvolená hodnota je zvyčajne `1440 sekúnd`, čo je `24 minút`.

Zvýšenie hodnoty je potrebné vykonať na 2 miestach:

- V <a href="/info">`php.ini` je nastavená maximálna dĺžka platnosti, ktorú bude server udržiavať</a>. Hodnota je nastavená smernicou `session.gc_maxlifetime`,
- Na strane skriptu PHP je potrebné zadať aktuálnu požadovanú platnosť.

Použitie v PHP:

```php
// server teraz udrží reláciu až 3600 sekúnd = 1 hodina
ini_set('session.gc_maxlifetime', '3600');

// všetci klienti (prehliadače) budú
// relácia odoslaná s platnosťou presne 3600 sekúnd
session_set_cookie_params(3600);

session_start(); // môžeme začať reláciu!
```
