Získanie zoznamu všetkých načítaných súborov
============================================

> id: '72716cbc-90a6-4870-848b-125e8430707f'
> slug:
> 	cs: ziskani-seznamu-vsech-nactenych-souboru
> 	sk: ziskanie-zoznamu-vsetkych-nacitanych-suborov
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '7e896aa0e501d0fe33dc6595c1bfb43e'

Pri ladení zložitejších aplikácií sa niekedy stáva, že neviem, aké všetky súbory boli načítané a či niečo nechýba.

Ak používate Composer alebo iný druh <a href="/autoloading-trid">autoloadingu tried</a>, pravdepodobne tento problém nepoznáte. Pri ladení starších aplikácií od iných vývojárov však môže ísť o pomerne častý jav.

Získanie všetkých načítaných súborov možno vykonať pomocou funkcie `get_included_files()`, ktorá ich vráti ako pole absolútnych reťazcov ciest.

Pri vývoji je bežné, že sa načíta obrovské množstvo súborov (napríklad aj tento pomerne jednoduchý blog používa takmer 160 súborov). Väčšinou však veľký objem nevadí, pretože obsah súboru sa načítava z vyrovnávacej pamäte OPCache, ktorá je optimalizovaná na hromadné čítanie.
