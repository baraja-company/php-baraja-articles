Refaktoring legacy PHP projektu - jak dohnat technologický dluh
===============================================================

> id: "8f37305a-e9dd-4b1e-9f53-04f0fca648f7"
> slug:
> 	cs: refaktoring-legacy-php-projektu
>
> publicationDate: "2021-05-11 20:45:00"
> mainCategoryId: "3666a8a6-f2a3-405d-8263-bd53c4301fb3"

Při konzultacích s poučenými a zkušenými majiteli projektů často narážím na otázku dlouhodobé udržitelnosti digitálního projektu. Řada velkých projektů, které přesáhnout 3 roky vývoje, začíná vnitřně zastarávat a přestává být udržitelný - teď si do toho ještě představte tým vývojářů s různou úrovní znalostí, zkušeností a hlavně pečlivostí.

Abyste váš digitální projekt udrželi po technické stránce na TOP úrovni, tak potřebujete:

- **Kompetentní vývojáře a projektového manažera**, kteří mají skvěle sehranou komunikaci a každý dobře ví, co dělá, a jaký to má dopad na ostatní,
- **Funkční nástroje a workflow**, které zná celý tým a podle toho přizpůsobuje svoji práci ostatním,
- **Automatizované úlohy**, které zajišťují denní rutinu, na kterou je tak leké zapomenout, že je extrémně důležitá.

Pokud vyvíjíte velký projekt, tak to zkrátka nemáte lehké. Je důležité dělat věci správně, ale mnohem důležitější je dělat ty správné věci.

Co je refaktoring a proč ho potřebujete
---------------------------------------

Pojem refaktoringu zahrnuje soubor činností, které upravují vzhled a vnitřní logiku kódu programu bez vlivu na okolní prostředí. Proces refaktoringu si můžete představit třeba jako vánoční úklid - být zůstává stále stejný, ale je mnohem lépe organizován, a vyhodí se zbytečnosti. U refaktoringu je také důležité myslet na to, že nejde o jednorázovou akci, ale o dlouhodobý proces, který musíte v pravidelných cyklech opakovat. Pokud to neuděláte, čekají na vás různé podivné chyby, finanční ztráty, problém s onboardingem nových lidí a pláč.

Když se vám podaří udržet projekt stabilní a stále aktuální, získáte tím některé skvělé výhody:

- Projekt bude **bezpečnější**. V rámci knihoven, které používáte, vychází pravidelně opravy chyb a bezpečnostních zranitelností. Bezpečnost řešit musíte, patří k základním životním funkcím zdravého projektu.
- Kód i vnitřní architektura se zjednoduší a bude lépe promyšlená. Lépe se vám pak budou hledat chyby, řadě chyb dokonce dokážete předcházet.
- Díky zjednodušenému kódu můžete k vývoji přizvat i juniory, díky kterým se <a href="https://www.youtube.com/watch?v=1wZtdIb-Ppk">významným způsobem zlevní</a> celkový rozpočet.
- Možná zjistíte, že některé věci děláte až příliš složitě a překombinovaně - projekt se celkově zjednoduší.

Abyste s velkým digitálním projektem stáli na místě, tak musíte běžet. Ale vy chcete růst.

Jak refaktorovat bezpečně
-------------------------

Každý refaktoring je velká sázka, která se nemusí vyplatit.

Vždycky dlouho předtím, než se do refaktoringu pouštím, zajišťuji stabilní prostředí.

Pro stabilitu potřebujete zejména **funkční logování chyb**. Pro jednoduché projekty stačí <a href="https://tracy.nette.org">Tracy</a>, která loguje chyby do HTML souboru. U pokročilejších projektů přichází v úvahu nástroje typu <a href="https://sentry.io/welcome/">Sentry</a> nebo <a href="https://rollbar.com">Rollbar</a>. Osobně používám Tracy na všech projektech, další nástroje podle typu projektu.

Zhruba měsíc před refaktoringem začínám sledovat chyby a tvořím si tabulku známých chyb, na které se později zaměřím. Důležité je vždy vědět, které chyby přinesl refaktoring, a které už projekt obsahoval v minulosti. Pomůže to pak lépe obhájit výsledky práce před zadavatelem.

Jakmile díky logu vím, že pracuji na relativně stabilním prostředí, můžeme se pustit do tvrdé práce. Další nadpisy zahrnují popis metod podle toho, jak velké za nimi číhá riziko.

Oprava Coding Style a Coding standard
-----------------------------------

Většina projektů nemá jednotný styl formátování kódu. To je velká chyba. Dobře napsaný projekt vypadá jako od jednoho člověka, ve kterém jsou všechny věci napsány stejně.

Základní formální chyby vám dobře opraví nástroj <a href="https://doc.nette.org/en/3.1/code-checker">Nette Code Checker</a>, který pravidelně spouštím nad celým projektem.

Coding Style zase zahrnuje způsob formátování kódu a odsazení jednotlivých výrazů jazyka. Ve světě PHP se hodně používá standard <a href="https://www.php-fig.org/psr/psr-12/">PSR-12</a>, ze kterého vychází velmi mnoho dalších standardů. Doporučuji použít.

Některé projekty nešikovně kombinují <a href="/mezery-a-tabulatory">taby a mezery</a>. Abyste rozbití konvence bílých znaků (ale i dalších chyb) efektivně zabránili, na rootu projektu vytvořte soubor `.editorconfig`, který říká vašemu editoru, jak má formátovat nově napsaný kód.

Já osobně používám tuto konfiguraci:

```
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true

[*.{php,phpt}]
indent_style = tab
indent_size = 4
```

Taky opravte všechny <a href="/apostrofy-a-uvozovky">uvozovky na apostrofy</a>. Jde to i automaticky.

Kontrolu formátování kódu lze provádět i automaticky, připravil jsem k tomu <a href="https://github.com/baraja-core/sandbox/blob/0db1f41068b6cedf79500c6a8b0ac26eae94a8eb/.github/workflows/coding-style.yml">plně funkční ukázku na GitHubu</a>. Pokud potřebujete kód také automaticky opravit, lze to provést stejným nástrojem. Velmi dobře umí kód také automaticky opravovat přímo PhpStorm, a to i hromadně nad celým projektem.

Jak opravit Coding Style u rozsáhlých projektů
----------------------------------------------

U rozsáhlých projektů doporučuji automatické formátování kódu provádět postupně po modulech, protože si tím můžete v Gitu zařídit ohromné množství konfliktů. Nejlepší strategií proto je jediným velkým commitem opravit co možná nejvíc řádků, tento commit pushnout přímo do masteru a poté distribuovat změnu do ostatních větví.

Pokud jsou konflikty příliš velké, dává smysl nejprve zformátovat kód na masteru, a poté ještě jednou v každé velké větvi, kde je hodně změn. Velmi mnoho změněných řádků se mezi sebou vyruší (provede se fast-forward), takže budete řešit jen velmi málo konfliktů, nebo někdy i žádné konflikty.

Když neuděláte nic, kód bude pořád špatný. Iterativní přepisování v průběhu mnoha let rozhodně nefunguje, protože každý merge přináší potřebu opravy desítek řádků, kde se reálně nestala žádná změna a zbytečně to komplikuje jak code review, tak i potenciální reverty změn.

PhpStan - statická analýza kódu a oprava typových chyb
------------------------------------------------------

Ještě předtím, než se pouštím do rozsáhlé opravy logiky, je velmi důležité provést kontrolu a opravu **základních životních funkcí projektu**. Mezi ně patří takové samozřejmé věci, které od každého projektu očekáváte, ale i tak někdy nejsou splněny.

Například:

- Všechny třídy, metody a funkce musí existovat
- Nesmí být poškozena dědičnost tříd
- Třídy musí implementovat použité rozhraní nebo rozhraní předka. Zároveň nesmíte dědit `final` třídy
- Nesmíte volat nebezpečné funkce typu `eval()`, `shell_exec()`, `var_dump()` a podobně. A pokud je i tak voláte, musí to být explicitně uvedeno v komentáři
- Vždy musíte odchytávat výjimky a nenechat spadnout celou aplikaci

Řešení tohoto problému je nainstalovat do projektu <a href="https://phpstan.org">PhpStan</a> a ten opravit **aspoň na úroveň 1**. Ano, je to těžké, a ano, je to hodně práce. Pokud to ale neuděláte, tak se každý refaktoring stává ruskou ruletou a vývojář jenom doufá, že škody budou co nejmenší.

Základní úroveň PhpStanu nevyžaduje, aby například všude existovaly datové typy a další formální věci, které budeme řešit až v budoucnu. Na druhou stranu se ale nesmí stát, že bude funkce nebo metoda vracet určitý datový typ, ale v typehintu tvrdit něco jiného.

Rector - bezpečná iterativní oprava
-----------------------------------

Známá programátorská poučka říká, že před přidáním nové chyby do systému (třeba vyhození výjimky) bychom tuto chybu nejdřív měli přidat jako varování, a když se dlouhobě neprojeví, tak ji až pak změníme na fatální chybu.

Stejně to je třeba s datovými typy. Při refaktoringu neznámého kódu nejprv nechávám automatické nástroje typu <a href="https://getrector.org">Rector</a> nejprve do kódu doplnit komentářové anotace, které nic nerozbíjí, ale pomáhají si vyjasnit, co kde později bude. Na tyto komentáře pak poslouchá PhpStan, kterým lze bezpečně ověřit, že jsme nic nerozbili a jde o bezpečnou úpravu.

Obecně komentáře k properties a argumentům přidávám v jednom commitu, poté třeba měsíc vyčkávám, a když vše dlouhodobě funguje, uchyluji se k přepisování na pevné datové typy na místech, kde dlouhodobě nebyl žádný problém.

Podívejte se na článek <a href="https://tomasvotruba.com/blog/2019/07/29/how-we-completed-thousands-of-missing-var-annotations-in-a-day/">How we Completed Thousands of Missing @var Annotations in a Day</a>.

Více o <a href="https://github.com/rectorphp/rector">Rectoru najdete na GitHubu</a>.

Aktualizace závislostí
----------------------

Ještě předtím, než se pustíme do aktualizace závislostí na balíky, nebo dokonce verzi PHP, je důležité prozkoumat a naučit se používat <a href="/github-actions-nejlepsi-ci-pro-rok-2021">automatické testy</a>.

Pokud testy fungují, můžeme se pustit do nástroje <a href="/composer">Composer</a>, kterým provedeme aktualizaci. Pokud můžete, tak si na pomoct přizvěte robota <a href="https://dependabot.com">Dependabot</a>, který váš projektový `composer.json` aktualizuje automaticky a umí ověřit kompatibilitu změn.

Pokud vás čeká velká sada úprav, vždy je provádějte pomalu a navyšujte po jednotlivých verzích. Nikdy neaktualizujte mnoho balíků najednou. Po každé aktualizaci proveďte scan celého projektu PhpStanem a opravte chyby. Je to dlouhý proces, který trvá několik hodin, ale v sázce je opravdu hodně.

Kam dál
-------

Další kroky jsou individuální podle typu a stavu projektu. Obecně platí, že dobře navržený kód psaný seniorními vývojáři se udržuje řádově lépe, než levně koupený kód od juniora, který pracoval v mediální agentuře, která má vývoj webů spíše jako bokovku, aby se neřeklo.

Držím vám palce! Bude to těžké, ale vy to zvládnete.
