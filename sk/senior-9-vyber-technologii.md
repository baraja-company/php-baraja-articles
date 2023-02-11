Ako vybrať technológie? Kedy prejdeme na JavaScript?
====================================================

> id: d3ea7ea7-3e2f-455d-a424-cce374bcd1d2
> slug:
> 	cs: senior-9-vyber-technologii
> 	sk: ako-vybrat-technologie-kedy-prejdeme-na-javascript
> 
> publicationDate: '2023-02-11 14:40:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '72807451867478e1a7b6d67f4e5c31b3'

Výber správnych technológií je základným predpokladom na to, aby ste sa mohli stať vedúcim vývojárom. Tieto rozhodnutia často nie sú jednoduché, pretože musíte zohľadniť súčasný technický stav aplikácie, kam smerujete vo vývoji, aké sú súčasné znalosti vášho tímu, aké znalosti sú bežné na trhu práce, aké sú náklady na jednotlivé technológie, aké riziká prinesú do vašej prevádzky, ako je technológia bezpečná a stabilná a v neposlednom rade, o čo budú mať vývojári záujem napríklad o 5 rokov, keď sa vymení 80 % vášho súčasného tímu.

Prešiel som 6 veľkými spoločnosťami, ktoré vyvíjajú v PHP. Len dvaja z nich sa dlhodobo snažia prejsť na inú technológiu, ostatní zostávajú. To má veľa problémov. V súčasnosti sa napríklad snažím nájsť seniorného vývojára PHP pre firemný projekt, ktorý vyvíjam pre spoločnosť O2, s požiadavkou dochádzať do kancelárií v Prahe, a vidím, ako sa trh s vývojármi PHP za posledných 5 rokov vyčistil. PHP už jednoducho nie je cool a málokto sa mu chce venovať. Nie je v nej dostatok juniorov.

Z rozhovorov s mladšími ľuďmi vnímam, že React a všeobecne "tenké" technológie sú v súčasnosti veľmi populárne. Z hľadiska aplikačnej architektúry má zmysel, ak tento smer objavíte včas a máte čas na prispôsobenie. Namiesto zložitého búchania webových layoutov a formulárov v Latte, na ktoré potrebujete prakticky priemerného vývojára na už o niečo zložitejšie zadanie, v Reacte stačí junior, ktorý začal v podstate pred mesiacom, a napriek tomu v budúcom riešení nerobí príliš veľa chýb.

React vám umožňuje zahodiť veľkú časť backendu, ktorý bol napísaný len preto, aby mohol frontend vôbec existovať. Stručne povedané, vývoj je vďaka tomu lacnejší a ako bonus získate rýchlejšie dodanie nových funkcií, pretože vývojári sa nemusia znova a znova zaoberať zložitými problémami, ktoré vyplývajú z návrhového jazyka PHP.

Väčšina webových aplikácií už ani nepotrebuje backend, alebo len minimálny backend. Keď vystavíte koncové body API v Node.js (tiež technológia postavená na javascripte), zrazu môže vývojár, ktorý predtým robil len React, písať aj časti backendu, pretože ide o rovnaký jazyk.

Pri hlbšej analýze projektov, ktoré som vyvinul za posledných 5 rokov, mi v Node.js chýba len niekoľko vecí, ktoré ma stále nútia používať PHP na niektoré operácie.

Konkrétne:

- Doctrine (a všeobecne relačný prístup k databáze založený na objektových entitách)
- Odosielanie a správa e-mailov
- Rozhranie SOAP API (bohužiaľ, občas sa ešte používa)
- relácie (musíte ho nahradiť napríklad tokenom JWT)
- Komplexná staršia logika, ktorá bola napísaná v PHP a nemôžem ju jednoducho refaktorovať
- Rýchle spracovanie zložitých dátových štruktúr, pri ktorých je potrebné údaje mutovať
- Existujúci ľudia v tíme, ktorých musíte preškoliť, aby robili niečo nové

Potom však prišiel Node.js, ktorý ostatné veci zvláda lepšie. Napríklad:

- Možnosť preniesť aplikáciu priamo do cloudu
- Oveľa (možno aj dvakrát) lacnejší vývoj rovnakej funkcie
- Rovnaká logika na BE a FE bez nutnosti písať kód dvakrát
- Koncové body REST API
- Paralelné volania viacerých kódov naraz
- Možnosť odoslať odpoveď HTTP, ale kód sa naďalej spúšťa
- Crony
- Knižnice na prácu s cloudovými službami
- Výrazne lepší čas odozvy, pretože nemusíte spúšťať obrovskú aplikáciu
- Plne funkčná paradigma (zbavíte sa napríklad DI, ktoré v JS nepotrebujete)
- Práca s formulármi a údajmi
- Jednoduché aktualizácie a aktívna komunita vývojárov

**Komentár k Doctrine:** Viem, že JS prináša veľa knižníc na prácu s databázami. Alebo aj nové paradigmy, ako je Mongo. Páči sa mi, kam smeruje spracovanie údajov. Na druhej strane si myslím, že tabuľkové relačné databázy nikdy nezmiznú. Keď robíte naozaj veľký projekt, v ktorom spravujete desiatky miliónov záznamov, potrebujete tradičnú technológiu, ktorú veľmi dobre poznáte a viete, čo môžete očakávať. Napríklad predstava, že chcem pridať stĺpec (vlastnosť) a znamenalo by to premapovať všetky entity pomocou migračného skriptu, ma dosť desí.
