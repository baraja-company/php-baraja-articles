Refaktorovanie staršieho projektu PHP - ako dohnať technologický dlh
====================================================================

> id: '8f37305a-e9dd-4b1e-9f53-04f0fca648f7'
> slug:
> 	cs: refaktoring-legacy-php-projektu
> 	sk: refaktorovanie-starsieho-projektu-php---ako-dohnat-technologicky-dlh
> 
> publicationDate: '2021-05-11 20:45:00'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: b13673b23a86bc82a88636e4a9464b4b

Pri konzultáciách s kompetentnými a skúsenými vlastníkmi projektov sa často stretávam s otázkou dlhodobej udržateľnosti digitálneho projektu. Mnohé veľké projekty, ktorých vývoj trvá dlhšie ako 3 roky, začínajú byť interne zastarané a neudržateľné - teraz si predstavte tím vývojárov s rôznou úrovňou znalostí, skúseností a hlavne pracovitosti.

Aby bol váš digitálny projekt technicky na najvyššej úrovni, potrebujete:

- **Kompetentní vývojári a projektový manažér**, ktorí skvele komunikujú a každý vie, čo robí a ako to ovplyvňuje ostatných,
- **Funkčné nástroje a pracovné postupy**, ktoré celý tím pozná a podľa toho prispôsobuje svoju prácu ostatným,
- **Automatizované úlohy**, ktoré poskytujú každodennú rutinu, na ktorú je tak ľahké zabudnúť, sú mimoriadne dôležité.

Ak vyvíjate veľký projekt, nemáte to jednoduché. Je dôležité robiť veci správne, ale ešte dôležitejšie je robiť správne veci.

Čo je refaktorovanie a prečo ho potrebujete
---------------------------------------

Pojem refaktorovanie zahŕňa súbor činností, ktoré upravujú vzhľad a vnútornú logiku kódu programu bez toho, aby ovplyvnili okolité prostredie. Proces refaktorovania si môžete predstaviť ako vianočné upratovanie - zostáva rovnaký, ale je oveľa organizovanejší a nepotrebné veci sa vyhadzujú. Pri refaktorovaní je tiež dôležité mať na pamäti, že nejde o jednorazovú udalosť, ale o dlhodobý proces, ktorý musíte opakovať v pravidelných cykloch. Ak to neurobíte, hrozia vám rôzne podivné chyby, finančné straty, problémy s nástupom do zamestnania a plač.

Ak sa vám podarí udržať projekt stabilný a aktuálny, získate niekoľko skvelých výhod:

- Projekt bude **bezpečnejší**. V knižniciach, ktoré používate, sú pravidelne vydávané opravy chýb a bezpečnostných zraniteľností. Musíte sa zaoberať bezpečnosťou, je to jedna zo základných životných funkcií zdravého projektu.
- Kód a vnútorná architektúra budú jednoduchšie a lepšie premyslené. Budete môcť lepšie nájsť chyby a mnohým chybám sa dá dokonca predísť.
- So zjednodušeným kódom môžete zapojiť aj mladších vývojárov a <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">výrazne tak znížiť</a> celkový rozpočet.
- Možno zistíte, že niektoré veci robíte príliš komplikovane a zložito - projekt sa celkovo zjednoduší.

Ak chcete udržať krok s veľkým digitálnym projektom, musíte bežať. Ale vy chcete rásť.

Ako bezpečne refaktorovať
-------------------------

Každá refaktorizácia je veľkou stávkou, ktorá sa nemusí vyplatiť.

Vždy si zabezpečím stabilné prostredie dlho predtým, ako sa pustím do refaktorovania.

Pre stabilitu potrebujete najmä **funkčné zaznamenávanie chýb**. Pre jednoduché projekty stačí <a href="https://tracy.nette.org">Tracy</a>, ktorý zaznamenáva chyby do súboru HTML. Pri pokročilejších projektoch prichádzajú na rad nástroje ako <a href="https://sentry.io/welcome/">Sentry</a> alebo <a href="https://rollbar.com">Rollbar</a>. Ja osobne používam Tracy na všetky projekty, ostatné nástroje podľa typu projektu.

Približne mesiac pred refaktorovaním začnem sledovať chyby a vytvorím si tabuľku známych chýb, na ktoré sa neskôr zameriam. Vždy je dôležité vedieť, ktoré chyby boli zavedené refaktorovaním a ktoré už projekt obsahoval v minulosti. To potom pomôže lepšie obhájiť výsledky práce pred klientom.

Keď vďaka denníku viem, že pracujem v relatívne stabilnom prostredí, môžeme sa pustiť do tvrdej práce. Ďalšie rubriky obsahujú opisy metód podľa toho, aké riziko sa za nimi skrýva.

Stanovenie štýlu kódovania a normy kódovania
-----------------------------------

Väčšina projektov nemá jednotný štýl formátovania kódu. To je veľká chyba. Dobre napísaný projekt vyzerá ako projekt jednej osoby, kde sú všetky veci napísané rovnako.

Základné chyby formátovania sa dajú dobre opraviť pomocou nástroja <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a>, ktorý pravidelne spúšťam nad celým projektom.

Štýl kódovania na druhej strane zahŕňa spôsob formátovania kódu a odsadenie jednotlivých jazykových výrazov. Štandard <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a> sa vo svete PHP používa často a je na ňom založených veľmi veľa ďalších štandardov. Odporúčam používať.

Niektoré projekty nešikovne kombinujú <a href="/spaces-and-tabs">tabuľky a medzery</a>. Ak chcete účinne zabrániť porušovaniu konvencie bielych znakov (a iným chybám), vytvorte v koreňovom adresári projektu súbor `.editorconfig`, ktorý vášmu editoru povie, ako má formátovať novo napísaný kód.

Osobne používam túto konfiguráciu:

root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4

Tiež opravte všetky <a href="/apostrofy-a-poznámky-prepravky">poznámky-prepravky na apostrofy</a>. Môže sa to vykonať automaticky.

Môžete tiež automaticky skontrolovať formátovanie vášho kódu, na čo som pripravil <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">plne funkčnú ukážku na GitHube</a>. Ak potrebujete kód automaticky opraviť, môžete to urobiť pomocou rovnakého nástroja. PhpStorm je tiež veľmi dobrý v automatickom opravovaní kódu priamo, a to aj hromadne v rámci celého projektu.

Ako opraviť štýl kódovania pre veľké projekty
----------------------------------------------

Pri veľkých projektoch odporúčam vykonávať automatické formátovanie kódu po jednom module, pretože to môže v systéme Git vytvoriť obrovské množstvo konfliktov. Preto je najlepšou stratégiou opraviť čo najviac riadkov jednou veľkou revíziou, poslať túto revíziu priamo do hlavnej vetvy a potom distribuovať zmenu do ostatných vetiev.

Ak sú konflikty príliš veľké, má zmysel formátovať kód najprv v hlavnej vetve a potom znova v každej veľkej vetve, kde je veľa zmien. Veľmi veľa zmenených riadkov sa navzájom zruší (urobte rýchly posun vpred), takže vyriešite len veľmi málo konfliktov alebo niekedy žiadne konflikty.

Ak neurobíte nič, kód bude stále zlý. Iteratívne prepisovanie v priebehu mnohých rokov rozhodne nefunguje, pretože každé zlúčenie prináša potrebu opraviť desiatky riadkov, na ktorých v skutočnosti k žiadnej zmene nedošlo, a zbytočne komplikuje kontrolu kódu aj potenciálne vrátenie zmien.

PhpStan - statická analýza kódu a oprava typových chýb
------------------------------------------------------

Predtým, ako sa pustíte do rozsiahlej opravy logiky, je veľmi dôležité skontrolovať a opraviť **základné funkcie životného cyklu projektu**. Patria sem také samozrejmé veci, ktoré očakávate od každého projektu, ale napriek tomu sa niekedy nesplnia.

Napríklad:

- Všetky triedy, metódy a funkcie musia existovať
- Dedičnosť tried nesmie byť porušená
- Triedy musia implementovať použité rozhranie alebo rozhranie predka. Zároveň nesmiete dediť triedy `final`
- Nesmiete volať nebezpečné funkcie, ako napríklad `eval()`, `shell_exec()`, `var_dump()` a podobne. A ak ich aj tak zavoláte, musí to byť výslovne uvedené v komentári.
- Musíte vždy zachytiť výnimky a nedopustiť, aby celá aplikácia spadla

Riešením tohto problému je nainštalovať <a href="https://phpstan.org">PhpStan</a> do projektu a opraviť ho **najmä na úroveň 1**. Áno, je to ťažké a je to veľa práce. Ak to však neurobíte, každá refaktorizácia sa stane ruskou ruletou a vývojár len dúfa, že škody budú čo najmenšie.

Základná úroveň PhpStan nevyžaduje, aby všade existovali napríklad dátové typy a ďalšie formálne veci, ktorým sa budeme venovať v budúcnosti. Na druhej strane sa nemusí stať, že funkcia alebo metóda vráti určitý dátový typ, ale v typehint tvrdí niečo iné.

Rektor - bezpečná opakovaná oprava
-----------------------------------

Známe programátorské príslovie hovorí, že pred pridaním novej chyby do systému (napríklad vyhodením výnimky) by sme ju mali najprv pridať ako varovanie a až potom ju zmeniť na fatálnu chybu, ak sa dlho neprejavuje.

Rovnako je to aj s dátovými typmi. Pri refaktorovaní neznámeho kódu nechávam automatické nástroje ako <a href="https://getrector.org">Rector</a> najprv pridať do kódu komentáre, ktoré nič nerozbijú, ale pomôžu objasniť, čo kde bude neskôr. Tieto komentáre potom vypočuje PhpStan, ktorý môže bezpečne overiť, či sme nič neporušili a či ide o bezpečnú modifikáciu.

Zvyčajne pridám komentáre k vlastnostiam a argumentom v jednej revízii, potom počkám možno mesiac, a keď všetko dlhodobo funguje, uchýlim sa k prepisu na pevné dátové typy na miestach, kde sa dlhodobo nevyskytol problém.

Pozrite si článok <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">Ako sme za deň doplnili tisíce chýbajúcich anotácií @var</a>.

Viac informácií o <a href="https://github.com/rectorphp/rector">rektorovi nájdete na GitHub</a>.

Aktualizácia závislostí
----------------------

Skôr ako sa pustíte do aktualizácie závislostí balíkov alebo dokonca verzií PHP, je dôležité preskúmať a naučiť sa používať <a href="/github-actions-best-ci-for-2021">automatizované testy</a>.

Ak testy fungujú, môžeme prejsť na nástroj <a href="/composer">Composer</a> a vykonať aktualizáciu. Ak môžete, využite pomoc robota <a href="https://dependabot.com">Dependabot</a>, ktorý automaticky aktualizuje súbor `composer.json` vášho projektu a dokáže skontrolovať zmeny kompatibility.

Ak sa chystá veľký súbor zmien, vždy ich vykonávajte pomaly a postupne ich verziu po verzii. Nikdy neaktualizujte veľa balíkov naraz. Po každej aktualizácii skontrolujte celý projekt pomocou programu PhpStan a opravte chyby. Je to dlhý proces, ktorý trvá niekoľko hodín, ale v stávke je veľa.

Kam ďalej?
-------

Ďalšie kroky sú individuálne v závislosti od typu a stavu projektu. Vo všeobecnosti platí, že dobre navrhnutý kód napísaný staršími vývojármi sa udržiava rádovo lepšie ako kód lacno kúpený od juniora, ktorý pracoval v mediálnej agentúre, ktorá má vývoj webu takpovediac na vedľajšej koľaji.

Držte nám palce! Bude to ťažké, ale zvládnete to.
