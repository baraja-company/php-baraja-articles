Lokálně senzitivní hashování
============================

> id: 76e09294-08ea-4dde-a431-2116595d9f04
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 
> publicationDate: "2022-07-11 10:45:00"
> mainCategoryId: "1f73dcfa-92a9-4738-ab30-8cbfb00ad23b"

Princip většiny hashovacích funkcí pro otisk dokumentů vzpočívá v tom, že na každý jednotlivý vstup vrátí vždy stejný výstup. Tomu se říká deterministické chování. Zároveň malá změna na vstupu způsobí velkou změnu na výstupu (prakticky se vrátí úplně jiný hash). To je výhodné hlavně v případě, kdy chceme ověřit, jestli se vstupní dokument změnil, a pokud ano, tak dostaneme něco úplně jiného. Příkladem takové funkce je MD5 nebo SHA1.

Pokud chceme z hashe odvodit obsah původního vstupu, tak neexistuje jednoduchý způsob, jak toho dosáhnout a nezbývá nám nic jiného, než hrubou silou zkoušet jednotlivé možnosti, než se trefíme. Díky velké změně na výstupu totiž nelze iteracemi určit, jestli se k cíli blížíme, nebo ne.

Některé hashovací funkce, jako například BCrypt pro stejný vstup vrátí pokaždé jiný výstup, aby byly odolné při hashování hesel. Výstup totiž rovnou obsahuje sůl, která znemožňuje tzv. slovníkový útok. Tato vlastnost je užitečná právě pro hashování hesel, nicméně pro kontrolu dokumentů je velmi nevhodná.

Kontrola podobnosti dokumentů a hledání obsahových duplicit
-----------------------------------------------------------

Představte si, že řešíme algoritmus internetového vyhledávače, který chce ověřit, jak moc se webová stránka změnila. Případně chce rychle ověřit, jestli se text z jedné stránky podobá textu na jiné stránce, nebo je dokonce úplně duplicitní.

Jedna z možností, jak toto ověřit, je porovnat každou stránku s každou, což je velmi náročné na systémové prostředky. Další varianta je počítání hashe typu SHA1, který ovšem hashuje obsah celého dokumentu a když se stránka změní jen o jediný znak, tak dostaneme jiný hash - pro tyto potřeby to je proto nevyhovující.

Jedno z možných řešení proto je počítat hash z různých oblastí dokumentu různě a hledat pak změny ve výstupu hashovací funkce.

Princip lokálně senzitivního hashe
----------------------------------

Máme stažený HTML kód webové stránky, pro který chceme spočítat hash. Dokument rozdělíme na jednotlivé oblasti (buňky) algoritmem, který je potřeba správně nakonfigurovat.

Jednotlivé oblasti hashujeme nezávisle běžným algoritmem a výsledek spojíme do jednoho řetězce.

Výstupem pak může být například: `3791744029724361934` (tento content hash poskytl nástroj Ahrefs).

Pokud například víme, že obsah hlavičky reprezentuje prvních 5 znaků a patičku zase posledních 5 znaků, tak víme, že prostředek řetězce reprezentuje obsah stránky. Když dojde ke změně obsahu patičky na všech stránkách (například správce webu přidá nový odkaz, změní se datum aktualizace a podobně), tak po stažení nové verze stránky dojde jen k nepatrné změně několika znaků v hashi v jeho pravé oblasti a my víme, že se změnila jen patička, ale obsah zůstal nezměněn. Takovou změnu tedy můžeme ignorovat a nemusíme procházet celý web.

Jak Google optimalizuje procházení webu?
----------------------------------------

Internetoví roboti musí šetřit, kde se dá. Procházení webu je velmi drahé a aktualizace stojí výpočetní čas.

Například při sledování chování algoritmu Google robota jsem vypozoroval, že reaguje jen na velké změny obsahu. Pokud se stránka změní jen málo, prochází běžným způsobem. Když se ale například výrazně změní patička i hlavička webu, vyhodnotí to jako redesign a projde větší část stránek najednou, aby získal co nejdřív novou podobu.

Podobným způsobem také rozpoznává duplicity napříč weby. Když si je skupina stránek velmi podobná nebo má stejný obsah, získají velmi podobný hash, podle kterého může robot rychle ověřit, že si jsou dokumenty podobné a může zvolit jeden kanonický a zbytek ignorovat.

Implementace hashovací funkce
-----------------------------

Hotová implementace lokálně senzitivní hashovací funkce v PHP není. Zároveň nevím o žádném volně dostupném balíku, který by tuto funkci implementoval dobře.
