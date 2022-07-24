Ako efektívne optimalizovať rýchlosť načítania stránky
======================================================

> id: '72f1d564-cb70-431c-a3dd-a3fdcaf14292'
> slug:
> 	- null
> 	sk: ako-efektivne-optimalizovat-rychlost-nacitania-stranky
> 
> cs: optimalizace-rychlosti-nacitani
> publicationDate: '2021-10-15 15:00:00'
> mainCategoryId: '483db7b7-5699-41fb-ba0b-d2b653bacd1f'
> sourceContentHash: '013c8f29c213d3f3f1df3fbe4be8d572'

Keď cestujem, často sa stretávam so zlým pripojením na internet, čo ma ako webového dizajnéra núti premýšľať o princípoch návrhu, ako vyriešiť rýchlosť webu aj pri slabom pripojení.

Z praxe som si osvojil niekoľko užitočných trikov:

Dôležité vstupné stránky sú často jednoduché a oplatí sa používať vlastné štýly CSS
-----------------------------------------------------------------------------------

Napríklad prihlasovacia stránka do systému CMS je často veľmi jednoduchá. Je naozaj potrebné pre jednoduchý formulár sťahovať celý Bootstrap a ďalšie rámce CSS/JS? Dôležité stránky sa oplatí optimalizovať a napísať natívny kód.

Po úplnom vykreslení stránky máme niekoľko sekúnd na to, aby používateľ vyplnil svoje údaje, a my môžeme v prehliadači na pozadí stiahnuť a uložiť do vyrovnávacej pamäte zvyšné štýly. V čase, keď sa používateľ prihlási, už bude mať pravdepodobne stiahnutý Bootstrap (a ak používa Edge, tak nie...).

Namiesto ikon môžeme často používať emoji
-----------------------------------

Naozaj! Emoji má veľa praktických výhod:

- Zaberá len 4 bajty, takže prekoná akýkoľvek obrázok
- Nikdy sa nestiahne, takže používateľ vždy vidí aspoň ikonu, a nie prázdne miesto.
- Zobrazuje sa vo vizuálnom štýle používateľa
- V súčasnosti už má širokú podporu zariadení
- Ušetrí požiadavku na server a nemusíme sa zaoberať tým, odkiaľ získať obrázok (to môže byť optimalizované prostredníctvom CDN, ale prečo, že)
- Mnohými ikonami sa pravdepodobne nechcete zaoberať. Kde napríklad získať ikony vlajok všetkých krajín, ktoré chcete zobraziť v administrácii? Použitie emoji: https://github.com/bara.../country/blob/main/flag-emoji.json

Používanie kritických štýlov priamo v HTML pri načítaní stránky
------------------------------------------------------

Niekedy vkladám dôležité štýly CSS definujúce rozloženie stránky a základný layout priamo do HTML. Dávam limit 1 KB, do ktorého sa snažím dostať čo najviac. Nevýhodou tohto prístupu je, že štýly sa musia prenášať pri každej požiadavke a nemožno ich ukladať do vyrovnávacej pamäte, na druhej strane sa však sťahujú neporovnateľne rýchlejšie ako obrázok.

Nie som až taký odborník na rýchlosť načítania, [Martin Michalek](https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/) by vám to povedal lepšie.

Použite ajax!
--------------

Na načítanie nedôležitého alebo pomalého obsahu vždy používam ajax. Trochu to zvyšuje technologické požiadavky aplikácie, ale môžem si to dovoliť.

Príkladom vhodného miesta na použitie ajaxu je takmer všetko v administrácii. Často je potrebné získať veľa údajov, ale používateľ nemá záujem o všetko. Ak má používateľ stiahnutý len tenký javascriptový klient a všetky údaje sa načítavajú prostredníctvom Vue a json, sťahuje sa vždy len minimálne množstvo údajov a odpovede sú okamžité.

Ako to urobiť vo Vue: https://vue.baraja.cz/api-a-ajax-ve-vue-js

Na zadnej strane používam knižnicu pre Nette: https://github.com/baraja-core/structured-api

Používanie vhodnej siete CDN
---------------------

Na distribúciu statického obsahu odporúčam používať CDN pre všetky typy lokalít. Ak neviete, ako nastaviť CDN, použite aspoň Cloudflare v režime proxy. Na základe hlavičiek HTTP automaticky ukladá statický obsah do vyrovnávacej pamäte. Nie každý poskytovateľ hostingu má dobre nastavený Pops a pri dlhých trasách vám to ušetrí stovky milisekúnd pri každej požiadavke.

Vhodný formát obrázku
---------------------

Jeden z mojich juniorov nedávno umiestnil na hlavnú stránku webu obrázok PNG, ktorý obsahoval fotografiu a zaberal 3 MB. Povedal, že je to v poriadku, pretože stránka sa na jeho pripojení načítala rýchlo (na jeho optike doma sa načítava, že áno...), a navyše povedal, že v dnešnej dobe prenášame veľa dát, napríklad video.

Som v tomto smere staromódny a optimalizujem, čo sa dá.

Fotografie do formátu JPG alebo lepšie WEBP. Uloženie obrázka do formátu JPG však nič neznamená, musíte ho spustiť prostredníctvom optimalizačnej služby: https://tinyjpg.com

Ak máte obrázky v systéme Git, tento nástroj ich dokáže automaticky komprimovať a odoslať žiadosť o stiahnutie: https://imgbot.net. Po pridaní nového obrázka sa opätovne odošle PR.

Ak potrebujete komprimovať tisíce obrázkov (napríklad celú galériu produktov na stránke), používam na to desktopovú aplikáciu pre Mac: https://imageoptim.com/mac.

Vhodnou kompresiou obrázkov sa zvyčajne ušetrí najviac údajov. Niekedy aj 50 %.
