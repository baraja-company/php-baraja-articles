Návrhové vzory v PHP
====================

> id: c22b3e54-bb66-4196-a998-f4b62b9308b2
> slugCS: navrhove-vzory
> publicationDate: "2020-02-13 10:32:29"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

Návrhové vzory (design pattern) jsou způsoby, jak přemýšlet o programování.

Přináší sbírku rad, hotových postupů, best-practices a pohledů na vývoj. Pro každé programové paradigma a typ úlohy existují určité návrhové vzory, které se nejlépe hodí.

Přínosy - proč používat návrhové vzory?
---------------------------------------

V programování se řešení určitých typů úloh neustále opakuje, proto dává smysl zvolit jednu metodu, jak tyto úlohy řešit a tu stále opakovat.

Hlavní přínos vzniká zejména při vývoji v týmu, kdy všichni vědí, jakým způsobem se bude aplikace vyvíjet (podle jakého návrhového vzoru) a ten pouze aplikují. Tímto následně odpadají zbytečné desítky hodin debugování podivně napsaného kódu a snahy pochopit principy, které autor zamýšlel.

MVC - příklad použití návrhového vzoru
--------------------------------------

Můj oblíbený návrhový vzor je `MVC` (z anglických slov `Model View Controller`), který říká, že se aplikace dělí na 3 nezávislé vrstvy, které se volají postupně a předávají si data.

Například při vykreslování stránky to může vypadat tak, že se nejprve rozhodne, o jaký typ stránky jde (například detail kategorie), proto se zavolá `CategoryController` s metodou `detail`.

Konkrétní příklad (hodně zjednodušuji):

```php
class CategoryController
{

    /**
     * @var CategoryManager
     */
    public $categoryManager;

    public function actionDetail(string $id): void
    {
        $this->template->id = $id;
        $this->template->category = $this->categoryManager->getById($id);
    }

}
```

> **Poznámka:**
>
> Jde pouze o ukázkový kód, který vysvětluje princip návrhového vzoru `MVC`.
>
> V reálné implementaci bychom museli dále dořešit, jak například získat instanci `CategoryManageru` a jak ho předat do property. Typicky se pro tento typ úlohy používá `Dependency injection`.

Ještě před vykreslením stránky pro detail kategorie se nejprve zavolá `CategoryController`, který má za úkol přijmout aktuální požadavek (tedy, že vykreslujeme detail kategorie s určitým ID, který získá router třeba v URL), získá data (zeptá se na ně odpovídajícího `Modelu`) a finální data předá do šablony pro vykreslení.

Obrovská výhoda tohoto principu je zejména v tom, že můžeme napsat mnoho modelů (aplikační logiky), která je nezávislá na způsobu prezentování dat (šabloně) a vzniká tím znovupoužitelný kód. Pokud budeme totiž chtít `CategoryManager` použít v jiném projektu, jednoduše přes `Controller` předáme specifickým způsobem data, která se vykreslí podle šablony, který definuje sám projekt, zatímco aplikační logika zůstane stejná a ničemu to nevadí, protože softwarová vrstva splnila své domluvené rozhraní a zodpovědnost.

> **Praktická poznámka:**
>
> Návrhový vzor `MVC` používá většina moderních frameworků, jako je například Nette, Symfony, Laravel a další.
>
> S `MVC` se můžeme setkat i při vývoji mobilních aplikací a dalších typů softwaru, kde potřebujeme získat data a ta vykreslit v šabloně podle typu stránky nebo pohledu.

Důležité návrhové vzory pro tvorbu webu
---------------------------------------

Obecně v programování existuje mnoho návrhových vzorů, které se pro vývoj webů nehodí. Tento seznam popisuje nejdůležitější vzory, které sám používám a měli byste je znát.

<a href="/kategorie-navrhove-vzory">Kompletní přehled všech návrhových vzorů</a>, příklady jejich použití a podrobné vysvětlení najdete na samostatné stránce.

- **MVC** - Princip rozdělení aplikace na `Model` (aplikační logika a data), `View` (šablona a pohled na data) a `Controller` (propojení `Modelu` a `View`).
- **Dependency injection** - Místo vytváření instancí tříd definujeme tzv. služby, které si necháme automaticky vkládat. Kód přiznává veškeré závislosti, jednotlivé části lze vyměnit a aplikaci sestavujeme dynamicky.
- **Singleton (Jedináček)** - Každá třída má jen jednu instanci (osobně používám v kombinaci s `Dependency injection`, kdy má každá služba jen jednu instanci, která se předává napříč aplikací).
- **Lazy Initialization (Odložená inicializace)** - Instanci objektu vytvoříme až v okamžiku, kdy je poprvé potřeba.
- **Adapter** - Pokud potřebujeme zajistit komunikaci dvou nekompatibilních tříd, tak `Adapter` převádí data z jednoho typu na druhý (typicky převádíme nativní PHP datové typy na databázové a zase zpět).
- **Command (Příkaz)** - Místo vykonání konkrétní úlohy rovnou (například odeslání e-mailu) si pouze zapamatujeme, že se to mělo stát. Jiný nebo stejný proces poté požadavek vyzvedne a zpracuje. Hlavní výhoda je zejména v tom, že můžeme aplikaci zpracovávat lazy (a tedy velmi zrychlit), opakovat nezdařené akce a jednoduše ukládat parametry volání. Zároveň tímto umožníme přijímat požadavky od různých klientů, přes API a podobně. Odeslané akce lze také před vyhodnocením vložit do fronty, prioritizovat a případně zrušit.
- **Strategy (Strategie)** – Zapouzdřuje nějaký druh algoritmů nebo objektů, které se mají měnit, tak aby byly pro klienta zaměnitelné.

Návrhových vzorů existuje ještě celá řada, toto byly ty nejdůležitější, které byste měli znát.

Anti-pattern
------------

Některé programátorské techniky vývoje se považují za tzv. `anti pattern`, což je přesný opak návrhového vzoru. Obvykle jde o techniku, při které vzniká podivný kód, který nelze jednoduše debugovat, udržovat a chová se "magicky".

Typickým příkladem je použití <a href="/globalni-promenna">Globálních proměnných</a>.
