Algoritmus internetového vyhledávače - Barely a crawler
=======================================================

> id: 5ad042bf-7fda-4b2e-a38c-762169d0b594
> slug:
> 	cs: vyhledavac-barely-a-crawler
>
> publicationDate: "2016-09-11 12:00:00"
> mainCategoryId: "367f936c-073f-44bd-b399-30738e93137a"

V dnešní lekci se budeme věnovat datovým barelům, jejich struktuře, StopSlovům a nakonec si popíšeme crawlery.

Datové barely
-------------

Jedná se o speciální datový typ, umístěný na více serverech současně ve více kopiích. Zpravidla se jedná o datově náročné soubory o velikosti stovek GB a jejich čtení je pomalé (proto jsou rozdělené na části) a jejich úprava je prakticky nemožná. Pokud chceme provést byť minimální změnu, tak musíme přepočítat celý barel. Například vyhledávač Seznam datové barely zvládá přepočítat maximálně jednou za pár dnů až týdnů, Google přepočítávání provádí jednou za několik hodin (a to pouze některých částí, nikdy celý najednou).

Barely obsahují slova a jejich výskyty na internetu. Dá se říci, že se jedná o surová data pro výsledky vyhledávání v ještě nesetříděné podobě (někdy bývají částečně tříděné dle relativní důležitosti) a předem připravené. Samotné vyhledávání totiž proběhne daleko před tím, než uživatel položí hledaný dotaz a právě zde jsou připraveny všechny výsledky. Samotné hledání by totiž bylo v reálném čase nemožné, vyhledávač proto v podstatě zde čte již hotové výsledky (hledání dělá komponenta indexér, o které bude samostatná kapitola).

Barely obsahují veškeré základní informace pro potřebu sestavení výsledků vyhledávání pro každé slovo, které se na internetu vyskytuje, proto je nutné řešit výkonnostní problémy při sekvenčním čtení. Barely v praxi bývají uložené po částech na více serverech a také v paměti RAM pro rychlejší přístup. Například vyhledávač Google barely vůbec nečte z disku, ale má je kompletně v operační paměti a na disku drží jen jejich kopii pro případ ztráty dat z paměti RAM.

Velké vyhledávače s dostatkem prostředků dále vedou tzv. „gold barely“, které obsahují nejdůležitější stránky na internetu a v kterých se hledá v případě velké zátěže vyhledávače. Pokud například dojde k havárii několika vyhledávacích serverů, tak právě gold barely dočasně obstarávají vyhledávání. Sice vyhledávač nenalezne veškeré výsledky, ale aspoň nedojde k úplnému výpadku hledání, ale jen k dočasnému omezení počtu prohledávaných dokumentů. Dále je nutné barely pravidelně aktualizovat, protože stránky na internetu neustále přibývají. Vzhledem k tomu, že barely mají zpravidla velikost v řádu gigabajtů, tak není možné provádět přírůstkovou údržbu, ale je nutné je jednou za nastavený čas znovu celé sestavit. Proto si vyhledávač vede ještě pomocný barel, kde jsou všechna data v podobě velké tabulky, z které se barely sestavují – například Google barely sestavuje přibližně každých 12 hodin. Velká náročnost je právě v tom, že barely musí být aspoň částečně setříděny a odrážet skutečný stav internetu. Dokud tedy vyhledávač barely neaktualizuje, tak uživatel nenalezne nové webové stránky.

Struktura barelu
----------------

Barel je v praxi uložen jako velký textový soubor, který obsahuje ID jednotlivých dokumentů, kde se hledané slovo vyskytuje a pozice výskytu právě hledaného slova.

Příklad hodně malého barelu se třemi stránkami:

```
[1:5,8,12,27,30;5:1,3,6;12:4,8,10]
```

Tento ukázkový barel obsahuje stránky s identifikátorem `1`, `5` a `12`. Přidružená čísla ukazují pozici výskytu slova v dokumentu. Takovýto barel vystihuje pouze výskyty jednoho jediného slova, takže každé slovo na internetu má svůj vlastní barel. Při hledání více klíčových slov je nutné přečíst více barelů (pro každé slovo jiný) a pak provést operace podle binárního stromu (více v předchozích kapitolách).

Průnik zpravidla probíhá tak, že se prochází jeden z dokumentů a hledá se v druhém. Pokud se jedná o velké barely, tak nemusí dojít ke kompletnímu přečtení a proto vyhledávače často stihnou přečíst jen část a pak musí vrátit výsledek, proto má smysl barely aspoň částečně třídit, aby na začátku barelu byly ty nejdůležitější stránky (které uživatel pravděpodobně hledá) a na konci barelu vše ostatní.

StopSlova
---------

Asi vás napadá, že na některá slova mohou být barely opravdu obrovské a proto by se s nimi špatně pracovalo. Například slovo `a` nebo slovo `1` se vyskytuje ve více než polovině dokumentů a přitom nenesou pro hledajícího uživatele žádný význam (protože je hledá jen vzácně a požaduje spíše okolní slova). Takovým slovům se říká StopSlova.

Problém StopSlov je v tom, že je nelze všechny vyhledat a vyhledávače proto ukazují jen zkreslenou realitu toho, jak internet skutečně na hledané slovo vypadá.

Příklad hledání se StopSlovy je dotaz `Praha 1`.

Vyhledávač na tento dotaz poskytuje pouze `3 020 000 výsledků`. Ve skutečnosti takových stránek ale existuje mnohem víc. Z výkonnostních důvodů se však neprohledávají všechny.

Možnosti přístupu k hledání se StopSlovy:

- Vůbec je neindexovat a nedělat pro ně barely (to dělají malé vyhledávače)
- Indexovat je, ale do barelů ukládat jen důležité stránky (to dělá například Google i Seznam)
- Indexovat je jako běžná slova a vytvářet kompletní barely (u tohoto řešení nastává problém v tom, že barely mohou snadno přesáhnout velikost běžného pevného disku a proto je nelze uložit a jejich čtení může trvat i jednotky sekund – toto řešení se proto v praxi nepoužívá, nebo jen u malých a specializovaných vyhledávačů)

Obecně žádný univerzálně správný přístup neexistuje a ani Google jej nepoužívá dokonale. Jedná se o natolik rozsáhlý problém, že by se jím dalo zabývat celý život. Pro potřeby této práce nicméně tento obecný popis stačí.

Crawler (robot, též pavouk)
---------------------------

Crawler je program, který systematicky prochází internet a vede si záznamy o tom, jaké slovo se na jaké stránce vyskytuje a také sbírá pomocné informace (též zvané meta informace). Crawler zpravidla běží na mnoha serverech, protože procházení internetu je náročné z hlediska propustnosti sítě a proto je nutné procházet mnoho stránek současně. Podle odhadů Google prochází až 30 tisíc stránek v jeden okamžik současně.

Ještě před otevřením stránky se Crawler podívá do databáze toho, na jaké adresy má v plánu v budoucnu jít. Pokud na adrese již byl, tak provede pouze aktualizaci záznamů (na významné weby chodí každých několik hodin, na zpravodajské weby každých několik minut, na běžné stránky maximálně jednou za měsíc).

Pokud robot přijde na novou adresu, kterou nezná, tak se podívá na obsažené odkazy na další dokumenty (stránky). U každého odkazu vypočítá jeho důležitost, a pokud se jedná o důležitý odkaz, tak na stránku později také přijde.

Například Seznam takto prochází jen několik úrovní odkazů na každém webu, Google se snaží procházet vždy kompletně celý web včetně nedůležitých dokumentů a to jej dělá nejrozsáhlejším vyhledávačem na internetu.
Robot se také snaží stránku během procházení hodnotit a počítat jí tzv. „rank“, což je číselné vyjádření toho, jak je stránka na internetu důležitá. Google v minulosti používal PageRank, který má rozsah od 0 do 10. PageRank 10 vypočítal Google pouze sám sobě a webům jako například microsoft.com nebo w3c.org. V dnešní době počítá hodnotu jinak (umělá inteligence a vektorová matematika).

Nejvyšší PageRank v české republice má web Seznam.cz s hodnotou 7. Rank má vysoký vliv na to, jak často bude robot stránku navštěvovat a jak vysoko bude ve výsledcích vyhledávání. Ranky se počítání právě ze zpětných odkazů mířících na námi požadovanou stránku.

Crawler s daty nic dalšího nedělá, pouze je stáhne z internetu a uloží do databáze, kde čekají na proces indexace.
