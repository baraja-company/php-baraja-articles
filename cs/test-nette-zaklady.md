Test základních Nette znalostí
==============================

> id: d137c177-503c-4e84-8709-0e65b0ce6060
> slug:
> 	cs: test-nette-zaklady
> 
> publicationDate: "2019-08-22 20:48:46"
> mainCategoryId: "28a2aef7-7490-43a9-add7-80d8a051f8a9"

Hranice úspěšnosti: 15 bodů

*Za každou správně zodpovězenou otázku získáš 1 bod. Za nesprávně zodpovězenou otázku nic. Pokud je odpověď jen částečná (a nebylo by na základě ní možné věc naprogramovat), tak se otázka počítá jako nesprávná (není možné získat polovinu bodu). Pokud řešení obsahuje bezpečnostní chybu, nebo chybu v kódu, nebo překlep v kódu, je odpověď považována za nesprávnou, protože by nešel spustit.*

-----------

1.	Vysvětli rozdíl mezi cyklem `for`, `while` a `foreach`. U každého uveď 1 konkrétní příklad využití, na kterém bude jasně vidět jeho hlavní výhoda.


2.	Máme proměnnou o které téměř nic nevíme (známe jen její název). Jak si můžeme prohlédnout její obsah? Jmenuje se například `$data`.


3.	Napiš pod sebe následující příkazy pro práci s Gitovým repositářem:
- Stažení aktuálních změn ze serveru
- označení souboru `Statistic.php`
- označení všech souborů v projektu
- označení všech souborů v adresáři `cron`
- commitnutí změn se zprávou "Můj commit"
- odeslání commitu na server


4.	Mějme v proměnné textový řetězec. Uveď příklad funkce, kterou vypočítáme kontrolní součet.


5.	Napiš úsek kódu, který v `Presenteru` vytvoří akci `delete`, která bude přijímat ID položky jako integer a bude mazat řádek z tabulky `question` podle zadaného ID. Po úspěšném smazání vypíše hlášku "Otázka byla smazána" a přesměruje na akci `list`.

Pod otázka za bod navíc: Pokud mazání z nějakého důvodu selže, tak nevyhodí fatální chybu, ale uživatele o tom informuje také hláškou (flash message).

6.	Když vytvářím Nette formulář, tak z něj je komponenta. Co je Nette komponenta?

7.	Potřebuji vytvořt jednoduchý Nette formulář pro vložení záznamu do tabulky `question`, která obsahuje seznam otázek. Struktura tabulky je:

| Sloupec   |            Vlastnosti            |
|-----------|----------------------------------|
| id        | int(8), unsigned, auto increment |
| question  | varchar(255)                     |
| is_active | tinyint(1), unsigned, defaultní hodnota: 1 |

Vytvoř vhodná formulářová pole pro vložení nového řádku do této tabulky. Po vložení záznamu musí dojít k vyhození FlashMessage informující o úspěšném vložení záznamu + přesměrování na editaci záznamu (akce `edit`).

- Validace, že bylo formulářové pole vyplněno
- Validace, že se text otázk vejde do varcharu podle struktury tabulky
- Validace, že otázka s tímto textem již neexistuje
- Definuj si další vlastní tabulku `group`, která bude obsahovat informace o skupinách. Při zakládání otázky pak půjde určit, do jaké skupiny otázka patří. Mezi tabulkami bude nutné nastavit relaci (popiš, jak se to dělá a jak bude nastavená).
- Jakým Latte makrem vykreslím formulář do šablony (default render)?

8.	Mějme editační formulář v `Presenteru`, který je založený jako komponenta. Chceme do něj předat defaultní hodnoty z toho, co je v databázi, tj. data musíme nějakým vhodným způsobem získat z tabulky.
- Jakým způsobem budeš postupovat a jaké prvky v kódu potřebujeme?
- Jak do formuláře předáš indentifikátor aktuálně editované položky?
- Jak se formulářovému prvku nastavuje defaultní hodnota?
- Jakým způsobem můžeme ověřit, že se uživatel snaží editovat položku, která v databázi neexistuje a jak ho o tom můžeme vhodně informovat?

9.	Mějme následující data získaná z databáze (využívá běžnou Nette Database):

```php
$questions = $this->db->questions()->fetchAll();
```

- Jakým způsobem vypíšeme texty všech otázek jako odrážkový seznam?
- Jak předáme data z tabulky do Latte šablony?
- Jaká Latte makra budeme potřebovat pro výpis položek? Uveď konkrétní implementaci vypsání sloupců `id` a `name` ve formátu:

	*1024: Jak se máte?*
	*1025: Co jste měli dnes k obědu?*

10.	Vyjmenuj příklad aspoň 3 různých formulářových polí, které se zapisují ve tvaru:

```php
$form->add(tady bude příklad);
```

a ke každému vysvětli, k čemu slouží a jaký vrací výstup (datový typ + příklad).


11.	Mějme odeslaný Nette formulář.
- Jak získáme všechna odeslaná pole (názvy a hodnoty)?
- Uveď příklad vypsání pole s názvem `question`.
- Uveď konkrétní implementaci kódu, který projde pole hodnot a klíčů a vrátí jeden string, ve kterém bude celkový soupis klíčů a hodnot, abychom mohli tento string například uložit do databáze, nebo poslat mailem (ukládání a odeslání není předmětem zadání a nebude hodnoceno).


12.	O každé podmínce rozhodni, jestli je výsledkem TRUE nebo FALSE:
- `1 > 0`
- `1 == 1`
- `1 == "1"`
- `1 === "1"`
- `1 == true`
- `1 === true`
- `1 === false`
- `1 == "1" && 1=== true`


13.	Jaké známe datové typy v PHP?
- Uveď aspoň 5 příkladů názvů datových typů
- U každého příkladu uveď aspoň 3 možné hodnoty, které mohou být v datovém typu uloženy (pokud do datového typu nelze tolik možných hodnot uložit, napiš to)
- U každého datového typu uveď typický příklad využití (co se do něj v praxi ukládá)
- Jaký je rozdíl v podmínce mezi `==` (dvě rovnítka) a `===` (tři rovnítka)?
- Vysvětli nevýhodu použití `==` v podmínkách a jak konkrétně `===` tento problém řeší (příklad, kde může `==` selhat a `===` zachrání situaci)


14.	Mějme tabulku koordinací (tabulka coordinations), která obsahuje seznam všech koordinací mezi 2 lidmi. Jedem z nich koordinaci pořádá a druhý je hostem. Napiš databázovou selection, která vrátí všechny řádky s koordinacemi, které se mě týkají (jsem organizátorem koordinace, nebo jsem hostem koordinace). Tabulka má sloupce `id`, `id_user_organizer` (ID organizátora), `id_user_quest` (ID hosta). Moje ID je uloženo běžným způsobem v `Presenteru`.


15.	Skupina otázek o Latte:
- Co je Latte?
- Jaký je rozdíl mezi `proměnnou`, `makrem`, `filtrem` a `n:atributem`? Co se používá kde?
- Jak vytvořím odkaz na `DashboardPresenter` na akci `default`?
- Vypisujeme tabulku otázek, jak vygeneruji odkaz na konkrétní editaci (`QuestionPresenter`, akce `edit`) otázky tak, abych předal ID aktuálně vypisované otázky? Napiš konkrétní Latte kód.

Symbolicky zapsáno (ukázka v PHP, přelož do Latte):

```php
foreach ($questions as $question) {
   echo $question->id; // ID otázky
   echo $question->question; // text otázky
}
```

- Jak se zapisuje pevná HTML mezera?


16.	K čemu slouží v Nette služby (services)?
- Jak se vytváří její instance?
- Co musí služba splňovat, aby ji bylo možné používat?
- Jak službu načtu do Presenteru?
- Mějme například službu `StatisticManager`, která má veřejnou metodu `getStatistics()`, která nepřijímá žádné parametry. Jak tuto službu načtu v Presenteru a v akci default zavolám veřejnou metodu `getStatistics()` a výsledek předám do šablony?
- Jaký je rozdíl mezi `objektem`, `třídou`, `službou`?
- Co je `model`, `entita` a `value object`?
- Všechny služby mají jednoho hlavního správce, který je zná a lze si je v něm vyzvednout. Jak se tento správce jmenuje? Jak do něj novou službu zaregistruji?


17.	Neon
- Co jsou Neon soubory?
- Jaké existují druhy a podle čeho se třídí?
- Co obsahují Neon soubory? Jaká data se do nich ukládají?
- Uveď příklad registrace následujího pole (předpis uvádím v PHP, bude jej nutné přeložit) tak, aby jej bylo možné použít jako parametr:

```php
$imageGenerator = [
   "points" => [
      480: [910, 30, 1845, 1150],
      600: [875, 95, 1710, 910],
      768: [975, 130, 1743, 660]
   ]
];
```

- Uveď příklad registrace služby v Neonu, které předáme parametr `imageGenerator`, který jsme registrovali v předchozím úkolu, aby jej služba dostávala v konstruktoru a bylo jej možné ve službě použít (ve smyslu konfigurace). U servisy uveď vzorovou implementaci konstruktoru tak, aby bylo ošetřeno, že prvním vstupním parametrem je datový typ pro pole.


18.	Objekty obecně
- Co je `metoda`, `properties` a `konstanta`? Jaký je mezi tím rozdíl?
- Metody i properties mají 3 základní stavy přístupnosti (`public`, `private`, `protected`), vysvětli rozdíl a konkrétní příklad využití a kdo může co a kdy vidět.
- Mám třídu `course` ve které je privátní property `currentCourse`, ve které je uložen aktuální kurz. Jak zjistit, aby bylo možné property z venku pouze číst a ne zapisovat?


19.	Když zakládám tabulky v databázi, které spolu logicky souvisí (třeba tabulku pro uživatele a pak tabulku jeho článků), tak potřebuji ošetřit, že budou data správně provázána.
- Co zajišťuje v databázi korektní provázání dat v rámci tabulek?
- Co je referenční integrita a jaká je její role v databázi?
- Jaké máme druhy relací? K čemu každý druh slouží?
- Jaké parametry nastavujeme relacím, abychom různým způsobem ošetřili smazání nebo změnu dat? Uveď 3 příklady a konkrétní využití + popis, jak funguje.


20.	K čemu slouží továrny (`OOP design pattern`)?
- Uveď příklad využití.
- Jsou Nette služby továrny?
- K čemu slouží injectování závislostí?
- Jaký je rozdíl mezi `DI` a `DIC`?
