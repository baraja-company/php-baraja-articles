Nezmeniteľnosť objektov - kľúčový koncept návrhu
================================================

> id: '057467db-4e3b-4e18-9ea5-dfb25feb3800'
> slug:
> 	cs: immutabilita
> 	sk: nezmenitelnost-objektov-klucovy-koncept-navrhu
> 
> publicationDate: '2022-07-24 15:00:00'
> mainCategoryId: ae4c1c70-11b3-433e-b1d0-e590155bb8b9
> sourceContentHash: '88c26f35883c860426b4708f0be8761f'

Nemennosť je jedným z najdôležitejších konceptov návrhu pri vytváraní stabilných aplikácií. Základný princíp hovorí, že ak je stav raz zapísaný, možno ho neskôr len čítať bez možnosti jeho úpravy. Ak potrebujeme zmeniť stav, musíme vytvoriť novú inštanciu a nahradiť celý objekt iným.

Typy údajov možno preto veľmi zhruba rozdeliť do dvoch veľkých kategórií:

- **Mutable** (premenlivý stav v rámci jednej inštancie)
- **Immutable** (nemenný vnútorný stav)

Mutabilné objekty možno interne meniť. To znamená, že poskytujú operácie, ktorých volanie v rôznych kombináciách spôsobuje rôzne výsledky. Nemennosť sa snaží tomuto správaniu zabrániť.

Definícia
--------

> Trieda je **nezmeniteľná** práve vtedy, ak údaje inštancie nemožno po vytvorení inštancie nijako zmeniť.

Všetky údaje sú teda pevne stanovené v konštruktore. Všetky skalárne dátové typy sú tiež automaticky nemenné.

Hlavná výhoda
--------------

Navrhovanie aplikácií s nemennými stavmi poskytuje zásadnú výhodu v bezpečnosti vykonávania operácií. Ak vieme, že raz zapísané údaje nemožno neskôr zmeniť (mutovať), môžeme napríklad veľmi spoľahlivo ladiť alebo rozdeliť aplikáciu na čiastkové funkcie bez rizika, že zabudneme na niektorý z medzistavov.

Myšlienka nemennosti je vo všeobecnosti v rozpore so zásadou ukladania stavov vo vlastnostiach objektov/entít a opisuje skôr funkčný prístup, pri ktorom údaje jednoducho "pretekajú" aplikáciou, ako to robí napríklad javascript.

Z hľadiska výkonu môžeme o nemenných objektoch automaticky povedať, že ich možno ukladať do vyrovnávacej pamäte na neurčito, pretože nikdy nebudú neaktuálne.

Skutočný príklad z jazyka PHP
--------------------

Zďaleka najčastejšie používaným nemenným objektom v PHP je objekt `DateTimeImmutable`, ktorý sa po vytvorení môže volať len pomocou formátovacích metód. Ak zmeníme interné nastavenia, metóda vráti novú inštanciu. Táto funkcia je kľúčová pri používaní ORM, ktorý používa tzv. vzor identity - umožňuje nám napríklad zaručiť, že keď prečítame dátum vytvorenia objednávky, bude rovnaký všade v aplikácii a referenčná integrita nebude narušená.

Konkrétny príklad premenlivého objektu:

```php
$date = new DateTime('2021-05-14');
$tomorrow = $date->modify('+1 deň');
echo $date->format('Y-m-d'); // 2021-05-15
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Vypísal sa ten istý dátum, pretože metóda `modify()` zmenila len vnútorný stav objektu `DateTime` a vrátila tú istú inštanciu. Nedošlo teda k takzvanej **mutácii vnútorného stavu**, čo je základným správaním objektovo orientovaného programovania. Aktualizáciou premennej sa zmenila aj pôvodná premenná.

A teraz príklad nemenného objektu:

```php
$date = new DateTimeImmutable('2021-05-14');
$tomorrow = $date->modify('+1 deň');
echo $date->format('Y-m-d'); // 2021-05-14
echo $tomorrow->format('Y-m-d'); // 2021-05-15
```

Objekt `DateTimeImmutable` je nemenný, čo znamená, že jeho vnútorný stav sa nikdy nemení. Po zavolaní metódy `modify()` sa do premennej uloží nová modifikovaná inštancia (tiež nemenná). Ak by sme novú hodnotu neuložili do premennej, neskôr by sa nedala použiť.

Pôvodnej hodnoty sa nikdy nedotknete a zostane bezpečne uložená.

Kedy by mala byť trieda nemenná?
---------------------------

Ak nemáte veľmi dobrý dôvod na to, aby bola trieda alebo funkcia nemenná, vždy ju píšte ako nemennú. To vám v budúcnosti zjednoduší návrh.

Mutabilné triedy by sa mali meniť čo najmenej. Vždy odporúčam zdokumentovať správanie nemennosti.

Azda jedinou nevýhodou nemennosti je, že pri každej zmene atribútu sa musí vytvoriť nová inštancia, čo má menší vplyv na výkon. Ak vaša aplikácia (ako väčšina aplikácií) má tendenciu zobrazovať údaje a meniť ich menej často, táto nevýhoda je pri výkone dnešných počítačov pomerne zanedbateľná.

Aké typy údajov by mali byť nemenné?
------------------------------------

- Všetky identifikátory a jedinečné kódy
- Väčšina relácií databáz ManyToOne a OneToOne
- Dátumy, časy, hodnoty kalendára
- Cyklické prechádzanie polí, kde chceme vykonať rovnakú operáciu na každom prvku
- Intervaly, dvojice, trojice, ...
- Geometrické údaje, body, čiary, súradnice GPS, fyzické adresy, ...
- Denníky a historické záznamy
- Informácie o spracovaných objednávkach a väčšina finančných údajov
- Metaúdaje o súvisiacom subjekte

Čo by nemalo byť nemenné:

- Veľké objekty s množstvom vlastností
- Väčšina tabuľkových výstupov z databázy, ako sú entity Doctrine
- Postupne zostrojené objekty z menších častí
