Návrhové vzory v PHP
====================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slug:
> 	cs: navrhove-vzory
> 	sk: navrhove-vzory-v-php
> 
> publicationDate: '2020-02-13 10:32:29'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '9ee32f0a3ce55bad52725f45fcd1c041'

Návrhové vzory sú spôsoby myslenia o programovaní.

Poskytujú zbierku rád, hotových postupov, osvedčených postupov a náhľadov na vývoj. Pre každú programovú paradigmu a typ úlohy sú najvhodnejšie určité návrhové vzory.

Výhody - prečo používať návrhové vzory?
---------------------------------------

Pri programovaní sa riešenie určitých typov problémov opakuje, preto má zmysel vybrať si jednu metódu na riešenie týchto problémov a túto metódu opakovať.

Hlavný prínos vzniká najmä pri tímovom vývoji, keď všetci vedia, ako sa bude aplikácia vyvíjať (podľa akého návrhového vzoru), a jednoducho ho aplikujú. Vďaka tomu potom odpadajú zbytočné desiatky hodín ladenia podivne napísaného kódu a snahy pochopiť princípy, ktoré autor zamýšľal.

MVC - príklad použitia návrhového vzoru
--------------------------------------

Môj obľúbený návrhový vzor je `MVC` (z anglického `Model View Controller`), ktorý hovorí, že aplikácia je rozdelená na 3 nezávislé vrstvy, ktoré sa navzájom postupne volajú a odovzdávajú si údaje.

Napríklad pri vykresľovaní stránky to môže vyzerať tak, že sa najprv rozhodne, o aký typ stránky ide (napríklad o detail kategórie), takže zavolá `CategoryController` s metódou `detail`.

Konkrétny príklad (veľmi zjednodušujem):

```php
class CategoryController
{
    public CategoryManager $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }
}
```

> **Poznámka:**
>
> Toto je len ukážka kódu, ktorá vysvetľuje princíp návrhového vzoru `MVC`.
>
> V skutočnej implementácii by sme museli ďalej vymyslieť, ako napríklad získať inštanciu `CategoryManager` a ako ju odovzdať vlastnosti. Na tento typ úloh sa zvyčajne používa `Dependency injection`.

Pred vykreslením stránky pre detail kategórie sa najprv zavolá `CategoryController`, ktorý prijme aktuálnu požiadavku (t. j. vykreslíme detail kategórie s určitým ID, ktoré získa smerovač napríklad v adrese URL), získa údaje (dopytovaním sa na príslušný `Model`) a odovzdá konečné údaje šablóne na vykreslenie.

Obrovskou výhodou tohto princípu je, že môžeme napísať mnoho modelov (aplikačná logika), ktoré sú nezávislé od spôsobu prezentácie údajov (šablóna), čo vedie k opakovane použiteľnému kódu. V skutočnosti, ak chceme použiť `CategoryManager` v inom projekte, jednoducho odovzdáme dáta cez `Controller` špecifickým spôsobom, ktorý sa vykreslí podľa šablóny definovanej samotným projektom, pričom aplikačná logika zostáva rovnaká a nikoho to nezaujíma, pretože softvérová vrstva splnila svoje dohodnuté rozhranie a povinnosti.

> **Praktická poznámka:**
>
> Návrhový vzor `MVC` používa väčšina moderných frameworkov, ako sú Nette, Symfony, Laravel a ďalšie.
>
> S `MVC` sa môžeme stretnúť aj pri vývoji mobilných aplikácií a iných typov softvéru, kde potrebujeme získať údaje a vykresliť ich do šablóny podľa typu stránky alebo zobrazenia.

Dôležité návrhové vzory pre vývoj webových stránok
---------------------------------------

Vo všeobecnosti v programovaní existuje mnoho návrhových vzorov, ktoré nie sú vhodné pre vývoj webových stránok. Tento zoznam opisuje najdôležitejšie vzory, ktoré sám používam a ktoré by ste mali poznať.

Kompletný zoznam všetkých návrhových vzorov</a>, príklady ich použitia a podrobné vysvetlenia nájdete <a href="/categories-design-patterns">na samostatnej stránke.

- **MVC** - Princíp rozdelenia aplikácie na `Model` (aplikačná logika a dáta), `View` (šablóna a zobrazenie dát) a `Controller` (prepojenie `Modelu` a `View`).
- **Dependency injection** - Namiesto vytvárania inštancií tried definujeme tzv. služby, ktoré sme automaticky injektovali. Kód pripúšťa všetky závislosti, jednotlivé časti sa dajú vymeniť a aplikáciu zostavujeme dynamicky.
- **Singleton (Singleton)** - Každá trieda má len jednu inštanciu (osobne to používam v kombinácii s `Dependency injection`, kde má každá služba len jednu inštanciu, ktorá sa odovzdáva v rámci celej aplikácie).
- **Lazy Initialization (Oneskorená inicializácia)** - inštanciu objektu vytvoríme, keď je prvýkrát potrebná.
- **Adaptér** - Ak potrebujeme zabezpečiť komunikáciu medzi dvoma nekompatibilnými triedami, `Adaptér` konvertuje údaje z jedného typu na druhý (typicky konvertuje natívne dátové typy PHP na databázové dátové typy a späť).
- **Príkaz** - Namiesto priameho vykonania konkrétnej úlohy (napríklad odoslania e-mailu) si len zapamätáme, že sa to malo stať. Ďalší alebo ten istý proces potom prevezme požiadavku a spracuje ju. Hlavnou výhodou je, že aplikáciu môžeme spracovať lenivo (a teda veľmi rýchlo), opakovať neúspešné akcie a jednoducho uložiť parametre volania. Zároveň nám to umožní prijímať požiadavky od rôznych klientov prostredníctvom rozhraní API atď. Odoslané akcie možno tiež zaradiť do frontu, určiť ich prioritu a prípadne ich zrušiť pred vyhodnotením.
- **Stratégia** - Zapuzdruje určitý druh algoritmov alebo objektov, ktoré sa majú zmeniť tak, aby boli pre klienta zameniteľné.

Návrhových vzorov je oveľa viac, tieto boli najdôležitejšie, ktoré by ste mali poznať.

Anti-vzor
------------

Niektoré techniky programovania sa považujú za "anti-vzor", čo je presný opak návrhového vzoru. Zvyčajne ide o techniku, ktorá vytvára zvláštny kód, ktorý sa nedá ľahko odladiť, udržiavať a správa sa "magicky".

Typickým príkladom je použitie <a href="/global-variable">globálnych premenných</a>.
