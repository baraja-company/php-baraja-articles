Jedného dňa hackeri zaútočia na vaše webové stránky
===================================================

> id: a11ca0a6-77e1-4ba4-8eca-83449c9cbf48
> slug:
> 	cs: jednou-vam-hackeri-napadnou-web
> 	sk: jedneho-dna-hackeri-zautocia-na-vase-webove-stranky
> 
> publicationDate: '2020-08-04 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '0f0ac4c091115e9c1f0833d318f34f5d'

To sa vám nestane? :DDDDDDD

Pri správe viac ako 300 webových stránok sa z času na čas dostávam do rôznych núdzových situácií. Niekedy sú dosť vyhrotené, ale často ide o úplnú banalitu. Ak ste sa podobne ako ja v minulosti nechali zlákať programovaním a tiež viete, že [programovanie je otrava](http://borisovo.cz/programming-sucks-cz.html), určite so mnou budete súhlasiť.

V zajatí zlých hackerov
----------------------

Keď sa webová aplikácia stane populárnou, stane sa lákavým cieľom pre hackerov. Ich motiváciou zvyčajne nie je zničiť celú webovú lokalitu, práve naopak. V skutočnosti **hackeri chcú, aby ste o nich ako správca servera vôbec nevedeli**. Dobrý hacker čaká celé mesiace, sleduje webový server a získava to najcennejšie - vaše údaje.

Na ochranu používateľov je nevyhnutné šifrovať všetky údaje a mať viacero vrstiev ochrany. Pre heslá použite niektorú z pomalých funkcií, napríklad [Bcrypt + soľ](https://php.baraja.cz/hashovani), Argon2, PBKDF2 alebo Scrypt. O [bezpečnostnom hodnotení] už písal Michal Špaček(https://pulse.michalspacek.cz/passwords/storages/rating#slow-hashes), ktorému ďakujem za doplnenie článku. Ako majiteľ webu musíte pri diskusii s programátorom trvať na správnom hashovaní hesla. Inak budete plakať, tak ako mnohí ľudia pred vami, ktorí si tiež namýšľali, že sa ich to netýka.

Viete rozpoznať, kedy na vás útočia?
---------------------------------

Všimol som si, že keď webový server dosiahne určitú hranicu návštevnosti (v Českej republike je to asi 50+ návštevníkov denne), začne naň z ničoho nič smerovať séria útokov. Aby to nebolo jednoduché, útočník má vždy výhodu, pretože si môže vybrať, na ktorú časť infraštruktúry začne útočiť, a vy - ako vlastník webového servera - to musíte včas rozpoznať, brániť sa a nabudúce sa na to lepšie pripraviť.

Koľko útokov na vás bolo napríklad za posledný mesiac? Viete presne? Koľko z nich ste bránili? Má niekto iný prístup k vášmu serveru?

Čo ak na vás práve teraz niekto útočí? A teraz... a teraz aj...?

Nanešťastie neexistuje univerzálny spôsob, ako rozpoznať útok. Existujú však nástroje, ktoré vám môžu poskytnúť značnú výhodu.

Čo sa mi v poslednej dobe najviac osvedčilo:

- **Keď útočník nevie, kde sa vaše servery fyzicky nachádzajú**, má oveľa ťažšiu pozíciu. Informácie o fyzickej architektúre možno veľmi dobre zakryť pomocou služby [Cloudflare](https://www.cloudflare.com/), ktorá sprostredkúva (a obojsmerne šifruje) všetku sieťovú komunikáciu. Má aj množstvo ďalších výhod, napríklad rýchlejšie načítanie zo zahraničia, automatickú ochranu pred útokmi DDOS, kompresiu aktív a v neposlednom rade automatické blokovanie nepoctivých návštevníkov. Cloudflare používam pre všetky svoje webové stránky (a takmer každý projekt používa iné technológie).
- **Zaznamenávanie chýb**. Vždy a všetko. Je pravdepodobné, že vaša aplikácia obsahuje veľa chýb, ktoré spáchal váš programátor. Či už ide o [uncaught exceptions](https://php.baraja.cz/vyjimky), chyby aplikácie, SQL injection a podobne. Ak programujete v PHP a nerozumiete logovaniu, existuje dosť problémov, ktoré zachytí [Tracy](https://tracy.nette.org/) (a prípadne ďalšie nástroje ako [Sentry](https://sentry.io/)) a logy prístupu na samotnom serveri. Zaznamenávanie chýb nie je príjemné, ale nevyhnutné. Chyby zaznamenávam na všetkých stránkach, ktoré aktívne spravujem, a informácie o nových chybách si nechávam posielať na svoj e-mail, aby som o nich okamžite vedel.
- **Bezpečná a aktualizovaná platforma**. Máte na svojej stránke WordPress? Viete, ako ho správne aktualizovať? Poznáte všetky riziká, ktorým čelíte? Keď je niečo zadarmo, takmer vždy je to na úkor niečoho iného. Osobne používam na vývoj aplikácií [Nette framework](https://nette.org/cs/) verziu 3, ktorú týždenne aktualizujem a aktívne hľadám bezpečnostné chyby, ktoré treba opraviť. Tak ako pravidelne vykonávate servis svojho auta, musíte pravidelne, aspoň raz za mesiac, vykonávať servis svojej webovej stránky. Údržba lokality zahŕňa aktualizáciu všetkých externých knižníc a dôsledné monitorovanie protokolov. Ak používate starú verziu knižnice, je veľmi pravdepodobné, že útočník o zraniteľnosti vie a pokúsi sa ju zneužiť.
Otázkou je, kedy
Veľké množstvo ľudí v mojom okolí neuveriteľným spôsobom podceňuje bezpečnosť a vystavuje na internete obsah, ktorý je veľmi ľahké prelomiť a zneužiť.

Dokonca aj veľké spoločnosti robia chyby, a to aj v takých triviálnych veciach, ako je útok [XSS](https://www.zive.cz/clanky/vyvojar-objevil-zranitelnost-v-seznamu-dokazal-mezi-vysledky-propasovat-zakazany-kod/sc-3-a-200023/default.aspx) (cross-site scripting).

Otázkou nie je, či niekto niekedy rozbije vašu webovú stránku. Otázkou je, kedy sa to stane a či budete dostatočne pripravení na to, aby ste sa s tým vyrovnali. Možno má hacker prístup k vášmu serveru už niekoľko mesiacov a vy o tom (zatiaľ) neviete.

Čo robiť, keď sa vyskytnú problémy
-------------------------------

V čase, keď sa o práci hackera dozviete, je už zvyčajne neskoro a hacker urobil všetko, čo potreboval. Veľmi pravdepodobne umiestnil škodlivý softvér na celý server a má nad ním aspoň čiastočnú kontrolu.

Ide o **chaotický typ problému a musíte konať rýchlo**.

Keďže ide o poslednú fázu útoku, osobne odporúčam webový server úplne odpojiť od internetu a začať postupne zisťovať, čo všetko sa stalo. Navyše sa nikdy presne nedozvieme, či server neskrýva niečo iné. Útočník má vždy značný náskok.

Ak sa rozhodneme vrátiť na server poslednú funkčnú zálohu, veľmi tým pomôžeme útočníkovi. Už vie, ako sa vlámať do vášho servera, a nič mu nebráni v tom, aby to za niekoľko hodín urobil znova. Okrem toho záloha pravdepodobne obsahuje škodlivý softvér alebo iný typ vírusu.

Aj keď je to medzi mojimi klientmi veľmi nepopulárna možnosť, osobne vždy odporúčam najprv **úplne odstrániť stránky WordPress** alebo akékoľvek systémy, ktoré klient neaktualizuje a ktoré majú veľa bezpečnostných hrozieb. Ak napríklad na jednom serveri hostíte 30 lokalít starších ako 2 roky, máte prakticky istotu, že aspoň jedna z nich obsahuje nejakú formu zraniteľnosti.

Ak problému nerozumiete, radšej sa hneď na začiatku obráťte na vhodného odborníka, ktorý má hlboké znalosti o vašom probléme a chápe veci v širšom kontexte.

**Etická poznámka:**

Ak spravujete webovú lokalitu pre klienta, u ktorého došlo k bezpečnostnému incidentu, je vašou povinnosťou ho o tom informovať. Ak dôjde k úniku akýchkoľvek údajov (napríklad hesiel, ale často aj oveľa viac), musíte tiež informovať koncových používateľov, že ich heslo je verejne známe a mali by si ho všade zmeniť. Ak použijete pomalú hashovaciu funkciu (napríklad **Bcrypt + soľ**), výrazne tým útočníkovi sťažíte prelomenie hesla. (Bohužiaľ, [Bcrypt passwords can be cracked](https://arstechnica.com/information-technology/2015/08/cracking-all-hacked-ashley-madison-passwords-could-take-a-lifetime/)). Ak chcete pravidelne dostávať informácie o tom, či vaše konto uniklo, odporúčam zaregistrovať sa na stránke [Have i been pwned?](https://haveibeenpwned.com/). Viac informácií o [narušení bezpečnosti](https://m.uoou.cz/vismo/zobraz_dok.asp?id_ktg=5020&n=poruseni-zabezpeceni) nájdete na webovej lokalite OOOO.

Atómové návyky
--------------

Za posledných šesť mesiacov som dočítal knihu [Atomic Habits](https://www.melvil.cz/kniha-atomove-navyky/), ktorá mi otvorila oči. Opisuje proces postupného zlepšovania o 1 % každý deň, ktorý z dlhodobého hľadiska prinesie veľa.

Keďže sa na mňa obracajú spoločnosti a jednotlivci v rôznych fázach technologického dlhu, rád by som zhrnul tie najhoršie veci:

- **FTP pre komunikáciu so serverom nemá zmysel**. Ide o nezabezpečený protokol, ktorý je riadený heslom. Ak chcete niekomu poskytnúť prístup, musí poznať heslo a môže robiť to isté, čo vy. Ak mu chcete odobrať prístup, nemôžete, jedinou možnosťou je zmeniť heslo. Lepšie je úplne prejsť na SSH a používať kľúče RSA. Často ma prekvapuje, že tento problém dnes riešia aj firmy. Nikdy nevytvárajte jednu tabuľku všetkých hesiel. A určite nikdy nie spoločné pre celý tím!
- Zdrojový kód patrí do systému Git**, údaje do databázy a statický obsah (obrázky, PDF, ...) do súborov na disku. Výber nesprávneho úložiska údajov zhoršuje dizajn a bezpečnosť aplikácie. Adresa URL na súbor PDF (napríklad s faktúrou) by mala obsahovať náhodne vygenerovaný obsah, ktorý pozná len príjemca. V prípade mimoriadne citlivého obsahu (napríklad výpisu z účtu) je dôležité obsah zašifrovať na disku a poskytnúť ho až po prihlásení.
- Neverejné časti stránky musia obsahovať hlavičku META `noindex` a `nofollow`, aby ich Google neindexoval. Citlivý obsah, ktorý nesmie poznať vaša konkurencia, musí byť chránený heslom.
- Zakázanie [výpisu obsahu adresára](https://www.simplified.guide/apache/disable-directory-listing) na vašom serveri. Pri nevhodnom nastavení môže byť celá lokalita strojovo prehľadávaná s cieľom nájsť citlivé súbory.
- Projekt root! Nikdy nesmie existovať priama adresa URL na konfiguračné súbory. Napríklad [Nette to robí správne](https://doc.nette.org/cs/3.0/quickstart/getting-started#toc-obsah-web-projectu) hneď v prvom návode. To isté platí aj pre [verejne dostupné repozitáre git](https://smitka.me/open-git/). Tento typ zraniteľnosti je veľmi ľahké podceniť a následky bývajú tragické.
- Chyba môže vzniknúť aj v dôsledku neúmyselnej chyby pri vývoji. Je veľmi dôležité, aby každú zmenu preskúmal živý človek. Mnoho chýb sa odhalí pomocou [PHPStan](https://github.com/phpstan/phpstan) alebo podobných nástrojov skôr, ako kód uvidí človek.
- Nikdy [neposielajte heslá v čitateľnej podobe e-mailom](https://www.lupa.cz/clanky/reset-a-poslani-hesla-v-citelne-podobe-e-mailem-nebezpecna-praktika/), vždy existuje iný spôsob riešenia.
- Bezpečnostné problémy v starých verziách knižníc a softvéru. Roboty prehľadávajú internet každú sekundu a útočia na známe bezpečnostné diery (SQL injection, XSS, CSRF, ...). Veľa známych dier má staršie verzie WordPress.

Zraniteľných miest je oveľa viac a problémy sa líšia od projektu k projektu. Ak potrebujete urobiť rýchly audit servera, odporúčam použiť [Maldet](https://www.rfxn.com/projects/linux-malware-detect/) a potom si najať vhodného odborníka, ktorý vám pomôže urobiť [úplný audit webu](https://baraja.cz/audit-webu), a to nielen z bezpečnostných dôvodov.

Bezpečnostní inžinieri sú ako plast v oceáne - jednoducho navždy. Zvyknite si na to.

Princíp akcie a reakcie v prírode
-------------------------------

Určite ste si všimli, že príroda takmer vždy využíva princíp reakcie. To znamená, že sa niečo deje v určitom prostredí a okolité organizmy na to reagujú rôznymi spôsobmi. Tento prístup má mnoho výhod, ale jednu obrovskú nevýhodu - vždy zostanete pozadu.

Ako mysliaci človek (dizajnér, vývojár, konzultant) máte veľkú výhodu, a tou je akcieschopnosť - to znamená, že vopred poznáte určitú časť veľkých rizík a aktívne im predchádzate. Možno nikdy nezabránite všetkým chybám, ale môžete aspoň zmierniť ich následky alebo vytvoriť kontrolné mechanizmy, ktoré včas odhalia problémy, aby ste mali čas reagovať.

Väčšina útokov sa odohráva počas dlhého časového obdobia, a ak sa prihlásite, zvyčajne budete mať dostatok času na odhalenie problému.
