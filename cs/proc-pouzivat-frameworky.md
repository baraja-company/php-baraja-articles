Proč a jak používat frameworky a knihovny
=========================================

> id: "5ea6cdde-f67c-4f3f-8871-37b27ee8e23a"
> slug:
> 	cs: proc-pouzivat-frameworky
> 
> publicationDate: "2020-02-16 22:13:46"
> mainCategoryId: "3e45c55a-a4cd-4745-b1bb-0332702fefbf"

Jeden známý vtip říká, že programátoři začnou používat frameworky až ve chvíli, kdy si napíší svůj vlastní a zjistí, že to nikam nevede. Nejvtipnější na tom je ale hlavně to, že to je pravda. Sám jsem si to zažil. Dokonce dvakrát.

Však i na hlavní stránce <a href="https://nette.org">Nette</a> se píše:

> Opravdoví programátoři **nepoužívají frameworky**. Píší webové aplikace přes příkazovou řádku rovnou na server. Tímto jim vzdáváme hold. Nám ostatním Nette ohromným způsobem ulehčí a zpříjemní práci.

Role frameworku při vývoji aplikace
-----------------------------------

Určitě to sami znáte:

Dostanete nápad na skvělý projekt, tak začnete psát kód na zelené louce přímo v čistém PHP... za pár hodin zjistíte, že se velká část práce pořád opakuje a řešíte základní systémové problémy. Třeba to, jak se připojit k databázi, jak vykreslit formulář, jak zvalidovat data, jak odeslat e-mail a mnoho dalšího.

Pravděpodobně si pro tyto úlohy napíšete vlastní funkce, které budete volat. Pokud píšete několik projektů najednou, tak si tyto funkce začnete mezi projekty sdílet v jednom velkém zabordeleném souboru a budete je podle aktuálních potřeb průběžně vylepšovat.

A až budou funkce v určité vývojové fázi, že na nich začnete stavět pokaždé nový projekt a rovnou vyvíjet, tak se dá hovořit o tom, že jste si napsali vlastní framework, nebo přinejmenším knihovnu.

Co je a co dělá framework
-------------------------

Jak už jsme si na praktickém příkladu ze života ukázali, tak framework je sbírka mnoha funkcí (ideálně ale tříd a objektů), které programátorovi šetří práci. Při vývoji totiž nemusí přemýšlet třeba nad tím, jak se připojit k databázi, ale ví, že to je už někde naprogramované a "prostě to funguje".

Hotové frameworky jsou tedy sesbírané chytrosti stovek lidí, kteří dlouhou dobu vyvíjeli tisíce aplikací, aby odladili jedno řešení a jeden ekosystém, jak nové projekty řešit.

Díky frameworkům také získáte řadu **best practices**, tedy postupů, jak dané problémy řešit a nemusíte nad tím přemýšlet. Pokud narazíte na problém, tak ho pravděpodobně už někdo před vámi řešil a vždycky je lepší najít řešení v dokumentaci, než ho sám složitě vymýšlet.

Abstraktní programování a princip zapouzdření
---------------------------------------------

Dobře navržené frameworky, jako je třeba Nette, umožňují programovat na **velmi vysoké úrovni abstrakce**.

Programátor (uživatel frameworku) tak vůbec nemusí rozumět tomu, co se interně děje a jak přesně vnitřně fungují jednotlivé komponenty, což mu ušetří spoustu času a úsilí. Může se tak zaměřit na řešení reálných problémů jeho aplikace a dostat se k výsledkům velmi rychle.

Sám David Grudl (autor Nette frameworku) kdysi říkal, že Nette navrhl tak, aby zvládl naprogramovat web pro klienta za noc, když přijde pozdě večer z hospody a ráno se web musí spouštět. To je způsobeno zejména tím, že je vývojář úplně odstíněn od technických věcí.

Aby toto mohlo fungovat a vývojář jen používal hotový framework jako celek, tak je velmi důležité správně používat <a href="/zapouzdreni">princip zapouzdření</a>.
