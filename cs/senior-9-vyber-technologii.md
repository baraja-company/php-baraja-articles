Jak vybrat technologie? Kdy přejdeme na JavaScript?
===================================================

> id: "d3ea7ea7-3e2f-455d-a424-cce374bcd1d2"
> slug:
> 	cs: senior-9-vyber-technologii
>
> publicationDate: "2023-02-11 14:40:00"
> mainCategoryId: 8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a

Výběr vhodných technologií je základní prerekvizita pro to, abyste se stali seniorním vývojářem. Rozhodnutí to často nebývají vůbec lehká, protože musíte brát v potaz současný technický stav aplikace, kam vývojově směřujete, jaké máte aktuální znalosti v týmu, jaké znalosti jsou časté na trhu práce, jaké jsou na jednotlivé technologie náklady, jaká vám to přinese rizika při provozu, o jak bezpečnou a stabilní technologii jde, a v neposlední řadě, co bude vývojáře třeba za 5 let zajímat, až se vám vymění 80 % současného týmu.

Prošel jsem 6 velkými firmami, které vyvíjí v PHP. Jenom 2 z nich se snaží v dlouhodobém horizontu přejít na jinou technologii, ostatní zůstávají. To má hodně problémů. Třeba v současné době se snažím najít seniorního PHP vývojáře pro korporátní projekt, který vyvíjím pro O2, s požadavkem na docházení do kanceláří v Praze, a vnímám, jak se za posledních 5 let trh s PHP vývojáři vyčistil. PHP už zkrátka není cool a moc lidí ho nechce dělat. Je v něm málo juniorů.

Podle pohovorů u mladších lidí vnímám, že dnes frčí hodně React a obecně "tenké" technologie. Z pohledu architektury aplikace to dává smysl, pokud tento směr objevíte včas, a stihnete se adaptovat. Místo složitého bušení webových layoutů a formulářů v Latte, na které potřebujete při už trochu složitějším zadání prakticky mediorního vývojáře, vám v Reactu stačí junior, který začal v podstatě před měsícem, a i tak nenapáchá v budoucím řešení moc chyb.

React vám umožní zahodit velkou kus backendu, který se psal jenom proto, aby mohl frontend vůbec existovat. Zkrátka vám to zlevní vývoj, a jako bonus dostanete rychlejší delivery nových features, protože vývojáři nemusí řešit stále dokola složité problémy, které pramení z návrhu jazyka PHP.

Většina webových aplikací už ani nepotřebuje backend, nebo jenom minimální backend. Když vystavíte API endpointy v Node.js (taky technologie postavená na javascriptu), najednou může vývojář, který předtím dělal jenom React, psát i kusy backendu, protože jde o stejný jazyk.

Při hloubkové analýze projektů, které jsem za posledních 5 let vyvíjel, mi v Node.js chybí už jen pár věcí, kvůli kterým stále mám pro některé operace PHP.

A to:

- Doctrine (a obecně relační přístup k databázi na základě objektových entit)
- Odesílání a správa e-mailů
- SOAP API (bohužel to občas pořád někde je)
- Sessions (musíte nahradit třeba za JWT token)
- Složitá legacy logika, co byla napsána v PHP a nedokážu to snadno refaktorovat
- Rychlé zpracování složitých datových struktur, kde je potřeba mutovat data
- Stávající lidé v týmu, které musíte přeučit na něco nového

Ale pak přijde Node.js, který zbytek věcí řeší lépe. Například:

- Možnost aplikaci rovnou vysadit do cloudu
- O dost (třeba i dvojnásobně) levnější vývoj stejné funkcionality
- Stejná logika na BE i FE bez nutnosti psát kód dvakrát
- REST API endpointy
- Paralelní volání více kódu najednou
- Možnost odeslat HTTP response, ale kód pokračuje v běhu
- Crony
- Knihovny pro práci s cloud službami
- Výrazně lepší response time, protože nemusí bootovat obrovská aplikace
- Plně funkcionální paradigma (zbavíte se třeba DI, které v JS nepotřebujete)
- Práce s formuláři a daty
- Jednoduchá aktualizace a aktivní komunita vývojářů

**Komentář k Doctrine:** Vím, že JS přináší spoustu knihoven pro práci s databází. Nebo dokonce nová paradigma, jako třeba Mongo. Líbí se mi, kam míří směr zpracování dat. Na druhou stranu věřím, že tabulkové relační databáze nikdy nezmizí. Když děláte opravdu velký projekt, kde spravujete vyšší desítky milionů záznamů, už zkrátka potřebujete tradiční technologii, kterou výborně znáte, a víte, co od ní čekat. Například představa, že chci přidat sloupec (property) a znamenalo by to migračním scriptem přemapovat všechny entity, mě dost děsí.
