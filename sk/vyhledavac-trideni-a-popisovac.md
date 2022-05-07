Algoritmus internetového vyhľadávača - triedenie a deskriptor
=============================================================

> id: ca4aaac5-0cd8-41db-b860-c32661c43d82
> slug:
> 	cs: vyhledavac-trideni-a-popisovac
> 	sk: algoritmus-internetoveho-vyhladavaca---triedenie-a-deskriptor
> 
> publicationDate: '2016-09-11 13:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: '541310dfdc96b75a320c7cad1b03e734'

V tejto lekcii o princípoch internetového vyhľadávača pochopíme, ako vyhľadávač triedi, opisuje a vyhodnocuje výsledky.

Triedenie výsledkov
----------------

Predstavme si hotový sud, ktorý je práve pripravený na vyhľadávacom serveri. Náš prvý vyhľadávací dotaz prichádza od používateľa a teraz musíme vykonať prvé "hrubé" triedenie, ktoré sa bude ďalej spresňovať.

Uveďme nasledujúcu ukážku vstupného dotazu:

[O (and) pejskovi (and) a (and) kočičce]

Áno, v tejto podobe vyhľadávací server prijme spracovaný dotaz od používateľa a teraz čaká na vrátenie výsledku. Celkovo na to máme menej ako `300 ms`, takže rýchlo :)

V prvom kroku získame sudy plné budúcich ID výsledkov (nazývaných aj kandidáti) a operácií, ktoré sa majú vykonať. Tento načítaný sud nemusí byť v niektorých prípadoch kompletný, ale obsahuje len hodnoty, ktoré sa serveru podarilo načítať z disku v určitom vopred stanovenom časovom intervale (nazývanom aj **timeout**). Dostaneme napríklad nasledujúce číselné rady (ktoré sú vopred čiastočne zoradené):

| Sudy | Dokumenty |
|-------|---------|
| o | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| pes | 5,19,23,42,46,58 |
| a | 2,3,4,5,9,12,15,19,23,26,28,34,39,42,58,67,83,91,115 |
| pussy | 9,19,42,57,58,62,68,83 |

Teraz vykonáme hrubý priesečník všetkých množín a získame zoznam ID dokumentov, ktoré sú rovnaké vo všetkých sudoch. V praxi prechádzame najkratší zoznam a hľadáme v ostatných.

Spoločné ID vo všetkých sudoch sú "19, 42, 58", takže budúca stránka s výsledkami obsahuje 3 položky v zatiaľ neznámom poradí. Na doklady sa stále pozeráme ako na ID, pretože sa tým ušetrí obrovské množstvo údajov. Vo výsledkoch vyhľadávania však vo všeobecnosti nechceme používateľovi uvádzať čísla dokumentov, ale skôr názov dokumentu (nadpis), opis, adresu URL a ďalšie informácie. Túto funkciu zabezpečuje komponent "deskriptor".

Deskriptor
---------

Z predchádzajúcej kapitoly nám zostal zoznam kandidátov na budúce výsledky. Získali sme ich v zoradenej podobe z hľadiska systému, t. j. tak, ako ich vyhľadávač postupne zoradil v indexe. V tomto momente už nemôžeme vykonať sekvenčné čítanie veľkých dát, ale musíme sekvenčne prechádzať niektorý typ relačnej databázy, aby sme našli ďalšie informácie pre iný typ triedenia, ktorý je pre používateľa zrozumiteľnejší.

Plátok databázy môže vyzerať napríklad takto:


| ID | Názov | Štítok | URL | Poradie |
|----|---------|---------|-----|------|
| 19 | Josef Čapek: Rozprávka o psíčkovi a mačičke | To bolo v čase, keď psíček a mačička ešte spolu hospodárili, mali svoj domček pri lese, žili tam spolu a chceli všetko robiť tak, ako to robia veľkí ľudia | www.troglodytarium.cz/webz/axf/.../Capek_J_Pejsek_a_kocicka.htm | 72 |
| 42 | Rozprávanie o psovi a mačke | "Dobre," povedal pes a mačka vzala mydlo a hrniec s vodou, kľakla si na podlahu, vzala psa ako kefu a vydrhla so psom celú podlahu. Podlaha bola celá mokrá | www.capek.narod.ru/povidani.htm | 86 |
| 58 | Ako pes a mačka robili tortu k sviatku| Zajtra mal pes sviatok a mačka narodeniny. Deti to vedeli a chceli psa a mačku prekvapiť na ich narodeniny. Rozmýšľali, čo by mohli urobiť pre psa a | a.da.mek.sweb.cz/capek.j/dort.htm | 34 |

Výpočet relevantnosti
-----------------

Posledným krokom je výpočet relevantnosti odpovede. Pre väčšinu výsledkov stačí reprezentovať relevantnosť ako jedno číslo s pevným polomerom, podľa ktorého sa výsledky zoradia. Podľa poradia sa výsledky opäť "zhruba" roztriedia do niekoľkých skupín (počet skupín závisí od rozptylu poradí) a s týmito skupinami sa ďalej pracuje.

Skupiny s najvyššou pozíciou sa vyberú ako prvé, často to stačí na zoradenie prvej stránky výsledkov a nemusíme sa zaoberať ničím iným. Ak má však niekoľko rôznych dokumentov rovnakú hodnosť, musíme vykonať podrobnejšiu analýzu a vypočítať ďalšie pomocné hodnosti, ktoré pomôžu pri hodnotení stránky. Dodatočným hodnotením môže byť napríklad kvalita odkazov, vek dokumentu, dĺžka názvu, "vzhľad" adresy URL a mnoho ďalších faktorov.

Vrátenie výsledkov používateľovi
---------------------------

Hurá! Máme výsledky a teraz už zostáva len vykresliť stránku pre používateľa. Balík výsledkov sa odovzdá späť serveru, ktorý výsledky vloží do vopred pripravenej šablóny, okolo stránky vloží reklamné bannery a odošle stránku späť používateľovi. Zároveň môže vykonávať aj ďalšie pomocné operácie, napríklad ukladanie výsledkov do vyrovnávacej pamäte. Ak v blízkej budúcnosti (napríklad v priebehu poslednej hodiny) niekto iný vyhľadá rovnaký dotaz, výsledky sa nebudú vyhľadávať znova, ale iba načítajú z dočasnej pamäte, čo často pomáha zlepšiť celkový výkon vyhľadávača.

Záver a pohľad na sémantiku
---------------------------

Cieľom tohto článku bolo opísať základné princípy vnútorného fungovania vyhľadávačov a to, ako dokážu vyhľadať teoreticky neobmedzený počet dokumentov za stále primeraný čas. Ide o obrovské matematické optimalizácie, ktoré boli vyvíjané dlhé roky najlepšími programátorskými tímami.

Úplný opis podrobností presahuje rámec tohto dokumentu. Celú vec komplikuje aj skutočnosť, že väčšinu ďalších krokov žiadny z vyhľadávačov podrobne nepopisuje, pretože sú ich obchodným tajomstvom.

Dôležité je tiež poznamenať, že vyhľadávače stále nerozumejú obsahu stránok a významu výsledkov a stále ide len o štatistický výpočet založený na hrubom výpočtovom výkone a schopnosti ľudí odkazovať na kvalitný text, a tento systém je tiež dosť zraniteľný (vezmite si príklad "Google bomby", keď zákerný webmaster vnucuje vyhľadávacím dotazom obsah, ktorý je irelevantný).

Tento problém by mala čiastočne vyriešiť umelá inteligencia, ktorú sa postupne snažia integrovať všetky vyhľadávače. To je však ešte stále vzdialená hudba budúcnosti, pretože to nie je jednoduchá úloha a väčšinou ide len o zdokonalenie štatistických výpočtových metód. Príkladom nech je grafové vyhľadávanie Google, ktoré sa snaží odpovedať na dotaz priamo (hoci vo svojej vnútornej štruktúre stále len vyhľadáva v preddefinovanej znalostnej databáze).

Takže keď nabudúce zadáte svojmu obľúbenému vyhľadávaču dotaz, spomeňte si, koľko práce musel váš vyhľadávač vykonať a koľko TB údajov musel prečítať z disku, aby našiel 10 najlepších dokumentov pre váš vyhľadávací dotaz.
