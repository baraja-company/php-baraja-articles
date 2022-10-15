Asynchrónny pohľad na svet
==========================

> id: '7ed25269-659f-4d17-a642-d07480cbfafa'
> slug:
> 	- null
> 	sk: asynchronny-pohlad-na-svet
> 
> cs: asynchronni-pohled-na-svet
> publicationDate: '2022-10-15 11:30:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '24d4ec106e7febec5ee84cdf86bd8f28'

Už dlho si všímam, že náš svet má asynchrónny a decentralizovaný charakter. Keď si to uvedomíte a začnete premýšľať o tom, ako to využiť vo svoj prospech, ľahko sa vám vynorí celá veľkolepá koncepcia, ako sa pozerať na riešenie zložitých problémov. V tomto príspevku by som chcel vysvetliť niektoré z nápadov, ktoré už používam. Nečerpám ich zo žiadneho konkrétneho zdroja, je to skôr kombinácia viacerých skúseností a mojich vlastných nápadov. Tieto zásady nefungujú vo všetkých prípadoch.

Definovanie prostredia a cieľov
-------------------------

Takmer všetky projekty, ktoré som kedy riešil (či už sám alebo ako člen tímu), mali pomerne presne definovaný cieľ. Tento prístup má veľký zmysel, pretože je dôležité vedieť, čo chceme. Na druhej strane si myslím, že definovať konkrétny cieľ je v komplexnom prostredí nemožné a často počas realizácie zistíme, že sa vlastne chceme dostať niekam inam.

Takže namiesto konkrétneho cieľa si vytvorím oblasť rôznych cieľov, kde existujú alternatívy, alebo dokonca úplne nový izolovaný nezávislý cieľ môže byť tým správnym cieľom, ak to má zmysel.

Príklady z praxe:

- Musím postupne expandovať na ďalšie trhy. Jedným z čiastkových cieľov, ktoré som objavil počas implementácie, môže byť strojový preklad webu.
- Potrebujem vyzdvihnúť 6 zásielok na rôznych miestach v Prahe a nepoznám najkratšiu trasu. Nechcem to zložito zisťovať, a tak sa jednoducho vydám smerom k najvzdialenejšiemu bodu a po ceste zmením plán smerom k ďalším trasám. Napríklad pri prestupovaní na Anděle sa rozhodnem, že pôjdem električkou, ktorá príde ako prvá, čo dynamicky ovplyvní ďalší plán trasy.
- Musím napísať tento príspevok, ale nemám hodinu času v kuse. Preto ju môžem písať po častiach počas jazdy metrom ako jednotlivé kapitoly. V budúcnosti potom vykonám konečné zlúčenie do tohto formulára.
- Neviem, ako funguje algoritmus na výpočet výkupu tovaru pre môjho klienta, ktorý musím preprogramovať. Takže položím otázku viacerým ľuďom naraz a popritom vyriešim niečo iné. Asynchrónne v priebehu 2 dní príde viacero rôznych odpovedí, na základe ktorých sa rozhodnem až neskôr.
- Potrebujem sa na dlhú dobu zbaviť PHP na serveri, takže postupne prepisujem jednu stránku po druhej do Reactu. Postupne presúvam stránky do cloudu a budujem ich na staticky generovanom Necte.

Žiadna z destinácií nie je konečnou destináciou. Ak pracujete s povrchom, a nie s bodom, je oveľa pravdepodobnejšie, že ho trafíte. Zároveň tu dobre funguje motivácia, pretože najhoršie, čo sa môže stať, je dosiahnuť cieľ a potom sa nemať na čo tešiť v budúcnosti.

Dôležité je tiež povedať, že na to, aby ste mali podobné zmýšľanie, potrebujete pomerne stabilné prostredie, v ktorom môžete robiť chyby. Tento prístup nemôže zaručiť konkrétny termín vyriešenia konkrétnej úlohy. Na druhej strane môže optimalizovať riešenie čo najväčšieho počtu paralelných úloh v čo najkratšom čase, hoci nie nevyhnutne v poradí, v akom prišli do frontu.

Tento princíp by sa dal prirovnať napríklad k tomu, ako fungujú výpočty v paralelnom programovaní. Stručne povedané, rozkladáte úlohy na jednotlivé vlákna, ktoré riešia rôzne čiastkové úlohy naraz, a keď získate všetky odpovede, môžete zostaviť riešenie.

Nasadenie nových verzií softvéru
--------------------------------

S tým sa stretávam často a všade.

Vývoj softvéru sa zvyčajne realizuje tak, že máte nejakú stabilnú verziu, ktorá je určená pre používateľov, potom máte nejaké testovacie prostredie, v ktorom sa vykonáva nový vývoj, a raz za čas presuniete novinky z testovacieho prostredia do produkčného, čo sa nazýva vydanie. Tento proces je relatívne bezpečný a pomalý.

Keďže som postupne prešiel na mikrofrontendovú architektúru, kde je frontend aplikácie (to, čo vidí používateľ) úplne oddelený od backendu (kde prebiehajú výpočty a spracovanie údajov), proces vydávania môže fungovať inak.

Stále mám jedno produkčné prostredie, ktoré vždy funguje. Z nej sa pre každú úlohu vytvorí nová vetva, ktorá sa bude zaoberať konkrétnou funkciou. Keďže na hostovanie frontendov používam Vercel (veľmi dobrý cloudový poskytovateľ), môžem mať pre každú vývojovú vetvu vlastnú adresu URL.

Po otestovaní novej funkcie sa okamžite zlúči a nasadí do produkcie v asynchrónnom procese. Preto sa môže bežne stať, že každý deň dôjde k nasadeniu možno 15 nových verzií.

Veľa z toho súvisí s myšlienkou, ktorá mi bola tlmočená v LMC - "Funkcia, ktorú poskytnete zákazníkovi zajtra, aj keď v nedokonalom stave, má pre neho oveľa väčšiu hodnotu ako funkcia, ktorá je dokonale odladená, ale bude k dispozícii až o rok." To však môže fungovať len vtedy, ak máte spôsob, ako rýchlo šíriť novinky - zvyčajne ide o vývoj webu.

Na frameworku Next.js (postavenom na Recto) a Verceli sa mi veľmi páči princíp, že svoj repozitár GitHub pripojíte priamo k Verceli a pri každej revízii prebehne automatické zostavenie, testy a potom nasadenie do produkcie, keď je všetko v poriadku. Vývojár sa teda nemusí o nič starať a aplikácia sa nasadzuje každú hodinu bez akéhokoľvek úsilia. Je veľmi dôležité formalizovať rutinné činnosti a následne ich automatizovať.

Publikovanie obsahu
----------------

V poznámkach Apple Notes som rozdelil desiatky tém a príspevkov. Pri každej téme neustále prichádzam s novými nápadmi, ktoré si zapisujem a triedim podľa značiek. Keď sa k téme vytvorí viacero poznámok, prevediem ich do kapitol a potom skupinu kapitol prevediem do článkov a príspevkov na FB.

Vlastnosti tohto princípu:

- Vopred neviem, koľko článkov niekedy uverejním,
- Ale na druhej strane viem, že ich môže byť veľa,
- Som schopný zabezpečiť, aby každý príspevok bol plný informácií, postrehov a myšlienok, ktoré dozrievali v priebehu času (tento príspevok dozrieval približne 3 mesiace),
- Je to udržateľné a ja si to užijem, pretože nemusím písať tonu textu na jednom mieste a riskovať, že niečo zabudnem.

A potom proces publikovania.

Veľa obsahu najprv v tichosti zverejním na webe, aby si ho Google všimol ako prvý a stránka sa indexovala. Keď má dobré pokrytie kľúčovými slovami a som s ňou celkovo spokojný, téma sa dostane napríklad na sociálne siete.

Pri mnohých témach viem, že vás nebudú zaujímať a skôr vás budú otravovať. Zároveň ich však chcem mať online, pretože ich niekto môže v budúcnosti hľadať. V takom prípade článok zostane len na webe. Príkladom týchto článkov sú rôzne prekrývajúce články, ktoré spájajú celú tematickú oblasť, aby som mal na stránke pokrytých čo najviac tém. Obálkové články sú často technickejšie a nudnejšie. Alebo sú to strojovo vytvorené kategórie, v ktorých jednoducho usporiadam viacero článkov do tém, čím pokryjem čo najviac kľúčových slov, ktoré potom niekto môže chcieť vyhľadať.

Znalosti, vzdelávanie, testovanie
------------------------------

Rád sa hrám s technológiami, pri ktorých nie je od začiatku jasné, na čo budú raz dobré.

Ako strojový preklad. Na prvý pohľad nemá zmysel prekladať desiatky článkov do cudzích jazykov pre čitateľov, s ktorými nie som v kontakte. Na druhej strane môže ísť o jeden z krokov, ktoré v budúcnosti povedú k rozhodnutiu, pre ktoré je potrebné splniť predpoklady.

Vo všeobecnosti nikto nevie, ako bude vyzerať budúcnosť. Preto mi dáva obrovský zmysel pokryť čo najviac možností, ktorým chcete aspoň povrchne porozumieť a možno sa nimi v budúcnosti zaoberať. Je dobré myslieť na kroky nielen ako na vyriešenú úlohu, ale ako na nikdy nekončiaci evolučný proces, ktorý nemá konečný cieľ.

Funguje tento prístup aj pri realizácii projektov na zákazku?
--------------------------------------------------------

Väčšinou nie.

Musíte mať klienta, ktorý je vaším partnerom a obaja chcete dodať najlepšie možné riešenie, aj keď dopredu viete, že sa to dozviete až na konci.

V našom biznise sa to nazýva agilný vývoj a tieto princípy na ňom stavajú. Na druhej strane som si všimol, že agilný vývoj, ako ho poznám, mi priamo nevyhovuje, a rád vymýšľam ohnuté alternatívy.

Pri agilite vidím veľa princípov záväzkov v zmysle toho, kto čo rieši v rámci konkrétneho šprintu. Väčší zmysel mi dáva vybudovať prostredie tak, aby ste mali obrovský počet nevybavených úloh, ktoré sa riešia na základe toho, čo je práve teraz najpotrebnejšie alebo na čo mám mentálnu kapacitu. Keď som napríklad na cestách, mám tendenciu riešiť viac úloh na rozvoj myslenia, ktoré môžem robiť vonku bez počítača. Keď som opäť v kancelárii, riešim úlohy náročnejšie na algoritmy a matematiku, pretože sa môžem plne sústrediť a vynaložiť veľa mentálnej energie.

Tento princíp neodporúčam, ak zakladáte úplne nový e-shop alebo ak vaše podnikanie krachuje. Potrebujete stabilné prostredie, ktoré funguje samo, a budete len šperkovať. Zároveň však viete, že ak nič neurobíte, vaše stabilné prostredie jedného dňa skončí.
