Jednou vám hackeři napadnou web
===============================

> id: "a11ca0a6-77e1-4ba4-8eca-83449c9cbf48"
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 
> publicationDate: "2020-08-04 12:00:00"
> mainCategoryId: "483db7b7-5699-41fb-ba0b-d2b653bacd1f"

Že se vám to nestane? :DDDDDDD

Při správě více než 300 webů se mi čas od času přihodí různé mimořádné situace. Někdy bývají dost vyhrocené, často jde však o naprostou banalitu. Pokud vás taky jako mně někdy v minulosti zlákalo programování a taky už dávno víte, že [programování je bolest](http://borisovo.cz/programming-sucks-cz.html), určitě mi dáte za pravdu.

V zajetí zlých hackerů
----------------------

Když se webová aplikace stane populární, stane se lákavým cílem hackerů. Jejich motivace obvykle není shodit celý web, ba právě naopak. Ve skutečnosti **hackeři chtějí, abyste o nich jako správci serveru vůbec nevěděli**. Dobrý hacker dlouhé měsíce vyčkává, pozoruje webový server a získává to nejcennější - vaše data.

Abyste své uživatele ochránili, je naprosto nezbytné veškerá data šifrovat a mít více úrovní ochran. Pro hesla používejte některou z pomalých funkcí, jako je například [Bcrypt + salt](https://php.baraja.cz/hashovani), Argon2, PBKDF2 nebo Scrypt. O [hodnocení bezpečnosti](https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes) už psal Michal Špaček, kterému tímto děkuji za doplnění článku. Jako majitel webu musíte při diskusi s programátorem na správném hashování hesel trvat. Jinak vás čeká pláč stejně, jako řadu lidí před vámi, kteří si také nalhávali, že se jich to netýká.

Poznáte, že je na vás mířen útok?
---------------------------------

Vypozoroval jsem, že pokud webový server dosáhne určité hranice návštěvnosti (v České republice to je přibližně nad 50 návštěvníků denně), začne na něj z ničeho nic mířit série útoků. Aby to nebylo lehké, tak má útočník vždy výhodu, protože si může vybrat, na kterou část infrastruktury začne útočit a vy - jako majitel webového serveru - to musíte včas poznat, ubránit a příště se na to lépe připravit.

Kolik na vás mířilo útoků třeba za poslední měsíc? Víte to přesně? Kolik z nich jste ubránili? Má k serveru přístup ještě někdo další?

Co když na vás někdo útočí právě teď? A teď taky... a teď taky...?

Bohužel **neexistuje jeden správný univerzální postup**, jak útok poznat. Existují však nástroje, které vám dají významnou výhodu.

Co se mi za poslední dobu nejvíc osvědčilo:

- **Když útočník neví, kde máte fyzicky servery**, má mnohem složitější pozici. Informace o fyzické architektuře lze velmi dobře mlžit službou [Cloudflare](https://www.cloudflare.com/), která proxuje (a obousměrně šifruje) veškerou síťovou komunikaci. Má i řadu dalších výhod, jako je například rychlejší načítání ze zahraničí, automatická ochrana proti DDOS útokům, komprimaci assetů a v neposlední řadě automatickou blokaci nečestných návštěvníků. Cloudflare používám pro všechny své weby (a to skoro každý projekt využívá jiné technologie).
- **Logování chyb**. Vždycky a všechno. Je velká pravděpodobnost, že vaše aplikace obsahuje hodně chyb, které spáchal váš programátor. Ať už to jsou [nezachycené výjimky](https://php.baraja.cz/vyjimky), aplikační chyby, SQL injection a podobně. Pokud programujete v PHP a logování moc nerozumíte, tak dost problémů zachytí [Tracy](https://tracy.nette.org/) (a případně i jiné nástroje, jako je například [Sentry](https://sentry.io/)) a access logy na samotném serveru. Logování chyb není nice to have, je to povinnost. Chyby loguji na všech webech, které aktivně spravuji a informace o nových chybách si nechávám posílat na mail, abych o tom ihned věděl.
- **Bezpečná a aktualizovaná platforma**. Máte na webu WordPress? Umíte ho správně aktualizovat? Znáte všechna rizika, která vám hrozí? Když je něco zadarmo, prakticky vždy to bývá na úkor nečeho jiného. Osobně pro vývoj aplikací používám [Nette framework](https://nette.org/cs/) ve verzi 3, který každý týden aktualizuji a aktivně vyhledávám bezpečnostní zranitelnosti, které opravuji. Stejně jako necháváte pravidelně servisovat své auto, tak je potřeba pravidelně servisovat svůj web a to minimálně jednou měsíčně. Údržba webu zahrnuje aktualizaci všech externích knihoven a důsledné sledování logů. Pokud používáte starou verzi knihovny, je velmi pravděpodobné, že o zranitelnosti útočník ví a pokusí se ji využít.
Otázka je kdy
Velká řada lidí v mém okolí bezpečnost neuvěřitelným způsobem podceňuje a vystavují na internet obsah, který lze velmi snadno prolomit a zneužít.

Ve skutečnosti chybují i velké firmy a to i na banalitách, jako je útok [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

Otázka totiž není, jestli vám někdy někdo prolomí web. Otázka je, kdy to bude a jestli budete dostatečně připraveni to řešit. Možná má už hacker přístup do vašeho serveru celé měsíce a vy o tom (zatím) nevíte.

Co dělat, když se stane průšvih
-------------------------------

V okamžiku, kdy se o práci hackera dozvíte, je obvykle pozdě a hacker udělal vše, co potřeboval. Velmi pravděpodobně umístil do celého serveru malware a má nad strojem aspoň částečnou kontrolu.

Jedná se o **chaotický typ problému a je potřeba jednat rychle**.

Protože se jedná o poslední fázi napadení, osobně doporučuji webový server úplně odpojit od internetu a začít postupně zjišťovat, co vše se stalo. Navíc nikdy nebudeme přesně vědět, jestli server neukrývá ještě něco dalšího. Útočník má vždycky značný náskok.

Pokud se rozhodneme na server vrátit poslední funkční zálohu, tak útočníkovi velmi pomůžeme. Ten totiž už zná způsob, jak prolomit váš server a nic mu nebrání v tom, aby to udělal za několik hodin znovu. Navíc záloha pravděpodobně obsahuje malware nebo jiný typ viru.

Ačkoli to je mezi mými klienty velmi nepopulární varianta, tak osobně vždy jako první doporučuji **úplně smazat WordPress weby**, resp. všechny systémy, které klient neaktualizuje a které mají hodně bezpečnostních hrozeb. Pokud například na jednom serveru hostujete 30 webů se stářím více než 2 roky, máte prakticky jistotu, že minimálně jeden obsahuje nějakou formu zranitelnosti.

Pokud problematice nerozumíte, raději včas kontaktujte vhodného specialistu, který má hluboké znalosti ve vaší problematice a chápe věci v širokém kontextu.

**Etická poznámka:**

Pokud spravujete web pro klienta, kterému se stal bezpečnostní incident, je vaší povinností ho o tom informovat. Pokud uniknou nějaká data (například hesla, často ale mnohem víc), je nutné informovat také koncové uživatele, že je jejich heslo veřejně známé a měli by si ho všude změnit. Pokud používáte pomalou hashovací funkci (například **Bcrypt + salt**), tak útočníkovi významným způsobem zkomplikujete prolomení hesel. (Bohužel [lze cracknout i hesla v Bcryptu](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Pokud si přejete dostávat pravidelné informace o tom, jestli váš účet někde unikl, doporučuji mít registraci ve službě [Have i been pwned?](https://haveibeenpwned.com/). Další informace o [porušení zabezpečení](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni) najdete na stránkách ÚOOÚ.

Atomové návyky
--------------

V posledním půl roce jsem dočetl knihu [Atomové návyky](https://www.melvil.cz/kniha-atomove-navyky/), která mi otevřela oči. Popisuje proces postupného 1% zlepšení každý den, které v dlouhodobém měřítku udělá hodně.

Protože se na mě obrací firmy i jednotlivci v různých fázích technologického dluhu, rád bych souhrnně prošel ty nejhorší věci:

- **FTP pro komunikaci se serverem nedává smysl**. Jde o nezabezpečený protokol, který se ovládá heslem. Pokud chcete někomu dát přístup, musí znát heslo a může to samé, co vy. Pokud mu chcete odebrat přístup, tak to nejde, jedině změnit heslo. Lepší je kompletně přejít na SSH a používat RSA klíče. Docela často se divím, že i takový problém firmy v dnešní době řeší. Nikdy si nedělejte jednu tabulku všech hesel. A už vůbec nikdy sdílenou na celý tým!
- **Zdrojové kódy patří do Gitu**, data do databáze a statický obsah (obrázky, PDF, ...) do souborů na disk. Výběr špatného datového úložiště zhoršuje návrh aplikace a také bezpečnost. URL na PDF (například s fakturou) by měl obsahovat náhodně vygenerovaný obsah, který zná jen příjemce. U extrémně citlivého obsahu (třeba výpis z bankovních účtů) je důležité obsah na disku šifrovat a poskytnout jen po přihlášení.
- Neveřejné části webu musí obsahovat META hlavičku `noindex` a `nofollow`, aby to nezaindexoval Google. Citlivý obsah, který nesmí znát vaše konkurence musí být zabezpečen heslem.
- Na serveru si vypněte [listování obsahu adresářů](https://www.simplified.guide/apache/disable-directory-listing). Nevhodným nastavením lze strojově projít celý web a najít citlivé soubory.
- Project root! Ke konfiguračním souborů nesmí nikdy existovat přímá URL. Třeba [Nette to dělá správně](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) hned od prvního tutoriálu. To samé platí pro [veřejně dostupné gitové repositáře](https://smitka.me/open-git/). Tento typ zranitelnosti je extrémně lehké podcenit a důsledky bývají tragické.
- Bug může vzniknout i nedopatřením při vývoji. Velmi důležité je pro každou změnu dělat codereview živým člověkem. Hodně chyb odhalí [PHPStan](https://github.com/phpstan/phpstan) nebo podobné nástroje ještě předtím, než kód uvidí člověk.
- Nikdy [neposílejte hesla v čitelné podobě e-mailem](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), vždycky to jde vyřešit jinak.
- Bezpečnostní problémy ve starých verzích knihoven a softwaru. Roboti každou sekundu prochází internet a útočí na známé bezpečnostní díry (SQL injection, XSS, CSRF, ...). Hodně známých děr mají starší verze WordPressu.

Dalších chyb existuje ještě celá řada a problémy se liší podle konkrétního projektu. Pokud potřebujete provést rychlou kontrolu serveru, doporučuji použít nástroj [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) a poté si najmou vhodného specialistu, který vám pomůže provést [kompletní audit webu](https://baraja.cz/audit-webu) a to nejen z důvodu bezpečnosti.

Bezpečnostní techniky jsou jako plasty v oceánu - prostě na vždycky. Zvykejte si.

Akční a reakční princip přírody
-------------------------------

Určitě jste si také všimli, že příroda používá prakticky vždy reakční princip. To znamená, že se něco v určitém prostředí stane a okolní organismy na to různě reagují. Tento přístup má řadu výhod, ale jednu obrovskou nevýhodu - vždycky budete pozadu.

Jako myslící člověk (projekťák, vývojář, konzultant) máte skvělou výhodu, a to být akční - tedy znát předem určitou část velkých rizik a aktivně zabránit tomu, aby se vůbec nestaly. Sice nikdy nezabráníte všem průšvihům, ale můžete následky aspoň zmírnit, nebo vybudovat takové kontrolní mechanismy, které problémy včas odhalí, abyste získali čas reagovat.

Většina útoků probíhá dlouhou dobu a pokud logujete, obvykle budete mít dost času problém odhalit.
