Prečo a ako používať rámce a knižnice
=====================================

> id: '5ea6cdde-f67c-4f3f-8871-37b27ee8e23a'
> slug:
> 	cs: proc-pouzivat-frameworky
> 	sk: preco-a-ako-pouzivat-ramce-a-kniznice
> 
> publicationDate: '2020-02-16 22:13:46'
> mainCategoryId: '3e45c55a-a4cd-4745-b1bb-0332702fefbf'
> sourceContentHash: '0926b7ec4f59904c008388431ff22eb5'

Existuje známy vtip, že programátori začnú používať frameworky až vtedy, keď napíšu vlastný a zistia, že to nikam nevedie. Vtipné na tom je, že je to pravda. Sám som to zažil. Dokonca dvakrát.

Na hlavnej stránke <a href="https://nette.org">Nette</a> sa však píše:

> Skutoční programátori **nepoužívajú frameworky**. Webové aplikácie píšu prostredníctvom príkazového riadku priamo na server. Toto je ich pocta. Nám ostatným Nette výrazne uľahčí a spríjemní prácu.

Úloha rámcov pri vývoji aplikácií
-----------------------------------

Som si istý, že to viete aj vy:

Dostanete nápad na skvelý projekt, a tak začnete písať kód na zelenej lúke priamo v čistom PHP... po niekoľkých hodinách zistíte, že väčšina práce sa stále opakuje a riešite základné systémové problémy. Napríklad ako sa pripojiť k databáze, ako vykresliť formulár, ako overiť údaje, ako odoslať e-mail a mnoho ďalšieho.

Pravdepodobne si napíšete vlastné funkcie, ktoré budete volať na tieto úlohy. Ak píšete niekoľko projektov naraz, začnete tieto funkcie zdieľať medzi projektmi v jednom veľkom neprehľadnom súbore a budete ich neustále upravovať podľa potreby.

A keď sú funkcie v takom štádiu vývoja, že na nich začnete zakaždým stavať nový projekt a rovno ho rozvíjať, potom môžete hovoriť o tom, že ste si napísali vlastný framework alebo aspoň knižnicu.

Čo je a čo robí rámec
-------------------------

Ako sme si už ukázali na praktickom príklade zo života, framework je kolekcia mnohých funkcií (ideálne však tried a objektov), ktoré programátorovi ušetria prácu. Pri vývoji nemusí premýšľať, ako sa pripojiť k databáze, ale vie, že je to už niekde naprogramované a "jednoducho to funguje".

Hotové frameworky sú teda zozbieraným umom stoviek ľudí, ktorí počas dlhého obdobia vyvinuli tisíce aplikácií na odladenie jedného riešenia a jedného ekosystému na riešenie nových projektov.

S rámcami získate aj súbor **najlepších postupov**, čo sú spôsoby riešenia problémov bez toho, aby ste o tom museli premýšľať. Ak narazíte na problém, pravdepodobne ho už niekto vyriešil pred vami a vždy je lepšie nájsť riešenie v dokumentácii, ako ho zložito riešiť sám.

Abstraktné programovanie a princíp zapuzdrenia
---------------------------------------------

Dobre navrhnuté rámce, ako je Nette, vám umožňujú programovať na **veľmi vysokej úrovni abstrakcie**.

Programátor (používateľ frameworku) tak nemusí rozumieť tomu, čo sa deje vo vnútri a ako presne komponenty fungujú, čo šetrí veľa času a úsilia. Môže sa sústrediť na riešenie skutočných problémov svojej aplikácie a veľmi rýchlo dosiahnuť výsledky.

Samotný David Grudl (autor frameworku Nette) raz povedal, že Nette navrhol preto, aby dokázal naprogramovať webovú stránku pre klienta za noc, keď príde neskoro domov z krčmy a ráno musí webovú stránku spustiť. Je to spôsobené najmä tým, že vývojár je úplne vzdialený od technických záležitostí.

Aby to fungovalo a aby vývojár mohol jednoducho používať hotový rámec ako celok, je veľmi dôležité správne používať <a href="/encapsulation">princíp zapuzdrenia</a>.
