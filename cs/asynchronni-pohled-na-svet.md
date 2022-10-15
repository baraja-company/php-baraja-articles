Asynchronní pohled na svět
==========================

> id: "7ed25269-659f-4d17-a642-d07480cbfafa"
> slug:
>   cs: asynchronni-pohled-na-svet
>
> publicationDate: "2022-10-15 11:30:00"
> mainCategoryId: "483db7b7-5699-41fb-ba0b-d2b653bacd1f"

Už docela dlouho si všímám, že náš svět má asynchronní a decentralizovanou povahu. Když si to uvědomíte, a začnete přemýšlet, jak to využít pro váš prospěch, může snadno vzniknout celá velká koncepce, jak se dívat na řešení složitých problémů. V tomto příspěvku bych rád vysvětlil některé myšlenky, které už teď používám. Nemám je z žádného konkrétního zdroje, jde spíše o kombinaci více zkušeností a vlastní nápady. Tyto principy nefungují pro všechny případy.

1. Definice prostředí a cílů
----------------------------

Skoro všechny projekty, které jsem kdy řešil (ať už sám nebo v týmu) měly poměrně konkrétně definovaný cíl. Tento přístup dává obrovský smysl, protože je důležité vědět, co chceme. Na druhou stranu věřím, že definovat konkrétní cíl v komplexním prostředí nelze, a často během implementace zjistíme, že chceme dojít vlastně někam jinam.

Místo konkrétního cíle si proto vystavuji plochu různých cílů, kde existují alternativy, nebo i úplně nový izolovaně nezávislý cíl může být správným cílem, pokud bude dávat smysl.

Příklady z praxe:

- Potřebuji postupně expandovat na další trhy. Jeden z dílčích cílů, který jsem během implementace objevil, může být strojový překlad webu.
- Potřebuji vyzvednout 6 zásilek na různých místech po Praze a neznám nejkratší cestu. Nechce se mi ji složitě zjišťovat, tak se prostě vydám směrem k nejvzdálenějšímu bodu a cestou měním plánu vůči dalším trasám. Když například přestupuji na Andělu, tak se rozhodnu pro tramvaj, která přijede jako první, což dynamicky ovlivní další plán trasy.
- Potřebuji napsat tento příspěvek, ale nemám hodinu času v kuse. Mohu ho proto psát po částech při jízdě v metru jako jednotlivé kapitoly. Finální merge do této podoby pak udělám v budoucnu.
- Nevím, jak funguje algoritmus pro výpočet splátkování zboží u mého klienta, který musím znovu naprogramovat. Položím proto dotaz na více lidí zároveň a během toho řeším něco jiného. Asynchronně za 2 dny přijde více různých odpovědí, podle kterých až později provedu rozhodnutí.
- Potřebuji se dlouhodobě zbavit PHP na serveru, tak postupně přepisuji jednu stránku po druhé do Reactu. Weby postupně přesouvám do cloudu a stavím je na staticky generovaném Nextu.

Žádný z cílů není konečná destinace. Pokud pracujete s plochou a ne s bodem, s mnohem větší pravděpodobní se trefíte. Zároveň tady funguje dobře motivace, protože to nejhorší, co se vám může stát, je dosáhnout svého cíle, a pak už nemít něco, na co se můžete těšit v budoucnu.

Hodně důležité také je říct, že pro podobný mindset musíte mít poměrně stabilní prostředí, kde můžete dělat chyby. Tento přístup neumí garantovat konkrétní termín vyřešení konkrétního jednoho úkolu. Na druhou stranu umí optimalizovat vyřešení co největšího počtu paralelních úkolů za co nejmenší možný čas, i když ne nutně v pořadí, v jakém přišly do fronty.

Princip by se dal přirovnat tomu, jak fungují například výpočty při paralelním programování. Zkrátka rozložíte úkoly na jednotlivá vlákna, která najednou řeší různé pod-úlohy, a jakmile získáte všechny odpovědi, můžete složit řešení.

2. Vysazování nových verzí softwaru
-----------------------------------

S tímhle hodně bojuju všude.

Vývoj softwaru se obvykle řeší tak, že máte nějakou stabilní verzi, která běží produkčně pro uživatele, pak máte nějaké testovací prostředí, do kterého se dělá nový vývoj, a jednou za čas novinky přesunete z testovacího prostředí pro produkce, čemuž se říká release. Tento proces je relativně bezpečný a pomalý.

Od té doby, co jsem postupně přešel na mikrofrontendovou architekturu, kdy je frontend aplikace (to co vidí uživatel) kompletně oddělený od backendu (tam, kde se dějí výpočty a zpracovávají data), může proces releasu fungovat jinak.

Mám stále jedno produkční prostředí, které vždycky funguje. Z něj vzniká pro každý úkol nová větev, ve které se řeší konkrétní feature. Protože pro hostování frontendu používám Vercel (velmi dobrý poskytovatel cloudových služeb), mohu pro každou vývojovou větev mít vlastní URL.

Jakmile se nová funkce otestuje, je ihned zamergována a vysazena do produkce asynchronním procesem. Běžně se proto může stát, že dojde třeba k 15 vysazením nové verze každý den.

Hodně to souvisí s myšlenkou, kterou mi předali v LMC - "Funkce, kterou poskytnete zákazníkovi zítra, byť v nedokonalém stavu, má pro něj mnohem vyšší hodnotu, než funkce, která bude perfektně odladěná, ale bude až za rok". Toto může fungovat ale jen v případě, když máte způsob, jak novinky rychle distribuovat - typicky webový vývoj.

Na frameworku Next.js (postavený na Rectu) a Vercelu se mi hodně líbí princip, že si spojíte GitHub repositář přímo s Vercelem, a s každým commitem proběhne automatický build, testy, a když je vše ok, tak vysazení do produkce. Vývojář se proto nemusí o nic starat, a aplikace se s nulovým úsilím vysazuje klidně každou hodinu. Je velmi důležité rutinní věci formalizovat a pak automatizovat.

3. Publikace obsahu
-------------------

Mám rozepsány vyšší desítky témat a příspěvků u sebe v Apple Notes. Ke každému tématu mě průběžně napadají nové myšlenky, které si poznamenávám, a řadím je podle štítků. Když k jednomu tématu vznikne víc poznámek, převádím je na kapitoly, a skupinu kapitol pak na články a FB příspěvky.

Vlastnosti tohoto principu:

- Předem nevím, kolik článků kdy vydám,
- Ale zase vím, že toho může být hodně,
- Jsem schopen zajistit, že každý příspěvek bude plný informací, postřehů a myšlenek, které vyzrávaly časem (tento příspěvek vyzrával přibližně 3 měsíce),
- Je to udržitelné a bude mě to bavit, protože nemusím v jeden okamžik napsat tunu textu, a riskovat, že se něco zapomene.

A pak proces vydávání.

Spoustu obsahu vydávám nejprve tiše na webu, aby se toho nejprve všiml Google a stránka se dostala do indexu. Když má dobré pokrytí klíčových slov a jsem s ním celkově spokojen, jde téma třeba na sociální sítě.

U hodně témat vím, že vás nebudou zajímat a spíše budou obtěžovat. Zároveň je ale chci mít online, protože je může někdo v budoucnu hledat. V takovém případě zůstává článek jenom na webu. Příkladem těchto článků jsou různé vykrývací články, které spojují celou tematickou oblast, abych měl na webu pokryto co největší tematické celky. Vykrývací články bývají často odbornější a nudné. Nebo to jsou strojově generované kategorie, kde jenom organizuji víc článků do témat, a pokrývám tak co nejvíc klíčových slov, co pak může někdo chtít hledat.

4. Poznání, vzdělávání, testování
---------------------------------

Rád si hraju s technologiemi, u kterých není od začátku jasné, k čemu to bude jednou dobré.

Třeba strojový překlad. Na první pohled nedává smysl přeložit desítky článků do cizích jazyků pro čtenáře, s kterými nejsem v kontaktu. Na druhou stranu to může být jeden k kroků, který v budoucnu umožní udělat rozhodnutí, pro které je potřeba mít splněné předpoklady.

Obecně nikdo nevíme, jak bude budoucnost vypadat. Dává mi proto obrovský smysl pokrývat co největší množinu možností, které chcete aspoň povrchně rozumět a v budoucnu možná řešit. Je dobré o krocích nepřemýšlet jenom jako o vyřešeném úkolu, ale jako o nikdy nekončícím vývojovém procesu, který nemá finální destinaci.

5. Funguje tento přístup pro dodávání zakázkových projektů?
-----------------------------------------------------------

Většinou ne.

Musíte mít klienta, který je váš partner, a oba chcete dodat nejlepší možné řešení, i když předem víte, že co to je, zjistíte až na konci.

U nás v branži se tomu říká agilní vývoj, a tyto principy na něm staví. Na druhou stranu jsem vypozoroval, že přímo agilní vývoj v té podobě, jak ho znám, mi přímo nevyhovuje, a rád vymýšlím ohnuté alternativy.

U agilie hodně vnímám princip závazků, kdo co vyřeší v rámci konkrétního sprintu. Víc mi dává smysl budovat prostředí tak, že máte obrovský backlog úkolů, které se řeší podle toho, co je zrovna nejvíc potřeba, nebo na to mám mentální kapacitu. Když jsem například na cestách, tak řeším spíše rozvojové úkoly na přemýšlení, které mohu dělat venku bez počítače. Když jsem zase v kanceláři, tak řeším spíše náročné úkoly na algoritmy a matematiku, protože se mohu plně soustředit a investovat hodně mentální energie.

Tento princip nedoporučuji, pokud zakládáte úplně nový e-shop, nebo vaše podnikání krachuje. Potřebujete stabilní prostředí, které běží samo, a vy budete jenom šperkovat. Zároveň ale víte, že kdybyste nedělali nic, tak vaše stabilní prostředí jednou skončí.
