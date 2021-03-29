Získání seznamu všech načtených souborů
=======================================

> id: "72716cbc-90a6-4870-848b-125e8430707f"
> slugCS: ziskani-seznamu-vsech-nactenych-souboru
> publicationDate: "2021-03-29 22:17:03"
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3

Při ladění složitějších aplikací se mi občas stane, že nevím, jaké všechny soubory byly načteny a jestli něco nechybí.

Pokud používáte Composer nebo jiný druh <a href="/autoloading-trid">autoloadingu tříd</a>, tak tento problém pravděpodobně neznáte. Při ladění starších aplikací od jiných vývojářů se ale může jednat o relativně častý jev.

Získání všech načtených souborů lze získat funkcí `get_included_files()`, která je vrátí jako pole stringů absolutních cest.

Při vývoji se běžně stává, že máte načteno enormní množství souborů (třeba i tento relativně jednoduchý blog používá téměř 160 souborů). Většinou velký objem ale ničemu nevadí, protože se obsahy souborů získávají z OPCache, která je optimalizována pro hromadné čtení.
