Úvod do jazyka PHP
==================

> id: '66b7fd22-6195-4999-ad8d-b799afdc1876'
> slug:
> 	cs: uvod
> 	sk: uvod-do-jazyka-php
> 
> perex: 'Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP.'
> publicationDate: '2019-09-29 19:25:06'
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a
> sourceContentHash: db6ba8fe6d5ca28aab62d7302c1d7527

Toto je online kurz na výučbu jazyka PHP určený na všeobecné vzdelávanie. Je k dispozícii zadarmo na tejto webovej stránke. Texty by sa nemali používať v žiadnom platenom kurze bez písomného potvrdenia. Vzorky použité na tejto stránke môžete vo svojej aplikácii používať bez ďalších obmedzení. [Podmienky](https://baraja.cz/vseobecne-obchodni-podminky).

Aktualizácia z HTML
--------------

"Upgrade"? Znie to skôr ako mediálny výmysel, a skutočne to tak je.

Neexistuje žiadna aktualizácia. PHP zachováva všetky funkcie a vlastnosti čistého dokumentu HTML, len pridáva nové možnosti zápisu a nasadenia. Skvelé, však?

Z tvorby statických stránok HTML už viete, že kód sa skladá zo značiek, ktoré sú definované ako kľúčové slová uzavreté v špicatých zátvorkách (napríklad `<b>Hello!</b>` vyjadruje tučný text **Hello!**).

PHP sa vkladá do stránky HTML ako značky `<?php` a `?>`, v ktorých je zapísaná ďalšia aplikačná logika. Dôležité je, že PHP má vlastnú syntax (pravidlá, podľa ktorých sa kód píše) a na rozdiel od HTML nie je tolerantný voči chybám.

Konkrétne príklady, ako to urobiť a čo jednotlivé značky znamenajú, uvedieme neskôr. Na začiatok je dôležité pochopiť všeobecné princípy fungovania jazyka PHP na serveri a spôsob spracovania kódu.

Tok komunikácie medzi používateľom a serverom
---------------------------------------

Pri bežnej stránke HTML to funguje približne takto:

> Používateľ odošle požiadavku (požiada o konkrétnu stránku HTML), server sa pozrie na disk a pošle späť presne to, čo je uložené. Nič zvláštne sa nedeje a nič viac ani neočakávajte. Stránky budú len statické, bez možnosti interakcie so serverom.

Ak však pridáme PHP, začnú sa diať zázraky:

> Používateľ si stránku vyžiada znova. Server otvorí súbor na disku, ale zistí, že neobsahuje len čisté HTML, ale aj špeciálne značky, ktoré označujú skript PHP. Najprv ich teda vyhodnotí a potom odošle to, čo PHP vygenerovalo.

Vyhodnocovanie kódu PHP sa štandardne vykonáva pri každom načítaní stránky, v budúcnosti sa naučíte, ako kód ukladať do vyrovnávacej pamäte (uložiť ho skompilovaný pre rýchlejšie spracovanie).

Rozdiel medzi spracovaním skriptov PHP a C/C++
------------------------------------------

Možno ste sa v škole učili používať jazyk `C` alebo `C++`. PHP je priamo založené na syntaxi jazyka `C` a v jadre sa používa jazyk `C`, takže je dobré poznať niektoré spoločné črty a naopak rozdiely.

Základným princípom bežných kompilovaných programov (ktoré sa spúšťajú lokálne priamo v počítači alebo smartfóne) je načítanie aplikačnej logiky do operačnej pamäte, ktorá komunikuje priamo s operačným systémom, ktorý prijíma vstupy od používateľa a následne zobrazuje výstupy programu používateľovi. Dôležité je, že program beží izolovane po celú dobu od spustenia až po ukončenie.

PHP začne s každou požiadavkou na vykreslenie stránky, zakaždým znovu načíta všetok kód a údaje a potom skončí. Skripty PHP majú preto doslova yuppie životnosť a zvyčajne existujú len desiatky milisekúnd.

Výhodou tohto prístupu je vyššia úroveň izolácie - ak sa niečo pokazí, všetko sa znovu načíta pri ďalšom načítaní stránky. Na druhej strane má tento prístup vyššie nároky na výkon, pretože sa musíme napríklad opakovane pripájať k databáze, čítať súbory z disku a podobne.

V budúcnosti sa dozviete, že skripty PHP môžete udržiavať načítané v operačnej pamäti pomocou rozšírenia `OP Cache`, ktoré má väčšina nových serverov (od PHP 7.1) nastavené v základnej konfigurácii.

Preberanie zahraničných skriptov PHP
--------------------------

Pomerne častou otázkou, ktorú so študentmi diskutujeme, je, ako stiahnuť cudzie skripty PHP zo servera a pozrieť si ich zdrojový kód. Tejto otázke predchádza úvaha, že kód HTML stránky možno ľahko zobraziť vo webovom prehliadači.

Odpoveďou je, že **Skripty PHP nemožno stiahnuť**. Je to preto, že kód PHP sa najprv vyhodnotí na webovom serveri a potom sa vygenerovaný kód HTML (alebo iný výstup) odošle do prehliadača používateľa. Preto je možné stiahnuť iba výstup skriptu PHP, nie samotný skript.

Ako funguje generovanie do HTML?
---------------------------------

Nefunguje to doslova surfovaním po stránkach HTML. Skript PHP musí byť vždy v súbore PHP.

Väčšina z vás bola doteraz zvyknutá vytvárať obrovské priečinky plné súborov s príponou **.html**. Teraz to bude oveľa menej súborov. V extrémnom prípade môže ísť o jeden súbor.
