Lokálne citlivé hashovanie
==========================

> id: '76e09294-08ea-4dde-a431-2116595d9f04'
> slug:
> 	cs: lokalne-senzitivni-hashovani
> 	sk: lokalne-citlive-hashovanie
> 
> publicationDate: '2022-07-11 10:45:00'
> mainCategoryId: '1f73dcfa-92a9-4738-ab30-8cbfb00ad23b'
> sourceContentHash: c23def9586aa05d73c4f1cf1fd1dda43

Princíp väčšiny hashovacích funkcií na vytváranie odtlačkov dokumentov spočíva v tom, že vždy vrátia rovnaký výstup pre každý vstup. Toto sa nazýva deterministické správanie. Zároveň malá zmena na vstupe spôsobí veľkú zmenu na výstupe (v skutočnosti sa vráti úplne iný hash). To je užitočné najmä vtedy, keď chceme skontrolovať, či sa vstupný dokument zmenil, a ak áno, dostaneme niečo úplne iné. Príkladom takejto funkcie je MD5 alebo SHA1.

Ak chceme z hashu odvodiť obsah pôvodného vstupu, neexistuje jednoduchý spôsob, ako to urobiť, a nezostáva nám nič iné, ako použiť hrubú silu pri každej možnosti, kým nezískame zhodu. Je to preto, že kvôli veľkej zmene výstupu nemôžeme pomocou iterácií určiť, či sa blížime k cieľu alebo nie.

Niektoré hašovacie funkcie, ako napríklad BCrypt, pre rovnaký vstup vrátia zakaždým iný výstup, aby boli odolné voči hašovaniu hesiel. Výstup totiž priamo obsahuje soľ, čo znemožňuje tzv. slovníkový útok. Táto funkcia je užitočná len na hashovanie hesiel, ale je veľmi nevhodná na kontrolu dokumentov.

Kontrola podobnosti dokumentov a vyhľadávanie duplicity obsahu
-----------------------------------------------------------

Predstavte si, že riešime algoritmus webového vyhľadávača, ktorý chce skontrolovať, ako veľmi sa webová stránka zmenila. Prípadne chce rýchlo skontrolovať, či je text z jednej stránky podobný textu na inej stránke alebo dokonca úplne duplicitný.

Jedným zo spôsobov overenia je vzájomné porovnanie jednotlivých stránok, ktoré je veľmi náročné na systémové prostriedky. Ďalšou možnosťou je vypočítať hash SHA1, ale ten hashuje obsah celého dokumentu a ak sa stránka zmení len o jeden znak, dostaneme iný hash - preto nie je vhodný na tieto účely.

Preto je jedným z možných riešení vypočítať hash z rôznych oblastí dokumentu odlišne a potom hľadať zmeny vo výstupe hashovacej funkcie.

Princíp lokálne citlivého hashovania
----------------------------------

Stiahli sme kód HTML webovej stránky, pre ktorú chceme vypočítať hash. Dokument rozdelíme na rôzne oblasti (bunky) pomocou algoritmu, ktorý je potrebné správne nakonfigurovať.

Každú oblasť zaheslujeme nezávisle pomocou spoločného algoritmu a výsledok spojíme do jedného reťazca.

Výstupom potom môže byť napríklad `3791744029724361934` (tento hash obsahu poskytol nástroj Ahrefs).

Ak napríklad vieme, že obsah hlavičky predstavuje prvých 5 znakov a pätička predstavuje posledných 5 znakov, vieme, že stred reťazca predstavuje obsah stránky. Ak sa obsah pätičky zmení na všetkých stránkach (napríklad správca webu pridá nový odkaz, zmení sa dátum aktualizácie atď.), potom sa pri prevzatí novej verzie stránky mierne zmení len niekoľko znakov v hash v pravej časti a my vieme, že sa zmenila len pätička, ale obsah zostal nezmenený. Takúto zmenu môžeme ignorovať a nemusíme prechádzať celú stránku.

Ako spoločnosť Google optimalizuje prehľadávanie webu?
----------------------------------------

Internetové roboty musia šetriť, kde sa dá. Prehľadávanie webu je veľmi nákladné a aktualizácie stoja veľa výpočtového času.

Napríklad pri pozorovaní správania sa robotického algoritmu spoločnosti Google som zistil, že reaguje len na veľké zmeny obsahu. Ak sa stránka zmení len málo, prehľadáva sa bežným spôsobom. Keď sa však napríklad výrazne zmení pätička a hlavička webu, vyhodnotí to ako redizajn a prejde väčšinu webu naraz, aby sa nový vzhľad čo najskôr zmenil.

Podobným spôsobom zisťuje aj duplicity na rôznych lokalitách. Ak je skupina stránok veľmi podobná alebo má rovnaký obsah, získajú veľmi podobný hash, ktorý robot môže použiť na rýchle overenie, či sú dokumenty podobné, a môže vybrať ten kanonický a ignorovať ostatné.

Implementácia funkcie hashovania
-----------------------------

V PHP neexistuje žiadna hotová implementácia lokálne citlivej hashovacej funkcie. Zároveň neviem o žiadnom voľne dostupnom balíku, ktorý by túto funkciu dobre implementoval.
