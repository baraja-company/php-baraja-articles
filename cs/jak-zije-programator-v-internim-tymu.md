Jak žije programátor v interním vývojovém týmu
==============================================

> id: "9309b0f2-0265-43aa-a9c3-10b2d486cdfc"
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 
> publicationDate: "2022-01-10 12:00:00"
> mainCategoryId: "483db7b7-5699-41fb-ba0b-d2b653bacd1f"

Za upřímnost se platí vysoká daň.

Tento web byl vždycky popisem reality, kterou lidé v IT zažívají, proto bych se rád podíval na mojí zkušenost při spolupráci ve vývojových týmech. V textu jsou uvedeny obecné zkušenosti, které jsem zažil napříč firmami. Žádná zkušenost není spojena s jednou konkrétní firmou a neslouží nutně ani jako kritika.

Firmy snaživé a proaktivní lidi spíše nechtějí
----------------------------------------------

Máš spoustu nápadů? Chceš inovovat? Baví tě hledat elegantní řešení složitých problémů, které tvůj tým řeší a trápí půlku firmy? Uvědomuješ si, že je důležitá bezpečnost, návrh softwaru a hledání úzkého hrdla projektu?

Pravděpodobně budeš ve vývojovém týmu dlouhodobě nešťastný.

Právě týmovost je něco, s čím poslední dobou hodně zápasím - teda konkrétně v tom plavu a snažím se pochopit pro mě zcela neintuitivní principy, kterých se musím držet.

- Týmovost znamená, že se vyvíjí to řešení, kterému rozumí celý tým. Často není nejlepší. Skoro nikdy není elegantní. Vlastně vždycky je málo efektivní. Ale má tu super výhodu, že mu rozumí celý tým a lze dlouhodobě spravovat. Toto je extrémně důležitá myšlenka. U velkých projektů totiž platí, že není potřeba zas tak velký tlak na efektivitu, protože jako firma můžete škálovat přes lidi a peněz máte dost. Mnohem důležitější je dodávat věci včas, klidně i částečně nefunkční a s předem neznámým omezením, ale včas. Souvisí to s myšlenkou, že řešení, které dodáte zítra (byť nedokonalé), má pro zákazníka mnohem větší hodnotu, než 100% odladěné řešení, které bude až za rok.
- Zavádění novinek a posouvání věcí je spíše špatně. Souvisí to s tím, že ve firmách pracují reální lidé, kteří mají rodiny a chtějí pracovat pouze v pracovní době. Z toho nutně plyne, že lidé mají omezený čas na vzdělávání a zkoušení nových věcí. V podstatě firmy musí lidem do pracovní doby narvat i vzdělávání a další rozvoj, které lze pro práci v budoucnu použít. Zajímavý důsledek je potom odkládání rozhodnutí a učení se novým věcem na nezbytně nutnou dobu. Za mně tento přístup chápu a dává mi obrovský smysl. Kdybych měl zaměstnance, tak taky budu preferovat klidné lidi, kteří ve firmě vydrží třeba 15 let, před lidmi, kteří mají sice velký obecný přehled, chtějí se posouvat, ale bude to na úkor týmu. Ono totiž platí, že společné řešení nikdy nebude takové, jaké vymyslí jednotlivec, ale to neznamená, že bude horší.

Lidé jako já jsou totiž potenciálním zdrojem problémů a konfliktů. Je hustý si to takhle uvědomit.

Při codereview hlaš jen objektivní chyby
----------------------------------------

Každá firma má svoje codestyle pravidla. Bohužel. Ze začátku mě to tak štvalo.

Když používáte některý z dospělejších jazyků, jako je třeba .NET, bývají codestyle pravidla více podobná. Jenom pak člověk zjistí, že na světě ještě existuje PHP a JavaScript. Hlavně to PHP občas trochu mrzí. PHP je velmi vyspělý jazyk s řadou skvělých funkcí, ale podle mé zkušenosti, se ve firmách používá tak třetina celkového potenciálu. Důvody bývají různé, nejčastěji setrvačnost.

V tomto ohledu jsem dlouho hledal procesní řešení na kontrolu kódu. Už dlouho si hraju s PhpStanem a sleduji myšlenky kolem něj. Základní myšlenka PhpStanu je, že upozorňuje jen na objektivní chyby, které s určitou mírou jistoty jednou nastanou. Čím víc PhpStan prozkoumávám a povyšuji na další levely, tím víc vnitřně validuji, jaká to je pravda.

Bylo by krásné, kdyby se PhpStan zavedl aspoň do poloviny českých firem. Bylo by ještě lepší, kdyby aspoň na úroveň 6. V praxi jsem zažil, že ty nejlepší firmy mají úroveň 4 nebo 5.

Zrovna nedávno jsem si říkal, jestli může existovat reálně běžící produkční aplikace, která má vlastní vývojový tým, a neprojde ani level 0 - to je level, který kontroluje věci, které byste u každé aplikace očekávali. Něco ve smyslu, že existují všechny třídy a funkce, soubory nemají parse error a podobně. A ano, i takové aplikace existují. Ale situace se dlouhodobě zlepšuje. Snad.

Při codereview proto postupuji podobně. Hlásím jen věci, které jsou objektivně vždy špatně. Často narážím na neodhadnutí míry - sice je něco špatně, ale zase to není tak velký problém, aby se to muselo řešit. Řada problémů se vyřeší jen konvencí, nebo prohlášením, že je riziko malé, tak nemá smysl ošetřovat.

Chybějící datové typy (i složené) jsem vždycky považoval za kritickou chybu. Pak jsem pochopil, že existuje testování a dump.

K této části nemám asi žádný konkrétní závěr. Mám k němu jen spoustu subjektivních připomínek, které jsou spíše zdrojem vzájemného nepochopení, tak je nebudu psát. Dlouhodobě se přikláním k závěru, že codereview dělat neumím - buď jsem moc přísný, nebo moc benevolentní. Neumím na obecné rovině poznat, na čem skutečně záleží a co není až tak důležité. Docela by mě zajímalo, jak to dělají ostatní lidé. Nikdo mi na to ještě nedokázal odpovědět tak, abych to taky uměl používat.

V zakázkovém vývoji nejsou prachy
---------------------------------

Máte agenturu a děláte weby na zakázku? Pokud nejste dostatečně velcí a neděláte velké projekty pro jiné korporace, pravděpodobně jednoho dne nebudete existovat.

Důvodem, proč se toto děje, je špatné financování projektů na míru. Pravděpodobně to souvisí s povahou smluv, které jsou výhodnější pro odběratele. Většina agentur jsou totiž brutálně podfinancované a reálně vydělávají jenom majitelům.

Když interně vyvíjíte projekt jako firma sami pro sebe, tak zpoždění dodávky o měsíc pravděpodobně tolik nevadí. Když jako agentura nedodáte zakázku o měsíc, zaručeně ten uvedený měsíc budete v práci pomalu spát, budete si soustavně ničit zdraví, klient bude nadávat do telefonu a ticketů, pořád se ptát, jestli by to nemohlo být rychleji, a jako odměnu ještě zaplatíte smluvní pokutu. A když ne, tak vás klient označí jako nespolehlivého dodavatele a možná bude uvažovat, že přejde jinam, kde to stejně nebude lepší.

Ale taková jsou pravidla hry, s kterými jsem se v praxi seznámil, a která mi udělala díru do rozpočtu.

Zakázkovka je pravděpodobně nejsložitější forma podnikání v IT. Zakázkovku nechcete dělat. Zakázkovka vás zničí. Budou vám volat o víkendu, v noci, o vánocích. Polovinu práce vám nikdo nezaplatí. Budete se omlouvat za chyby, za které nemůžete. Budete se dívat na Instagramu na storýčka kamarádů, kteří jeli letos už na třetí dovolenou, zatímco vy sedíte v sobotu ve 3 hodiny ráno v kanclu a doděláváte klientský projekt, za který dostanete v pondělí stejně vynadáno, protože ve smlouvě bylo víc, než se stihlo. Lidi si budou brát dovolenou a nemocenskou a vy budete makat místo nich. Nebo vám ukončí spoulupráci. A pak budete rádi, že zaplatíte nájem.

Ale zase se toho hodně naučíte. Protože jenom pod tlakem máte velký tlak na vzdělávání a efektivitu, abyste příště zvládli za stejný časový úsek zase o trochu víc. Blbý je, že tyto znalosti a zkušenosti moc nejde použít jinde, protože o něj firmy prostě nemají zájem (vysvětloval jsem nahoře).

Naštěstí jde v tomto případě o příběh z roku 2019 a je to už daleko.

Ideální svět nikdy nebude
-------------------------

Jestli dělám, co mě baví? A ty snad jo? :D

Řekl bych, že všechno je otázkou poměru. Pravděpodobně nikdy nebudete dělat práci, která vás bude 100% naplňovat. Pravděpodobně vám vždycky bude někdo vysvětlovat, že neumíte věc, kterou jste se na sílu učili 10 let a vnitřně víte, že umíte.

Na druhou stranu člověk za určitou míru popření vlastní osobnosti získává prostor mít něco víc. Třeba čas o víkendu, lepší zdraví, více času pro sebe, stabilní prostředí a podobně. Že se to dlouhodobě nedá vydržet je zřejmé taky.

Líbí se mi, když si mohu zkusit různé věci, abych na vlastní kůži zažil, jak to mají ostatní. Pracovat ve vývojovém týmu je proti zakázkovce ohromná nuda.

Už to není o hledání nejlepšího a nejrychlejšího řešení, ale o spolupráci. O zlepšování komunikace, emocí, a hlavně o tom, naučit se být člověk. A to je taky obrovská hodnota.
