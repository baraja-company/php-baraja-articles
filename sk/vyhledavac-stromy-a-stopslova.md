Algoritmus internetového vyhľadávača - Trees a StopLead
=======================================================

> id: '11ec73f2-e505-4338-89aa-8307a55b7ba0'
> slug:
> 	cs: vyhledavac-stromy-a-stopslova
> 	sk: algoritmus-internetoveho-vyhladavaca---trees-a-stoplead
> 
> publicationDate: '2016-09-11 10:00:00'
> mainCategoryId: '367f936c-073f-44bd-b399-30738e93137a'
> sourceContentHash: d011d2d6943271ca5e45c3d3cdf03d61

Každú sekundu pribudne na internete 5 miliónov nových stránok a táto rýchlosť sa neustále zvyšuje. Aby sme v tomto obrovskom mori informácií urobili poriadok a niečo v ňom našli, existujú vyhľadávače. Cieľom nasledujúcej práce je priblížiť problematiku vyhľadávania a vysvetliť celý proces od vytvorenia novej stránky až po jej vyhľadanie vo vyhľadávači.

Úloha nájsť a roztriediť súbor miliárd dokumentov nie je jednoduchá. Len spoločnosť Google potrebuje 300 000 webových serverov, aby túto úlohu zvládla za niekoľko hodín. Vyhľadávanie vašej otázky sa v skutočnosti uskutočňuje dlho predtým, ako ju položíte. Google už má v pamäti uložené výsledky vyhľadávania, ktoré budete v najbližších dňoch požadovať.

Architektúra vyhľadávača
------------------------

<img src="{$baseUrl}/images/fulltext-schema.png" alt="Spoločná architektúra vyhľadávača až pre 50 miliónov dokumentov" class="w-100 mb-3">

Správne navrhnutý vyhľadávač obsahuje mnoho komponentov, ktorých je rádovo niekoľko stoviek. Tento diagram opisuje najzákladnejšie prvky, ktoré musí vyhľadávač minimálne obsahovať, aby mohol fungovať. Ide o starú a už nepoužívanú architektúru, ktorá fungovala približne do roku 2005, keď bolo na webe len niekoľko miliónov dokumentov. Táto architektúra neumožňuje "nekonečné" množstvo obsahu a tiež možné výpadky vyhľadávacích serverov (základné vyhľadávanie). V prípade výpadku jednej zložky by prestal fungovať celý systém a vyhľadávač by bol nedostupný.

Úplný opis súčasných trendov v navrhovaní nových architektúr presahuje rámec tohto článku, pretože ide zvyčajne o obchodné tajomstvo veľkých spoločností. Cieľom tohto článku je vysvetliť, ako táto základná architektúra funguje. Jednotlivé komponenty sú podrobne vysvetlené v nasledujúcich kapitolách.

Vstupný dotaz
------------

Najviditeľnejšou zložkou aj pre bežných používateľov je vstupný dotaz, ktorý je v súčasnosti najčastejšie reprezentovaný ako textové pole. Vstupné pole sa nachádza na špeciálnej stránke, ktorá je zvyčajne určená len na zadávanie údajov.

V ďalšej fáze používateľ zadá vyhľadávací reťazec a odošle formulár na servery vyhľadávača, čím sa začne komunikácia s výstupnou zložkou, niekedy nazývanou aj hlavné vyhľadávanie. Výdajné servery sú konštruované tak, aby zvládli obrovské zaťaženie od používateľov a musia byť schopné spracovať všetky vyhľadávania naraz.

Architektúra dispečerského servera je zvyčajne jednoduchý server Apache so základnou inštaláciou a vynikajúcou šírkou pásma siete. Keď dotaz vstúpi na server, spracuje sa prostredníctvom regresných rozhodovacích stromov a vytvorí sa "mapa dotazu", ktorá zachytáva kompletnú sémantiku ostatných významov, aby bolo jasné, čo používateľ vlastne hľadá. Metódy prepisovania dopytov sú príliš ďaleko nad rámec tohto článku, preto uvedieme len všeobecné popisy toho, ako môže a nemôže prepisovanie vyzerať.

Uvažujme nasledujúci dotaz:

```
"O psovi a mačke"
```

Tento dotaz sa prevedie na binárny strom, ktorý zachytáva jeho sémantickú podstatu:

<img src="{$baseUrl}/images/fulltext-tree.png" alt="Symbolický stromový diagram" class="w-100 mb-3">

Zmyslom stromu rozboru je zachytiť, ako sa vyhľadávač pozerá na výsledky vyhľadávania. Tento strom sa spolu s dopytom odošle na pomocné vyhľadávacie servery (v tomto dokumente označené ako Base search), kde sa vyhľadávajú jednotlivé kľúčové slová (indexové čítanie) a následne sa triedia podľa pravidiel (podľa operácií).

| Operátor | Operácie triedenia
|----------|------------------
| AND | Intersection |
| OR | Sum |
| NOT | Doplnok |

Neexistuje žiadne skutočné triedenie výsledkov vyhľadávania, táto zložka len vykonáva rýchle binárne operácie s obrovským množstvom údajov (často milióny záznamov) a v podstate len porovnáva ID jednotlivých dokumentov.

Ako funguje samotné triedenie výsledkov vyhľadávania na stránke s výsledkami, sa dozviete v nasledujúcich častiach. Výstupný server sa jednoducho používa na kombinovanie veľkého množstva údajov z rôznych vyhľadávacích serverov s cieľom zvýšiť celkový výkon a rozložiť záťaž a potom zistené hodnoty vloží do stránky s výsledkami (nazýva sa "webová stránka"), ktorá obsahuje zložené informácie z desiatok serverov.

Prepis do binárneho stromu
-----------------------

Ako bolo spomenuté v predchádzajúcej kapitole, celé vyhľadávanie je postavené na binárnych stromoch, ktoré hovoria, ako budú vyzerať výsledky a čo treba hľadať. Celá "logika" a "inteligencia" vyhľadávača priamo závisí od kvality programu, ktorý vytvára strom - konkrétne deskriptor.

Správne zostavený strom by mal obsahovať všetky potrebné metainformácie o dotaze v takej podobe, aby sa dal ľahko spracovať aj pomocou sekvenčného čítania z disku a mal by eliminovať náhodné prístupy do pamäte, preto zvyčajne obsahuje jednotlivé hľadané slová (od používateľa), ich prepisy (a pravopis) a v neposlednom rade aj väzby medzi slovami, t. j. ich relatívne vzdialenosti v texte.

Metódy prepisu dopytu
---------------------

Najzákladnejším spôsobom prepisu dotazu je jeho rozbor na jednotlivé slová (podľa medzier), s ktorými sa bude ďalej pracovať. Druhým krokom je vyhľadanie StopSlov a ich nahradenie ukazovateľom premennej (pretože, ako si ukážeme neskôr, nemá zmysel StopSlová vôbec vyhľadávať).

Majme nasledujúci dotaz: "O psovi a mačke", jeho prepis v najzákladnejšom stave vyzerá takto:

```
"O (a) psovi (a) a (a) mačke"
```

Druhým krokom je odstránenie slov, ktoré nemajú žiadnu relevanciu pre vyhľadávanie. Samozrejme, napríklad slovo "o" alebo "a" má pre nás ľudí význam, ale nie pre vyhľadávanie v databáze, pretože ho možno nahradiť iným slovom a výsledky sa vypíšu. Cieľom by nemalo byť nájsť doslovnú zhodu dotazu s indexom, pretože to by viedlo k zlým výsledkom, pretože slová v texte na internete sa často píšu v rôznych tvaroch a táto metóda sa snaží nájsť výsledky v iných tvaroch, ako sme dostali od používateľa. Ide teda o prvý náznak "inteligentného" vyhľadávača.

Tieto bezvýznamné slová sa často nazývajú "stopwords", každý vyhľadávač by mal viesť index takýchto slov a tento index by mal byť často aktualizovaný (ručne).

```
["pes (a) * (a) mačka"]
```

V tomto okamihu hľadáme 2 rôzne slová ("psovi" a "mačke"), medzi ktorými bolo ďalšie slovo (interne ho označíme hviezdičkou).

Cieľom tejto úpravy nie je zvýšiť kvalitu vyhľadávania, ale zvýšiť výkon. Je to preto, že keď hľadáme slovo, ktoré je príliš frekventované, dochádza k rýchlemu načítaniu a táto úprava pomáha procesu - najmä tým, že nehľadáme slovo, ktoré sa aj tak bude nachádzať na tejto pozícii vo väčšine dokumentov.

Dôležité je tiež poznamenať, že túto úpravu (vynechanie slova) nemôžeme vždy vykonať, preto by si každý vyhľadávač mal viesť samostatný index fráz, ktoré sa vyskytujú na internete, aby si overil, ktorá úprava vedie k dobrým výsledkom a ktorá nie. Občas však vyhľadávač vykoná túto úpravu, aj keď danú frázu nemá vo svojom indexe - najmä ak používateľ hľadá významné slovo, pri ktorom sa očakáva, že na prvých pozíciách budú významné stránky, ktoré túto "chybu" prekonajú, pretože používatelia ich zvyčajne aj tak požadujú.

Posledným krokom je práca s anglickým jazykom a "vyčistenie" dotazu od nepotrebných znakov. Napríklad pri dotaze "práčka" používateľ zvyčajne očakáva výsledky len na slovo "práčka" a znak čiarky môžeme ignorovať.

Presné metódy fungovania tohto systému nie sú známe a každý vyhľadávač má svoje vlastné. Predpokladá sa, že ide väčšinou len o štatistický model, ktorý tieto úpravy vykonáva na základe poznatkov o miliardách textov v databáze.

Anglická transkripcia je tiež veda sama o sebe, preto aj tu bude uvedený len základný popis. Vo väčšine prípadov stačí ku každému slovu pridať jeho skloňované podoby (podľa slovníka) a vyhľadávať ich spolu so slovom, čím sa dosiahne efekt, že vyhľadávač nájde dokumenty, v ktorých sa slová nevyskytujú len v základnej podobe (ako ich zadal používateľ), ale dokáže nájsť aj skloňované verzie - veľmi užitočná funkcia. Problémom tohto prístupu je skloňovanie nejasných a problematických slov a tiež skloňovanie otázok, ktoré sú krátke (a preto nie je možné z kontextu určiť, ako sa má slovo skloňovať). Príkladom problematického skloňovania nech je slovo "hry".

Pri anglickom dopyte "I love her" bude zle navrhnutý skloňovač skloňovať slovo "games" napríklad ako "games, gaming, gaming room, ...", preto je potrebné indexovať aj okolité slová ako frázy a skloňovať len v známych situáciách alebo použiť iný postup (tu často nastupujú fonetické algoritmy).

Posledným krokom je odoslanie hotového stromu na vyhľadávacie servery Base, kde sa uskutoční skutočné vyhľadávanie v sudoch - to je predmetom nasledujúcej kapitoly.
