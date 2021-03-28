Základní filosofie objektově orientovaného programování
================================

> id: c38214d9-8c92-4484-9c31-239d716f2545
> slugCS: filosofie-oop
> publicationDate: "2020-01-02 22:21:56"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

Objektově orientované programování je paradigma, tedy pohled na to, jak programovat. Brzy sami uvidíte, že právě OOP přináší dost zásadní zjednodušení na všechny časté úlohy a potíže, které se při reálném programování řeší stále dokola.

Základní myšlenka
-----------------

Základní myšlenka objektově orientovaného programování je rozdělit velkou aplikaci (složitý úkol) na spoustu malých částí, které umíme řešit elegantně a nezávisle na sobě.

Pokud například programujeme rezervační systém pro letenky, tak jde o velmi komplexní projekt, který řeší tisíce případů. Když se nám podaří celou složitou logiku rozložit na mnoho vrstev a částí, dokážeme celý složitý systém snadno pochopit a jednotlivé dílčí úkoly naprogramovat samostatně.

Praktické výhody OOP a rozdělení kódu na objekty
------------------------------------------------

Kromě akademického pohledu na věc existuje i řada praktických důvodů, proč OOP používat:

- V aplikaci vznikne tzv. <a href="/zapouzdreni">zapouzdření</a>, což znamená, že se vždy zpracovávají data v určitém lokálním kontextu, kterých je málo a nemusíme tedy přemýšlet na celou aplikací najednou.
- Dokážeme aplikaci rozdělit na stovky tisíc malých částí, které mohou vyvíjet různí lidé paralelně a jen si domluvíme společné rozhraní.
- Na aplikaci se lze dívat z pohledu různých vrstev (abstraktně na celé moduly, nebo lokálně na konkrétní funkce řešící konkrétní algoritmus).
- Při debugování aplikace (odstraňování chyb) jsme schopni aplikaci krokovat, některé části vynechat nebo nahradit.
- Můžeme lépe aplikovat **návrhové vzory** a díky tomu předejít chybám a složitostem v kódu ještě předtím, než nastanou.

Osobně si neumím představit, že by týmy s více jak jedním člověkem programovali jakkoli jinak.

> **Poznámka:**
>
> Použití objektů přináší počítači o trochu vyšší režii paměti i procesoru, proto tím výkon aplikace trochu snížíme. V reálném prostředí to ale ničemu nevadí, protože přeprogramováním aplikace do objektů sice ztratíme část výkonu (obvykle jednotky procent), zato ale ušetříme čas programátorům (obvykle desítky až stovky procent). Čas člověka je vždycky mnohem dražší (a navíc velmi omezený), než čas počítače.
>
> Nezapomeňte také na to, že OOP přináší velké zjednodušení celé aplikace a umožňuje rozsáhlé aplikace dokončit za rozumný čas. Velké množství složitých aplikací by bez objektů skoro nešlo naprogramovat.

Vztah vůči reálnému světu
-------------------------

Základní snahou OOP je při návrhu softwaru co nejvíc simulovat vlastnosti, chování a principy reálného světa. Objekty v OOP reprezentují reálné entity. Tento způsob myšlení nám umožňuje budovat obrovské komplexní systémy, kterým lze dobře rozumět, interně řeší reálné problémy tak, jak by se řešily i bez počítače a je možné principy vysvětlit reálným lidem.

Pokud například implementujeme aplikaci na správu obsahu, dává smysl si celou vnitřní logiku rozvrhnout do mnoha reálných entiít (článek, autor, kategorie) a relace nevytvářet podle uměle generovaných ID záznamů (jak se to konvenčně v databázích dělá), ale podle reálných vazeb.

Příklad konkrétní implementace:

```php
class Article {
    /** @var Author */
    private $author;

    /** @var Category[] */
    private $categories;

    /** @var string */
    private $title;
}

class Author {
    /** @var string */
    private $name;
}

class Category {
    /** @var string */
    private $name;
}
```

Jak můžeme vidět, třída `Article` neobsahuje pouze technické parametry, jako je například ID záznamu s autorem, ale jde o reálnou vazbu na entitu Autora, která má opět své vlastnosti.

Vysvětlení konkrétní implementace a syntaxe najdete v <a href="/uvod-do-oop">úvodním tutoriálu</a>.
