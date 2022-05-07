Ako žije programátor v internom vývojovom tíme
==============================================

> id: '9309b0f2-0265-43aa-a9c3-10b2d486cdfc'
> slug:
> 	cs: jak-zije-programator-v-internim-tymu
> 	sk: ako-zije-programator-v-internom-vyvojovom-time
> 
> publicationDate: '2022-01-10 12:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: e65dd78fa9813b552f30b98f04aacb30

Za úprimnosť sa platí vysoká cena.

Táto stránka vždy opisovala realitu, ktorú ľudia v IT zažívajú, preto by som sa rád pozrel na svoje skúsenosti s prácou vo vývojových tímoch. Nižšie sú uvedené všeobecné skúsenosti, ktoré som získal v rôznych spoločnostiach. Žiadna skúsenosť nie je spojená s jednou konkrétnou spoločnosťou a nemusí slúžiť ako kritika.

Spoločnosti nemajú tendenciu chcieť pracovitých a proaktívnych ľudí
----------------------------------------------

Máte veľa nápadov? Chcete inovovať? Baví vás hľadať elegantné riešenia zložitých problémov, ktoré váš tím rieši a ktoré trápia polovicu spoločnosti? Uvedomujete si dôležitosť bezpečnosti, návrhu softvéru a hľadania úzkych miest projektu?

Pravdepodobne budete vo vývojovom tíme dlho nešťastní.

Tímová práca je niečo, s čím v poslednom čase veľmi bojujem - teda konkrétne v nej plávam a snažím sa pochopiť pre mňa úplne neintuitívne princípy, ktoré treba dodržiavať.

- Tímová práca znamená vytvoriť riešenie, ktorému rozumie celý tím. Často to nie je to najlepšie. Takmer nikdy to nie je elegantné. V skutočnosti je to vždy neefektívne. Jeho výhodou však je, že mu rozumie celý tím a dá sa dlhodobo riadiť. Ide o mimoriadne dôležitú myšlienku. Pri veľkých projektoch totiž nie je taký tlak na efektivitu, pretože ako spoločnosť môžete škálovať prostredníctvom ľudí a máte dostatok peňazí. Oveľa dôležitejšie je dodať veci načas, aj keď čiastočne rozbité a s vopred neznámymi obmedzeniami, ale načas. Súvisí to s myšlienkou, že riešenie, ktoré dodáte zajtra (aj keď nedokonalé), má pre zákazníka oveľa väčšiu hodnotu ako stopercentne odladené riešenie, ktoré tu bude až o rok.
- Inovácia a presadzovanie vecí je skôr nesprávne. Súvisí to s tým, že v podnikoch pracujú skutoční ľudia, ktorí majú rodiny a chcú pracovať len počas pracovnej doby. Z toho nevyhnutne vyplýva, že ľudia majú obmedzený čas na učenie sa a skúšanie nových vecí. Podniky v podstate musia do pracovného času ľudí vtesnať vzdelávanie a rozvoj, ktoré sa dajú využiť v budúcej práci. Zaujímavým dôsledkom je potom odkladanie rozhodnutí a učenia sa nových vecí na potrebný čas. Pokiaľ ide o mňa, tomuto prístupu rozumiem a dáva mi veľký zmysel. Ak by som mal zamestnancov, tiež by som uprednostnil pokojných ľudí, ktorí vo firme vydržia napríklad 15 rokov, pred ľuďmi, ktorí majú veľký všeobecný prehľad a chcú sa posunúť ďalej, ale bude to na úkor tímu. Je pravda, že kolektívne riešenie nikdy nebude rovnaké ako individuálne, ale to neznamená, že bude horšie.

Ľudia ako ja sú potenciálnym zdrojom problémov a konfliktov. Je super o tom takto premýšľať.

Pri prehliadaní kódu hľadajte objektívne chyby
----------------------------------------

Každá spoločnosť má svoje pravidlá. Bohužiaľ. Zo začiatku ma to veľmi rozčuľovalo.

Ak používate niektorý z vyspelejších jazykov, ako je napríklad .NET, pravidlá kódového štýlu bývajú podobnejšie. Až potom zistíte, že na svete stále existuje PHP a JavaScript. Najmä PHP je niekedy trochu frustrujúce. PHP je veľmi vyspelý jazyk s mnohými skvelými funkciami, ale podľa mojich skúseností sa vo firmách využíva asi na tretinu svojho celkového potenciálu. Dôvody bývajú rôzne, najčastejšie ide o zotrvačnosť.

V tejto súvislosti som dlho hľadal procedurálne riešenie na kontrolu kódu. Už dlho sa hrám s PhpStan a sledujem myšlienky okolo neho. Základnou myšlienkou programu PhpStan je, že upozorňuje len na objektívne chyby, ktoré sa určite niekedy vyskytnú. Čím viac skúmam PhpStan a posúvam ho na vyššiu úroveň, tým viac si vnútorne overujem, aká je to pravda.

Bolo by pekné, keby bol PhpStan implementovaný aspoň v polovici českých firiem. Ešte lepšie by bolo, keby to bolo aspoň na úrovni 6. V praxi som videl, že najlepšie spoločnosti majú úroveň 4 alebo 5.

Práve nedávno som rozmýšľal, či môže existovať reálne fungujúca produkčná aplikácia, ktorá má vlastný vývojový tím a neprechádza ani úrovňou 0 - to je úroveň, ktorá kontroluje veci, ktoré by ste očakávali v každej aplikácii. Niečo v zmysle uistenia sa, že všetky triedy a funkcie existujú, súbory nemajú chyby pri analyzovaní a podobne. A áno, takéto aplikácie existujú. Z dlhodobého hľadiska sa však situácia zlepšuje. Dúfajme, že.

Takže podobný prístup uplatňujem aj v prípade codereview. Hlásim len veci, ktoré sú objektívne vždy nesprávne. Často sa stretávam s nesprávnym posúdením stupňa - niečo nie je v poriadku, ale opäť to nie je taký veľký problém, aby sa to muselo riešiť. Mnohé problémy sa riešia konvenciou alebo vyhlásením, že riziko je malé, takže nemá zmysel ho riešiť.

Chýbajúce typy údajov (aj zložené) som vždy považoval za kritickú chybu. Potom som pochopil, že existuje test a výpis.

Nemyslím si, že mám konkrétny záver k tejto časti. Mám k tomu len veľa subjektívnych pripomienok, ktoré budú skôr zdrojom vzájomného nepochopenia, takže ich nebudem zverejňovať. Z dlhodobého hľadiska sa prikláňam k záveru, že nemôžem robiť codereview - buď som príliš prísny, alebo príliš benevolentný. Na všeobecnej úrovni nedokážem povedať, čo je naozaj dôležité a čo nie. Docela by ma zaujímalo, ako to robia ostatní. Nikto mi zatiaľ nedokázal dať odpoveď, ktorú by som mohol použiť aj ja.

Vývoj na zákazku nie je finančne náročný.
---------------------------------

Máte agentúru a robíte vývoj webových stránok na zákazku? Ak nie ste dostatočne veľký a nerobíte veľké projekty pre iné spoločnosti, pravdepodobne jedného dňa nebudete existovať.

Dôvodom je nedostatočné financovanie projektov na mieru. Pravdepodobne to súvisí s charakterom zmlúv, ktoré sú pre kupujúcich výhodnejšie. V skutočnosti je väčšina agentúr brutálne podfinancovaná a reálny zisk z nich majú len ich majitelia.

Keď interne vyvíjate projekt ako spoločnosť pre seba, oneskorenie dodávky o mesiac pravdepodobne až tak nevadí. Ak ako agentúra nedodáte zákazku do mesiaca, máte istotu, že v spomínanom mesiaci budete v práci pomaly spať, sústavne si ničiť zdravie, klient bude nadávať do telefónu a lístkov, stále sa pýtať, či by to nemohlo byť rýchlejšie, a za odmenu ešte zaplatíte zmluvnú pokutu. A ak to neurobíte, klient vás označí za nespoľahlivého dodávateľa a môže uvažovať o tom, že pôjde inam, kde to aj tak nebude lepšie.

Ale toto sú pravidlá hry, ktoré som spoznal v praxi a ktoré mi urobili dieru do rozpočtu.

Uzatváranie zmlúv je pravdepodobne najzložitejšou formou podnikania v oblasti IT. Nechcete uzatvárať zmluvy. Uzatváranie zmlúv vás zničí. Zavolajú vám cez víkend, v noci, na Vianoce. Za polovicu práce, ktorú vykonáte, nedostanete zaplatené. Budete sa ospravedlňovať za chyby, ktoré nemôžete urobiť. Na Instagrame sa budete pozerať na rozprávanie priateľov, ktorí tento rok odišli už na tretiu dovolenku, zatiaľ čo vy budete v sobotu o tretej ráno sedieť v kancelárii a dokončovať projekt klienta, za ktorý vám v pondelok aj tak vynadajú, pretože v zmluve bolo viac, ako ste stihli urobiť. Ľudia si budú brať dovolenku a nemocenskú a vy budete pracovať namiesto nich. Alebo dostanete výpoveď. A potom budete radi platiť nájomné.

Ale na druhej strane sa veľa naučíte. Práve to, že ste pod tlakom, na vás vyvíja veľký tlak, aby ste sa učili a boli efektívni, aby ste nabudúce dokázali urobiť o niečo viac za rovnaký čas. Zlé je, že tieto znalosti a skúsenosti nemôžete uplatniť inde, pretože firmy o ne jednoducho nemajú záujem (vysvetlil som vyššie).

Našťastie je to príbeh z roku 2019 a je to ešte ďaleko.

Nikdy nebude existovať ideálny svet
-------------------------

Ak robím to, čo ma baví? Vy áno? :D

Myslím, že je to všetko otázka miery. Pravdepodobne nikdy nebudete robiť prácu, ktorá by vás stopercentne napĺňala. Pravdepodobne vám vždy niekto bude vysvetľovať, že nemôžete robiť to, čo ste sa 10 rokov násilne učili a vnútorne viete, že môžete.

Na druhej strane, za určitú dávku popretia vlastnej osobnosti získate priestor na niečo viac. Napríklad víkendový čas, lepšie zdravie, viac času pre seba, stabilné prostredie atď. To, že sa to nedá dlhodobo udržať, je tiež zrejmé.

Páči sa mi, že môžem vyskúšať rôzne veci a zažiť z prvej ruky, ako to majú ostatní. Práca vo vývojovom tíme je v porovnaní s prácou na zákazku obrovská nuda.

Už to nie je o hľadaní najlepšieho a najrýchlejšieho riešenia, ale o spolupráci. Ide o zlepšenie komunikácie, emócií a hlavne o to, aby ste sa naučili byť ľuďmi. A to je tiež obrovská hodnota.
