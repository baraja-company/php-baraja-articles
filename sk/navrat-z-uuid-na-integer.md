Návrat z UUID na celé číslo
===========================

> id: d842517b-c4f6-4cef-a839-d08ff3804fca
> slug:
> 	cs: navrat-z-uuid-na-integer
> 	sk: navrat-z-uuid-na-cele-cislo
> 
> publicationDate: '2021-05-02 17:00:00'
> mainCategoryId: '95374429-e651-46bd-9149-15aa716f8207'
> sourceContentHash: b7c136f3e3ca8a4563230c557ab1fa9c

Pri vývoji softvéru sa programátor často dostane do slepej uličky, keď musí prijať architektonické rozhodnutie, ktoré bude mať obrovský vplyv na budúcnosť jeho práce na desiatky rokov. Zároveň je to rozhodnutie, ktoré sa nedá zvrátiť, a za každú chybu sa platí drahá cena. Databázy sú typickým príkladom architektonického rozhodnutia, pri ktorom pri každej malej chybe padajú hlavy.

Jedným z veľkých rozhodnutí poslednej doby je spôsob ukladania primárnych kľúčov v databázových tabuľkách. Aj keď sa to zdá ako triviálny problém, skrýva sa za ním oveľa viac, než si myslíte.

Možnosti primárneho kľúča
-------------------------

V zásade existujú štyri základné možnosti:

- celé číslo
- celé číslo unsigned
- big int
- <a href="/uuid-performance">UUID</a>

Integer je jednoducho celé číslo (v prípade `unsigned` potom unsigned, teda vždy kladné, a v prípade `big int` potom môže dosahovať extrémne veľké hodnoty). Veľmi jednoduchý koncept. UUID je potom textový reťazec (napríklad v tvare `c4a760a8-dbcf-5254-a0d9-6a4474bd1b62`), ktorý sa skladá z niekoľkých častí, z ktorých každá môže mať určité vlastnosti a je užitočná pri vytváraní obrovských multiserverových alebo decentralizovaných aplikácií. okolo UUID existuje veľký ekosystém užitočných technológií, ktoré riešia problémy, o ktorých možno ani neviete, že ich máte alebo budete mať v budúcnosti.

Používajte správne kladivo
-------------------------

Prednedávnom (v zime 2020) mi môj priateľ Paul vysvetľoval koncept použitia vhodného riešenia problému danej veľkosti. Ide o skvelú a dôležitú myšlienku, na ktorú mnohí vývojári radi zabúdajú - vytvárajú tak nesmierne zložité riešenia, hoci to nie je potrebné. V angličtine máme pre tento jav peknú frázu **over-engineering**.

Veľkosť a jedinečnosť UUID
--------------------------

Základná výhoda UUID spočíva v tom, že ak je aplikácia príliš veľká a databázu rozdelíme na mnoho webových serverov, kde je jedna databázová tabuľka taká obrovská, že sa nezmestí na disk jedného počítača, môžeme ju rozdeliť na mnoho fyzických počítačov, z ktorých každý bude poznať svoju časť tabuľky a zvyšok si vyžiada od svojich kolegov. UUID rieši aj zásadný problém vkladania nových riadkov, keď v prípade extrémne veľkých aplikácií potrebujeme paralelne zapisovať riadky na mnohých miestach a nechceme čakať, kým sa uvoľní kapacita zápisu hlavného servera.

Koncept písania na viacerých miestach súčasne využívajú napríklad chatové aplikácie. Keď odošlete správu cez Messenger, odošle sa na najbližší databázový server Facebooku, ktorý správe priradí UUID a časovú pečiatku a zapíše ju do svojej miestnej databázy. Váš priateľ na druhom konci sveta zasa zapisuje správy do svojho miestneho dátového centra a medzitým celá cloudová infraštruktúra zabezpečuje synchronizáciu na celom svete. Znie to super, však? :)

Aby takýto paralelný zápis fungoval, je potrebné vyriešiť problém kolízie záznamov. Ak jednotlivé lokálne databázy používajú jednoduché celé číslo, veľmi skoro sa stane, že dva nezávislé servery zapíšu dva rôzne záznamy pod rovnakým identifikátorom. Keď sa tieto záznamy synchronizujú, nastane kolízia. Zvyčajne neexistuje žiadne riešenie - nemôžete prečíslovať ID, pretože k tomu môžu viesť iné relácie.

UUID to rieši napríklad tak, že každému serveru dá dohodnutý prefix, ktorý vloží na začiatok každého UUID, potom vloží časovú značku a potom samotný identifikátor.

> **Záujímavý fakt:** Pri zápise takého obrovského množstva údajov nás nezaujíma ani tak to, kedy bol ktorý záznam zapísaný, ale v akom poradí bol zapísaný (aby sme napríklad používateľovi neprehodili poradie správ).

Možno sa pýtate, ako riešiť situáciu, keď sa servery nemôžu dohodnúť, ktorý prefix majú použiť. Tento problém vzniká napríklad v decentralizovaných alebo offline aplikáciách. V tomto prípade môže byť identifikátor UUID dokonca generovaný náhodne.

Otázka teda znie, aká je pravdepodobnosť, že pri <a href="https://stackoverflow.com/questions/1155008/how-unique-is-uuid">náhodnom generovaní UUID</a> dôjde ku konfliktu. No, vám sa to pravdepodobne nestane. Existuje približne `2^122` jedinečných UUID (pretože ide o `128-bitové číslo`). V praxi je pravdepodobnosť konfliktu približne `0,00000000006 (6 × 10-11)`. V praxi to znamená, že ak budeme generovať **1 miliardu UUID** každú sekundu počas nasledujúcich 100 rokov, pravdepodobnosť konfliktu bude `50 %`. Je teda pravdepodobnejšie, že ku konfliktu nedôjde, a UUID je definitívnym riešením vašich problémov s databázou.

Je takéto robustné riešenie potrebné?
-------------------------------

Ak neviete, odpoveď je **nie**.

Pri primárnom kľúči nastavenom na `int` s príznakom `unsigned` je možných `4 294 967 295` hodnôt (4 miliardy). Porovnanie veľkostí celých čísel nájdete v <a href="https://dev.mysql.com/doc/refman/8.0/en/integer-types.html">dokumentácii MySql</a>.

V čase, keď do jednej tabuľky uložíte 4 miliardy záznamov, vám pravdepodobne skôr dôjde miesto na disku.

Výkonnosť celých čísel a spájania
----------------------

Celé čísla sú naozaj veľmi rýchle. V MySql sú pre ne k dispozícii natívne optimalizácie. Indexy fungujú správne (navyše sú oveľa menšie), zaberajú len 4 bajty, veľmi rýchlo sa pripájajú a vo väčšine prípadov budú vyhovovať.

Ak máte problém s replikáciou databázy, najlepším riešením by mohlo byť umiestnenie celej databázy do cloudu, napríklad MS Azure, a externé vyhľadávanie. Aj pri ukladaní desiatok miliónov záznamov sa rýchlosť prístupu cez integer na konkrétny riadok pohybuje v rádoch milisekúnd (na dobre nakonfigurovanom serveri pod 3 ms) a s klastrovaným indexom môže byť tento čas dobre udržateľný aj pri veľkom počte požiadaviek.

Ak naozaj potrebujete používať UUID, radšej opustite svet MySql a vyberte sa cestou databázy Postgres, ktorá má na rozdiel od MySql vlastný dátový typ pre <a href="https://www.postgresql.org/docs/9.1/datatype-uuid.html">UUUID</a>. Práca so spojeniami je v prípade UUID a MySql obrovským problémom a pri spojení len 3 tabuliek (pričom každá má len niekoľko desiatok tisíc záznamov) môže spracovanie celého dotazu trvať niekoľko stoviek milisekúnd až niekoľko sekúnd. A to je bohužiaľ problém MySql, ktorý pravdepodobne nemôžete vyriešiť.
