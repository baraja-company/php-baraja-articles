Čo by som zmenil na Nette, keby som bol David Grudl
===================================================

> id: bad8fe4f-92a4-4396-8698-7ec42bb2aef8
> slug:
> 	- null
> 	sk: co-by-som-zmenil-na-nette-keby-som-bol-david-grudl
> 
> cs: co-bych-zmenil-na-nette
> publicationDate: '2022-11-18 17:45:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '953e6c78b9dff185bbf8e3e9582d24c3'

Nette Framework aktívne používam už takmer 6 rokov na vývoj komerčného softvéru. Počas prvých rokov som bol veľmi nadšený a rámec dokonale riešil všetky potreby, ktoré sme ako tím mali, a v podstate nebol dôvod hľadať iný nástroj.

Približne od roku 2019 začínam pozorovať určitý ústup pôvodného nadšenia v rámci Nette, kde mi začínajú chýbať niektoré pokročilé funkcie. Často ide o veci, ktoré sú mimo samotného frameworku, takže ani neočakávam, že budú niekedy implementované, ale na druhej strane sa vývojár musí rozhodnúť, ako pokračovať vo vývoji ďalej, a možno siahnuť po inej technológii.

Vrstva databázy
-----------------

Databázu Nette považujem za jednu z najväčších chýb rámca, bohužiaľ.

Každý týždeň sa v spoločnosti konajú pohovory, na ktoré sa hlásia mladí ľudia, ktorí práve skončili školu, alebo aj ľudia na strednej úrovni, ktorí prechádzajú z iného rámca, a objavujú databázu Nette ako súčasť ich skúmania Nette. Ako databázová vrstva to nie je zlé, ale problémom je potom praktické využitie v reálnych projektoch.

Je to preto, že Nette Database sa nesnaží tlačiť vývojárov k tomu, aby od začiatku používali striktné dátové typy, aby pomenovávali stĺpce a vzťahy, aby oddeľovali metódy dopytovania (úložiska) od modelov a ďalšie drobné nuansy, ktoré celkovo robia veľa.

Dlhé roky som používal knižnicu Dibi, ktorá vo svojej dobe zohrávala obrovskú úlohu. Umožňovalo to celkom pekne vyhľadávať v databáze a zjednotiť rozhranie. Na druhej strane, doba pokročila a keď databázy vracajú netypované objekty, PHPStan jednoducho nebude spokojný už len z princípu.

Osobne by som buď začlenil natívnu podporu pre entity a úložiská do databázy Nette, alebo by som poskytol oficiálnu podporu pre Doctrine v rámci Nette. Ak programujete aplikácie na úrovni veľkej korporácie, musíte to s vývojom myslieť vážne a najmä Doctrine má veľký zmysel. Zároveň chcete nováčikov hneď naučiť lepším technológiám a nechcete ich učiť starým zvykom.

Tracy a zaznamenávanie chýb
---------------------

Tracy stále považujem za najlepší runtime debugger pre PHP.

Keď som v roku 2017 prišiel do jednej nemenovanej digitálnej agentúry a videl som technický stav ich projektov Nette, bol som zhrozený. Koľkokrát mali stránky napísané v čistom PHP bez akéhokoľvek pokusu o zaznamenávanie chýb. Pomerne jednoduchým riešením bolo nainštalovať Tracy do projektu, zavolať `Debugger::enable()` a nič viac nerobiť. Chyby sa automaticky zaznamenávali do adresára, ktorý bolo potrebné každých niekoľko dní ručne vybrať.

Čo sa týka Tracyho, zdá sa mi, že je to hotový produkt, ktorý David nemá dôvod vylepšovať. Na druhej strane stále vidím priestor na zlepšenie v novom smere.

Prečo napríklad Tracy neobsahuje platené funkcie pre spoločnosti alebo jednotlivcov, ktorí to so zabezpečením myslia vážne?

Spoločnosť Tracy by mohla implementovať vlastné webové rozhranie typu Sentry, v ktorom si vytvoríte účet a výrobné chyby sa automaticky odošlú prostredníctvom rozhrania API, kde ich môžete sledovať v rámci projektov. Aplikácia by tiež mohla požiadať o jednotlivé stránky a automaticky zistiť, že sú nedostupné, a upozorniť majiteľa iným spôsobom (keďže Tracy nemôže logicky zistiť pád servera). Niekedy sa stretávam aj s problémom, že Tracy je náhodne povolený na produkčnej lokalite a prostredníctvom DIC môžete zistiť, ako získať prístup k databáze alebo inde. Na to by vás mala upozorniť aj Tracy.

Tracy mohol implementovať aj ďalšie drobnosti, ktoré poznám z Node.js. Napríklad pre zobrazenie zdrojového kódu by mohlo byť možné zvoliť interval znakov, v ktorom sa riadok zvýrazní špeciálnym spôsobom, prečo je možné zvýrazniť konkrétny token v rámci riadku. Zároveň by bolo pekné pridať podporu pre druhý (možno modrý) zvýrazňovací riadok na zobrazenie kontextu v ďalšej časti aplikácie.

Pri zobrazovaní úryvku zdrojového kódu by som sa nebál pridať tlačidlo, ktoré dokáže vykresliť zvyšok kódu v ajaxe, aby som ho mohol preskúmať priamo v prehliadači z callstacku. Symfony to robí a je to skvelá funkcia. Ak by sa to doplnilo základnou statickou analýzou s možnosťou prechádzať metódy, triedy a dedičnosť, ušetrilo by to veľa práce pri ladení celému tímu.

Ak zásobník volaní prechádza balíkom, bolo by dobré zobraziť verziu balíka, s ktorou pracujem pre konkrétny súbor. Často sa mi stane, že v balíku, ktorý vyvíjam, sa vyskytne chyba a ja môžem aj vizuálne vedieť, že ide o staršiu verziu.

Statické smerovanie
----------------

V rámci Nette Router by som umožnil definovať nový typ s názvom statický router. Bol by to potomok základného smerovača, ale dokázal by akceptovať iba reťazec-literál, čo by nebol regulárny výraz, ale priama cesta. Veľa stránok funguje staticky (kontakty, rôzne články, prekvapivo často aj domovská stránka) a smerovač musí kvôli tomu zakaždým vyhodnocovať regexové pravidlá pre zhodu a zloženie URL.

Ak by sa napríklad v šablóne Latte používala statická trasa, mohla by sa v čase kompilácie šablóny bezpečne preložiť na statický reťazec `{$basePath}/path`, takže by nebolo potrebné pri každej požiadavke kompilovať napríklad 100 adries URL, ktoré sú v rozložení.

Chápem, že rovnakú funkčnosť môžem dosiahnuť pomocou ukladania do vyrovnávacej pamäte na strane šablóny, ale oveľa viac by som ocenil systémové riešenie.

V rámci Next.js som úplne prepadol konceptu statického smerovania. Neexistuje nič také ako `n:href`, skrátka musíte vopred poznať adresu URL, čo vás núti nerobiť toľko presmerovaní, vopred premýšľať o formátoch URL a udržiavať ich trvalé.

Rozhranie PSR
------------

Je škoda, že Nette neimplementuje rozhranie PSR natívne. Ako vývojára balíkov vás to vedie na scestie, ak chcete vôbec definovať závislosť Composeru od Nette. Na jednej strane vám Nette vyrieši veľa problémov, na druhej strane viete, že ho potom jednoducho nenahradíte. Už viackrát som sa pristihol, že kvôli absencii všeobecného rozhrania radšej používam Symfony, Laravel alebo úplne iné všeobecné rozhranie.

Nedostatočná podpora generických štandardov pre budúcu prenosnosť je hlavným dôvodom, prečo som v roku 2022 od Nette úplne odišiel a nové aplikácie vyvíjame v tímoch výlučne v generickom jazyku.

Vrstva API
----------

Čo ak chcete použiť Nette ako backend na spracovanie požiadaviek REST API? Vytvoríte prezentér a odošlete odpoveď ako `$this->sendJson()`? Budete inštalovať knižnicu tretej strany? Alebo ste David Grudl, ktorý rieši potrebu chýbajúcej knižnice tým, že hneď napíše novú?

Prečo spoločnosť Nette nemôže implementovať niečo tak bežné, ako je rozhranie REST API, hneď po vybalení? V súčasnosti takmer každá aplikácia potrebuje komunikovať prostredníctvom rozhrania API - napríklad na poskytovanie údajov do frontendu.

Nováčikov to opäť vedie k tomu, aby používali vstavaný Latte a fragmenty namiesto toho, aby zistili, že existuje lepšia možnosť, a to vytvoriť celý frontend samostatne a len mu poskytovať údaje.

Dôvera v to, čo sa stane za 10 rokov
-------------------------

Posledný bod je veľmi pragmatický a vychádza z debaty, ktorú z času na čas počúvame od obchodných oddelení v tíme.

Čo by sa stalo, keby David Grudl úplne zastavil vývoj Nette? Čo keby ho niekto prestal vyvíjať? Na čo by sme prešli? A aké náročné by to bolo?

Mnohí vývojári (často bez skúseností z podnikovej sféry) sa tejto obave radi vysmievajú a tvrdia, že je to v poriadku. Akože áno, ale nie je.

Keď riešite otázku, či investovať čas do Nette alebo Reactu a Node.js na školenie juniorských vývojárov vo vašej spoločnosti, bohužiaľ vyhráva druhá možnosť.

Nette ešte neušiel vlak, ale v desiatkach rozhovorov, ktoré som absolvoval za posledných šesť mesiacov, to cítim dosť silno.
