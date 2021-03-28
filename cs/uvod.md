Úvod do studia PHP
================================

> id: "66b7fd22-6195-4999-ad8d-b799afdc1876"
> slugCS: uvod
> perex: "Úvodní tutoriál do jazyka PHP, možnosti jazyka, informace o tvorbě webu v PHP."
> publicationDate: "2019-09-29 19:25:06"
> mainCategoryId: f4a34087-1b51-4761-8128-4459dfe83d8a

Toto je online kurz studia jazyka PHP, který vznikl za účelem obecného vzdělání. Je k dispozici zdarma na tomto webu. Texty by neměly být použity v rámci jakéhokoli placeného kurzu bez písemného potvrzení. Použité ukázky z celého webu můžete použít bez dalšího omezení i ve Vaší aplikaci. [Všeobecné obchodní podmínky](https://baraja.cz/vseobecne-obchodni-podminky).

Upgrade z HTML
--------------

„Upgrade“? Zní to spíše jako mediální obrat, a skutečně tomu tak je.

Žádný upgrade se totiž nekoná. PHP zachovává všechny funkce a vlastnosti čistého HTML dokumentu, jen přidává nové možnosti psaní a nasazení. Skvělé, že?

Z vývoje statických HTML stránek již víte, že se kód skládá z tagů, které jsou definovány jako klíčová slova uzavřena ve špičatých závorkách (například `<b>Ahoj!</b>` vyjadřuje tučný text **Ahoj!**).

PHP se do HTML stránky vkládá jako tag `<?php` a `?>`, uvnitř kterého se píše další aplikační logika. Důležité je, že PHP má vlastní syntaxi (pravidla, podle kterých se kód píše) a na rozdíl od jazyka HTML není tolerantní k chybám.

Konkrétní příklady, jak na to a co jednotlivé značky znamenají se dozvíme později. Pro začátek je důležité pochopit obecné principy, jak PHP na serveru funguje a jak se kód zpracovává.

Průběh komunikace uživatele se serverem
---------------------------------------

U obyčejné HTML stránky to funguje zhruba takto:

> Uživatel pošle požadavek (vyžádá si konkrétní HTML stránku), server se podívá na disk a pošle zpět přesně to, co je uloženo. Nic zvláštního se nestane a nic víc od toho nečekejte. Stránky budou jen statické, bez žádné možnosti interakce serveru.

Pokud však přidáme PHP, tak se začnou dít teprve divy:

> Uživatel si opět vyžádá stránku. Server otevře daný soubor na disku, ale vidí, že neobsahuje jen čisté HTML, ale i speciální značky, které označují PHP script. Nejprve je tedy vyhodnotí a až pak odešle to, co PHP vygenerovalo.

Vyhodnocení PHP kódu se ve výchozím nastavení provádí při každém načtení stránky, v budoucnosti se dozvíte, jak kód cachovat (ukládat zkompilovaný pro rychlejší zpracování).

Rozdíl zpracování PHP scriptu oproti C/C++
------------------------------------------

Možná jste se ve škole učili používat jazyk `C` nebo `C++`. PHP ze syntaxe jazyka `C` přímo vychází a uvnitř jádra se jazyk `C` používá, proto je dobré znát určité společné rysy a naopak odlišnosti.

Základní princip běžných zkompilovaných programů (které běží lokálně přímo ve vašem počítači nebo smartphone) je načtení aplikační logiky do operační paměti, která přímo komunikuje s operačním systémem, který získává vstupy od uživatele a následně mu zobrazuje výstupy programu. Důležité je, že program běží od spuštění až po jeho ukončení celou dobu izolovaně.

PHP se spouští s každým požadavkem na vykreslení stránky, pokaždé načte veškerý kód a data znovu a poté se ukončí. PHP scripty proto mají doslova jepičí život a běžně existují jen desítky milisekund.

Výhoda tohoto přístupu je vyšší míra izolace - pokud se cokoli pokazí, s dalším načtení stránky se všechno načte znovu. Naopak tento přístup má vyšší nároky na výkon, protože se musíme například opakovaně připojovat k databázi, číst soubory z disku a podobně.

V budoucnosti se dozvíte, že lze PHP scripty držet načtené v operační paměti pomocí rozšíření `OP Cache`, které má většina nových serverů (od PHP 7.1) v základní konfiguraci nastaveno.

Stažení cizích PHP scriptů
--------------------------

Relativně často se studenty řešíme otázku, jak stáhnout cizí PHP scripty ze serveru a podívat se na jejich zdrojový kód. Tomuto dotazu předchází úvaha, že HTML kód stránky lze ve webovém prohlížeči jednoduše zobrazit.

Odpověď je, že **PHP scripty stáhnout nelze**. Důvod je ten, že se PHP kód nejprve vyhodnocuje na webovém serveru a následně se do prohlížeče uživatele posílá vygenerovaný HTML kód (nebo i jiný výstup). Proto lze stáhnout jen výstup z PHP scriptu, nikoli samotný script.

Jak funguje generování do HTML?
---------------------------------

Nefunguje to tak doslova, že budete surfovat po HTML stránkách. PHP script se musí vždy nacházet v PHP souboru.

Do teď, byla většina zvás zvyklá vytvářet obrovské složky, plné souborů, končící na příponu **.html**. Nyní to bude mnohem méně souborů. V extrémním případě se může jednat o jediný soubor.
