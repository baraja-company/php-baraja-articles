Úvod do objektově orientovaného programování v PHP
================================

> id: 44a79461-dcfd-4dd0-b6fd-ba5db76db6de
> slugCS: uvod-do-oop
> publicationDate: 2020-01-01 19:54:35
> mainCategoryId: 3e45c55a-a4cd-4745-b1bb-0332702fefbf

Vítejte v prvním článku online kurzu OOP v PHP. Kompletní seznam článků najdete na <a href="/oop">přehledové stránce</a>.

> **Autorské poznámky k obsahu:**
>
> Cílem této série je **co nejlépe vysvětlit podstatu** objektově orientovaného programování tak, abyste nemuseli trávit stovky hodin času experimentováním nad věcmi, co nedávají smysl. Veškeré postupy a návody jsou z praxe a jsou vysvětleny tak, jak bych si je chtěl sám přečíst v době, kdy jsem se učil programovat. Některé **koncepty jsou hodně zjednodušené** na úkor 100% faktické správnosti, protože věřím, že je vždycky **lepší rozumět hlavně principům** a mít aspoň nějakou představu o problematice, než se v programování ztrácet a vnímat všechno jako jeden velký chaos.
>
> Jak sami brzy uvidíte, programování v OOP má obrovské přínosy pro váš kód. Přinese vám novou úroveň abstrakce nad aplikací a umožní řešit velmi složité úlohy, které by jiným způsobem řešit vůbec nebylo možné.

Co jsou to objekty
------------------

V základním pojetí programování jsou objekty speciální datové typy, které kromě své vlastní hodnoty umí definovat podrobný stav, metody, chování a umožňují provádět různé operace, jako v reálném světě.

Objekt v programování je něco podobného, jak nějaká "věc" v reálném světě, kterou umíme pojmenovat (například podstatným jménem) a má vlastnosti, jako věc v reálném světě. Objektem může být například článek, který má nějaké hodnoty (název, obsah, autora, datum vydání, ...) a metody (vložení nového nadpisu, přečtení obsahu, výpočet počtu dnů od publikace, ...). V tomto článku se budeme zabývat zejména tím, jak objekt vytvořit a na základní úrovni s ním pracovat.

Objekty a třídy
---------------

Jak už jsme si řekli, tak **objekt je něco, co existuje**. V této fázi je velmi důležité to slovo `existuje`, protože jak si záhy ukážeme, tak v programování je potřeba řešit ještě praktickou implementaci.

Protože při programování píšeme kód a objekt je něco "živého", tak budeme od tohoto okamžiku rozlišovat dva různé pojmy, u kterých je důležité podrobně chápat význam a ten striktně rozlišovat:

- **Objekt** je něco "živého", co právě teď existuje (má instanci), může něco vykonávat nebo reagovat na jiné objekty.
- **Třída** je staticky napsaný kód, který se bude teprve vyhodnocovat a po celou dobu zůstává stejný.

Protože při programování vždy píšeme kód jako statický řetězec (text), tak budeme programy rozlišovat na `třídy` (statická definice celého objektu) a `objekty` ("živá" instance třídy).

Příklad definice třídy:

```php
class Article
{

    /** @var string */
    public $title;

    /** @var string|null */
    public $autor;
}
```

> **Praktické poznámky:**
>
> V reálné aplikaci se obvykle každá třída zapisuje do samostatného PHP souboru, který se jmenuje stejně jako název třídy.
>
> Pro třídu `Article` tedy vytvoříme například `src/Article.php`. Tato konvence není v PHP vyžadována, ale pomáhá lepší čitelnosti aplikace, abyste neměli mnoho kódu dohromady na jednom místě.
>
> Každá třída může v celé aplikaci existovat maximálně jednou (mít unikátní název), ale může existovat (téměř) nekonečně mnoho instancí (podle kapacity operační paměti). Jedna třída dále nemůže být rozdělena na více souborů a musí být definována najednou na jediném místě. Pokud potřebujete vytvořit více tříd se stejným názvem, použijte **jmenné prostory** (namespace).
>
> Později si také ukážeme, že dobře strukturovaný kód umožní jeho znovupoužití v rámci mnoha projektů.

Tento kód definuje třídu `Article` s dvěma property (`title` a `author`). Komentáře PHP ignoruje a slouží pouze pro statickou typovou kontrolu (typicky je čte PhpStorm).

Kód:

```php
/** @var string */
public $title;
```

znamená definici tzv. `properties`, což je vlastnost třídy `Article`. Můžeme si to představit třeba tak, že `Article` je druh pole a `title` je jeho index, do kterého můžeme zapisovat hodnotu. Každá property musí mít definovanou přístupnost (`public`, `protected` nebo `private`), později si vysvětlíme, co které nastavení znamená. Pokud nevíte, jakou přístupnost v konkrétním případě vybrat, dejte `public`.

Instance třídy a vytvoření objektu
----------------------------------

Pojem `instance` je jeden z nejsložitějších pojmů na pochopení, proto je důležité, abyste následující řádky četli velmi pozorně a doporučuji si všechny příklady poctivě vyzkoušet.

Pokud naše aplikace má již definovanou třídu ve zdrojovém kódu, tak se reálně nikde nepoužívá a PHP o ní neví. Je to z toho důvodu, že OOP zavádí tzv. `princip zapouzdření dat`, což znamená to, že třída má svůj vnitřní (lokální) kontext, který je platný právě jen uvnitř třídy. Při vývoji bez objektů jsme totiž byli zvyklí, že se kód vyhodnocoval vždy celý. Třídu si zkuste také představit jako určitou formu funkce nebo externě volaného programu.

Nyní můžeme vytvořit naši první instanci, k tomu použijeme nový operátor `new`.

```php
$myArticle = new Article;
$myArticle->title = 'Můj první článek'; // zapíše hodnotu

echo $myArticle->title; // vypíše "Můj první článek"
```

Všimněte si toho, že do proměnné `$myArticle` vkládáme celý objekt `Article`, který vznikl operátorem `new`. Toto pro nás znamená, že objekt je vlastně `datový typ`, proto ho můžeme přesouvat přes proměnné a dále s ním pracovat.

Operátor šipky `->` slouží pro manipulaci s objektem. Pokud máme k dispozici instanci konkrétního objektu (máme proměnnou s jeho obsahem), tak můžeme snadno zapsat hodnoty do vnitřních properties, případně je přečíst nebo různě měnit pomocí metod (ukážeme si později).

**Instance objektu je vytvoření lokální "živé" kopie třídy do paměti (proměnné).**

Vytvoření více instancí stejné třídy současně
---------------------------------------------

Jak už jsme si řekli, tak každý objekt má svůj lokální kontext, který platí v jeho vnitřnostech. Díky tomuto principu můžeme vytvořit mnoho nezávislých instancí té samé třídy a s těmi libovolně manipulovat. Instancí můžeme vytvořit dokonce dynamický počet a například je uložit jako prvky pole. Možnosti jsou neomezené.

> **Pozor:**
>
> Při vytváření extrémního množství instancí (stovky a více) pamatujte na paměťovou náročnost, protože PHP musí držet v operační paměti informace o všech instancích. V praxi ale problém s přeplněním paměti kvůli hodně instancím nenastává.

Příklad:

```php
$firstArticle = new Article;
$firstArticle->title = 'Můj první článek';

$secondArticle = new Article;
$secondArticle->title = 'O pejskovi a kočičce';

echo $firstArticle->title;  // vypíše "Můj první článek"
echo $secondArticle->title; // vypíše "O pejskovi a kočičce"
```

Všimněte si toho, že výpis property `title` (`->title`) je závislý na tom, z jaké instance zrovna čteme.

Shrnutí
-------

Ukázali jsme si, jak definovat naši první třídu a vytvořit její instanci (objekt), do kterého jsme zapsali několik hodnot.

V dalším pokračování si vysvětlíme pojem <a href="/metody-a-predavani-vstupu">Konstruktor, metody a předávání vstupů</a>.
