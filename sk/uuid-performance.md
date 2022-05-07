UUID a výkonnosť rozsiahlych aplikácií
======================================

> id: '2f072ce8-13b1-41f6-b328-2bd3b416cdd2'
> slug:
> 	cs: uuid-performance
> 	sk: uuid-a-vykonnost-rozsiahlych-aplikacii
> 
> publicationDate: '2019-11-08 10:09:54'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: '3b6d0e37684aedc0aedd92f2654e3626'

Keď veľkosť databázy presiahne milióny riadkov, je vhodné začať škálovať aplikáciu a rozdeliť databázu na viacero fyzických serverov.

Najväčším problémom rozdelenia databázy na viac častí je jej následná synchronizácia, keď používateľ požaduje konkrétne údaje.

Prečo používať UUID a aké sú jeho výhody oproti automatickému zvyšovaniu
--------------------------------------------------------

Predpokladajme, že máte tabuľku `článkov`, ale keďže máte obrovskú stránku, sú tam desiatky miliónov článkov a musíte ich fyzicky rozdeliť na viacero počítačov.

Ak by sme ako `id` (primárny kľúč) použili obyčajné celé číslo s nastavením ako autoincrement, veľmi rýchlo by sme zistili, že pri decentralizovanom vytváraní záznamov na rôznych počítačoch a ich následnej synchronizácii dochádza ku kolíziám ID a musíme záznamy komplikovane prečíslovať. Navyše, ak riešime mnoho relácií do iných tabuliek, môže to byť veľmi zložitá réžia, pri ktorej sa ľahko dopustíme chýb.

Preto namiesto číselného identifikátora môžeme generovať `UUID`, čo je textový reťazec generovaný zložitým algoritmom, ktorý zaručuje, že bude jedinečný, aj keď sa generuje nezávisle na viacerých počítačoch.

Výhody:

- Ak máte viacero nezávislých databáz, ktoré potom synchronizujete, použitie identifikátora UUID znamená, že jeden identifikátor je jedinečný vo všetkých databázach, nielen v tej, v ktorej sa nachádzate a kde bol vygenerovaný. Po zlúčení do jedného klastra nevznikajú žiadne konflikty.
- Svoj `primárny kľúč` môžete poznať ešte pred samotným vložením záznamu do databázy. Tým sa zníži počet dotazov SQL, zjednoduší sa logika transakcií a môžete ho ľahko použiť ako cudzí kľúč pred existenciou kolekcie záznamov.
- UUID nezverejňuje informácie o počte dátumov a sekvencií a je bezpečnejší na použitie v URL. Ak napríklad zistím, že som používateľ `19010018`, je ľahké odhadnúť, že existuje aj používateľ `19010017` a ďalší. Tento útok sa nazýva vektorový útok.

Generovanie nového UUID
----------------------

UUID možno získať buď jednoduchým dotazom SQL `SELECT UUID();`, ale tým sa zvýši počet dotazov do databázy a stratíme možnosť najprv hromadne pripraviť údaje v logike aplikácie a potom ich zapísať naraz.

Preto ako dobré riešenie rád používam balík <a href="https://github.com/ramsey/uuid">ramsey/uuid</a> získaný programom Composer. Samotný UUID má niekoľko verzií a balík môže hravo generovať všetky druhy podľa potreby.

Vďaka tomu sa ľahko používa:

```php
require 'vendor/autoload.php';

use Ramsey\Uuid\Uuid;

// Generuje objekt UUID verzie 1 (založený na čase)
$uuid1 = Uuid::uuid1();
echo $uuid1->toString() . "\n"; // e4eaaaf2-d142-11e1-b3e4-080027620cdd

// Generuje objekt UUID verzie 3 (založený na mene a hashovaný ako MD5)
$uuid3 = Uuid::uuid3(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid3->toString() . "\n"; // 11a38b9a-b3da-360f-9353-a5a725514269

// Generuje objekt UUID verzie 4 (náhodný)
$uuid4 = Uuid::uuid4();
echo $uuid4->toString() . "\n"; // 25769c6c-d34d-4bfe-ba98-e0ee856f3e7a

// Generuje objekt UUID verzie 5 (založený na mene a hashovaný ako SHA1)
$uuid5 = Uuid::uuid5(Uuid::NAMESPACE_DNS, 'php.net');
echo $uuid5->toString() . "\n"; // c4a760a8-dbcf-5254-a0d9-6a4474bd1b62
```

Ak používate Doctrine, existuje rozšírenie <a href="https://github.com/ramsey/uuid-doctrine">ramsey/uuid-doctrine</a>, ktoré generuje ID priamo ako dátový typ.

Fyzické ukladanie v databáze
---------------------------

Pri mojich prvých pokusoch som použil `varchar(36)` ako primárny kľúč (ID), ale <a href="https://www.facebook.com/groups/backendisti/permalink/2465260887049808/">to vôbec nie je dobrý nápad</a>.

> **Vysvetlenie vnútornej logiky:**
>
> > Databázy MySql (a mnohé iné) nemôžu efektívne používať ako primárny kľúč typy `varchar`, `char` alebo iné dátové typy vyjadrujúce reťazec.
> V niektorých databázach existuje dátový typ `GUID`, ktorý je určený na priame ukladanie UUID. Ak nemôžete použiť tento typ, existuje vhodná náhrada v tvare `binary(16)`.

Pri fyzickom skúmaní databázy je potom ID reprezentované vo formáte HEX (pretože binárny formát sa nedá zobraziť), namiesto pekného ID `726c67c4-e5eb-4a4c-8fcc-031da5d6f3c6` sa zobrazí len `726C67C4E5EB4A4C8FCC031DA5D6F3C6`, čo v dotaze INSERT vyzerá ako `'?kYߟKg2c;'`.

Konverzia pôvodných údajov z `varchar(36)` na `binary(16)`
----------------------------------------------------

Predpokladám, že novo nastavené ID reprezentujete (alebo plánujete reprezentovať) v databáze ako:

`id` binary(16) NOT NULL

Samotná zmena dátového typu však nefunguje, takže niečo ako:

SET FOREIGN_KEY_CHECKS=0;

ALTER TABLE article CHANGE id id BINARY(16) NOT NULL

SET FOREIGN_KEY_CHECKS=1;

Existujú v podstate dva dôvody:

- Primárny kľúč a relácia k nemu `musia mať rovnaký dátový typ`. Preto je potrebné zmeniť typ údajov pre ID článku a napríklad aj v relačnej tabuľke, ktorá priraďuje články k autorom.
- Binárny formát obsahuje niečo trochu iné ako pôvodný reťazec. Musíte použiť konverznú funkciu.

Jediným správnym riešením je preto zálohovať údaje (ale to by ste mali urobiť pred každou migráciou), pripraviť prázdnu databázu s funkčnými vzťahmi a údaje do nej opäť vložiť prostredníctvom migrácie.

Ak ste predtým vygenerovali UUID podivne, je lepšie zvoliť nejakú sekvenčnú metódu na získanie UUID a prečíslovať všetky záznamy. Dôvodom je, že sekvenčné usporiadanie umožňuje lepšie zoradenie hodnôt a vytvorenie `btree`, vďaka čomu je výkon takmer identický s `bigint`.

**Ak viete o lepšom spôsobe konverzie existujúcej databázy z UUID uloženého ako varchar do binárneho formátu bez nutnosti vymýšľať zložité migrácie a so zachovaním cudzích kľúčov, budem veľmi vďačný za spätnú väzbu**.
