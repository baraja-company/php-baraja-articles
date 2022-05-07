Osobitný význam úvodzoviek
==========================

> id: '2649942d-94d6-48c3-9c78-5a303165bf72'
> slug:
> 	cs: uvozovky-vyznam
> 	sk: osobitny-vyznam-uvodzoviek
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '6cbbbf59-9bbd-4ca3-a6c3-eb204a2f8070'
> sourceContentHash: c1b63b98a3e9d9c642247c363747616a

V PHP môžete často nahradiť úvodzovky apostrofmi a dosiahnuť rovnaký efekt. Niekedy dokonca zámerne používame kombináciu úvodzoviek a apostrofov, aby sme dosiahli iný výstup bez toho, aby sme museli používať escapovanie.

**TLDR: Používanie úvodzoviek môže byť v niektorých kontextoch nebezpečné, preto všade používajte apostrofy. Úvodzovky sú vhodné len v špeciálnych prípadoch.**

| Znak | Názov |
|------|-----------
| `"` | Úvodzovky |
| `'` | Apostrof |

Príklad jeden - rovnaký výstup
-----------------------------

```php
echo "Ahoj, svet!"; // toto je to isté
echo 'Ahoj, svet!'; // takto
```

V tomto prípade na použití úvodzoviek alebo apostrofov nezáleží. Na prvý pohľad sa môže zdať, že medzi úvodzovkami a apostrofmi nie je žiadny rozdiel.

Kombinácia úvodzoviek a apostrofov
------------------------------

Zoberme si nasledujúci kód:

```php
echo 'Moja obľúbená farba je "červená"!';
```

Ak by som na ohraničenie vypisovaného reťazca použil znaky `carriage returns`, bolo by nejednoznačné, kde reťazec **začína** a kde končí**, preto som na ohraničenie reťazca použil znaky `apostrof`, čo tento problém rieši.

Vloženie úvodzoviek do reťazca ohraničeného úvodzovkami
---------------------------------------------------

Občas sa môže vyskytnúť situácia, keď potrebujem v rámci toho istého reťazca uviesť aj `citát` aj `apostrofu`. Môžete použiť nielen spojenie dvoch reťazcov, ale aj takzvané **escapovanie** znakov.

```php
echo "Tento reťazec \"obsahuje\" úvodzovky.";
```

Spätné lomítko má v tomto prípade význam "presne tento znak", a preto sa úvodzovky nebudú považovať za koniec reťazca (ani za nič iné), a preto budú vždy úvodzovkami. Takto sa môžu označovať aj iné zvláštne znaky, pri ktorých nemožno zaručiť ich trvanlivosť a správny význam by mohol byť nesprávne pochopený.

Premenná v rámci reťazca
-----------------------

Uvádzacie značky môžu byť veľmi zložité, pretože umožňujú vkladať premenné priamo do reťazca:

```php
$color = 'Červená';

echo "Moje oblíbená barva je {$color}, a tvoje?";
```

Oveľa bezpečnejšie sú apostrofy, ktoré to neumožňujú a reťazec musíte preložiť:

```php
$color = 'Červená';

echo 'Moja obľúbená farba je' . $color . 'a tvoje?';
```

Dobré návyky
--------------------------

Vo všeobecnosti odporúčam na ohraničenie čohokoľvek používať apostrofy (ak je to možné), pretože sa v reťazcoch vyskytujú oveľa menej často ako len úvodzovky.

PHP je navyše webový jazyk, t. j. používa sa na generovanie dokumentov HTML, kde sú úvodzovky veľmi časté práve preto, že sa používajú aj na generovanie častí kódu HTML. Osobne odporúčam zvyknúť si všade striktne používať apostrofy, pretože potom si nemusíte pamätať, čo uzatvárate.

Správanie úvodzoviek
--------------------------

Dávajte si pozor! Nevyhadzujte úvodzovky úplne! Majú niektoré špeciálne výhody, ktoré môžu byť užitočné pre pokročilú prácu s PHP - začiatočníci ich však považujú za chyby a nerozumejú im.

```php
$x = 10; // nastaviť premennú
echo "Hodnota proměnné je: $x, přesně."; // a zoznam
```

V úvodzovkách môžete tiež uviesť hodnoty premenných alebo znak dolára spôsobí, že všetko, čo je za ním, je premenná. Ak teda nechcete vypisovať hodnotu premennej, ale znak dolára, musíte escapovať.

```php
$price = 25; // cena v dolároch
echo "Cena produktu: $price\$"; // vypíše "Cena výrobku: 25 USD"
```

Výhoda úvodzoviek je v tomto prípade otázna a možno by bolo lepšie použiť apostrofy a reťazce jednoducho prepojiť.

```php
$price = 25;
echo 'Cena výrobku:' . $price . '$'; // vypíše to isté ako predchádzajúci príklad
```

> **Poznámka:** Cena v dolároch je správne zapísaná vo formáte `$25`, čo by spôsobilo ešte väčší zmätok, pretože zápis `$$price` v skutočnosti volá niečo, čo sa nazýva `premenná` (v názve premennej voláme názov premennej, ktorá sa má volať - len zmätok).

**TIP:** Mohlo by vás zaujímať <a href="/promenna-premenna">premenná</a>.

Označenie reťazca
--------------------------

Vo všeobecnosti sa všetko, čo je v úvodzovkách alebo apostrofoch, považuje za reťazec. Takto:

```php
$x = 5;   // toto je niečo iné,
$x = "5"; // ako toto.
```

V prvom prípade je **číslo** 5 uložené v premennej **$x**, v druhom prípade je v tej istej premennej uložený **reťazec** "5". Našťastie to v PHP nevadí a s oboma variantmi môžete pracovať (takmer) rovnako, pretože PHP dokáže automaticky **transformovať** premenné podľa ich obsahu. Vo všeobecnosti sa však odporúča nepísať čísla v úvodzovkách, najmä pri výpočtových operáciách, pri ktorých môže dôjsť k chybám pri zaokrúhľovaní.

Niekedy môžem chcieť uložiť výstup funkcie do premennej:

```php
$pi = 3.14159;
$zaokrouhleni = round($pi); // toto je správne
$zaokrouhleni = "round($pi)"; // toto je "zlé" (otázka je, aký výstup očakávam).
```

Chyba v druhom prípade je práve v úvodzovkách, keď sa do premennej **$round** neukladá výstup funkcie **round()**, ale reťazec volajúci túto funkciu.
> **TIP:** Hodnotu `$pi` nemusíme takto priamo zadávať do skriptu a môžeme použiť funkciu `pi()`, ktorá je presnejšia pre zložitejšie výpočty.

Osobitný význam slova escaping
--------------------------

> **Upozornenie:** Nasledujúce príklady fungujú len v úvodzovkách, ak ich uzavriete do apostrofov, budú sa správať ako bežné znaky bez špeciálneho významu (okrem `\'`, ktorý sa apostrofu vyhne).
Escaping je určený na to, keď chcem vypísať nejaký špeciálny znak vnútri úvodzoviek alebo apostrof, ktorý by mohol byť interpretovaný ako jazykový výraz, a teda spracovaný, aj keď to programátor nezamýšľal. Príklad sme si už ukázali, v tejto časti sú opísané možné výnimky zo správania.

Je to preto, že niekedy má samotný útek zvláštny význam. Príklad:

```php
echo "Dlhý text rozdelený do dvoch riadkov.";
```

Predchádzajúci príklad vytlačí:

```php
Dlouhý text, rozdělený na
dva řádky.
```

Ak teda chceme vypísať lomítko, musíme ho tiež escapovať (escapovanie znaku `n` nie je v tomto prípade potrebné, pretože by sa opäť chápalo ako wrap, alebo v tomto prípade nesmieme escapovať vôbec):

```php
echo "Dlhý text rozdelený do dvoch riadkov.";
```

Takýchto špeciálnych znakov je viac, napríklad `\t` tvorí tabulátor. Úplný zoznam nájdete v oficiálnej dokumentácii.

Skutočné používanie špeciálnych znakov pri escapovaní
-----------------------------------------------

Na úvod by som chcel zdôrazniť, že v jazyku PHP sa dá takmer všetko urobiť viacerými spôsobmi a uvedené príklady slúžia len na ilustráciu, ako sa dá k problému pristupovať.

Ak chceme napríklad analyzovať text po riadkoch, môžeme použiť funkciu <a href="/explode">explode</a>.

```php
$parser = explode("\n", $retezec); // analyzuje text po riadkoch
```

V tomto prípade má použitie špeciálneho znaku `\n` zmysel, pretože môžeme veľmi efektívne povedať, že chceme analyzovať podľa zlomov riadkov.

```php
$parser = explode('
', $retezec);
```

> **Upozornenie:** V systémoch Unix a Windows sa znaky používané na označenie konca riadkov zamieňajú. Napríklad systém Windows používa `CRLF` (dvojicu znakov `\r\n`), zatiaľ čo Linux používa iba `LF` (jeden znak `\n`). Pri rozbore podľa riadkov by ste na to mali pamätať. Zvyčajne sa tento problém rieši normalizáciou znakov len na `LF`.

Zhrnutie
-------

Ak môžete, všade používajte **apostrofy**.

Je dobré poznať používanie úvodzoviek a používať ich len tam, kde je to nevyhnutné (alebo všeobecne dobré). Ak vypisujete text, ktorý môže obsahovať úvodzovky, uzavrite ho do apostrofov (ktoré sa potom správajú predvídateľnejšie). Osobne používam úvodzovky na vyjadrenie rôznych špeciálnych znakov, ktoré je zbytočne zložité zadávať v apostrofoch a vyžadovali by si zložité escapovanie.
