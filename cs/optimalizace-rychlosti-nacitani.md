Jak efektivně optimalizovat rychlost načítání stránek
=====================================================

> id: "72f1d564-cb70-431c-a3dd-a3fdcaf14292"
> slug:
>   cs: optimalizace-rychlosti-nacitani
>
> publicationDate: "2021-10-15 15:00:00"
> mainCategoryId: "483db7b7-5699-41fb-ba0b-d2b653bacd1f"

Při cestování často narážím na špatné připojení k internetu, což mě jako webdesignera nutí přemýšlet o návrhových principech, jak rychlost webu i na špatném připojení vyřešit.

Z praxe jsem přišel na pár užitečných triků:

Důležité vstupní stránky jsou často jednoduché a vyplatí se použít vlastní CSS styl
-----------------------------------------------------------------------------------

Třeba login stránka do CMS bývá často velmi jednoduchá. Potřebuje jednoduchý formulář skutečně stahovat celý Bootstrap a další CSS/JS frameworky? Důležité stránky se vyplatí optimalizovat a napsat nativní kód.

Až se stránku povede plně vykreslit, získáme pár sekund, kdy uživatel vyplňuje svoje údaje, a na pozadí můžeme zbývající styly stáhnout a uložit do cache prohlížeče. Až se uživatel přihlásí, bude mít Bootstrap již pravděpodobně stažený (a když je na edge, tak ne...).

Místo ikonek lze často použít emoji
-----------------------------------

Opravdu! Emoji má spoustu praktických výhod:

- Zabírá jen 4 bajty, takže trumfne každý obrázek
- Nikdy se nestane, že by se nestihlo stáhnout, takže vždy uživatel vidí aspoň nějakou ikonku, než aby tam bylo prázdné místo
- Zobrazí se v uživatelském vizuálním stylu
- V současné době již má širokou podporu zařízení
- Ušetříme požadavek na server a nemusíme řešit, odkud obrázek získat (to lze sice optimalizovat přes CDN, ale proč, že jo)
- Hodně ikonek se vám pravděpodobně nechce řešit. Kde třeba sehnat ikonky vlajek všech států, které chcete zobrazit v administraci? Použijte emoji: https://github.com/bara.../country/blob/main/flag-emoji.json

Při načítání webu použijte kritické styly přímo v HTML
------------------------------------------------------

Důležité CSS styly definující layout stránky a základní rozložení dávám někdy i přímo do HTML. Dal jsem si limit 1 KB, do kterého se snažím dostat co nejvíc. Nevýhoda tohoto přístupu je, že se musí styly přenášet v každém requestu a nelze je cachovat, na druhou stranu se stáhnou nesrovnatelně rychleji, než obrázek.

Na rychlost načítání nejsem až takový expert, to by vám lépe pověděl [Martin Michálek](https://www.programia.cz/rozhovor-martin-michalek-rychlost-webu/).

Používej ajax!
--------------

Na dotahování nedůležitého nebo pomalého obsahu používám vždy ajax. Sice to trochu zvyšuje nároky na technologické zázemí aplikace, ale já si to můžu dovolit.

Příklad vhodného místa pro použití ajaxu je třeba skoro všechno v administraci. Velmi často je potřeba získat spoustu dat, ale uživatele nezajímá vše. Když je u uživatele stažený jen tenký javascriptový klient a všechna data se dotahují přes Vue a json, vždy se stahuje jen minimum dat a odpovědi jsou instantní.

Jak to udělat ve Vue: https://vue.baraja.cz/api-a-ajax-ve-vue-js

Na backendu používám knihovnu pro Nette: https://github.com/baraja-core/structured-api

Používejte vhodné CDN
---------------------

Pro distribuci statického obsahu doporučuji používat CDN pro všechny typy webů. Pokud si neumíte CDN nastavit, použijte aspoň Cloudflare v režimu proxy. Statický obsah bude ukládat automaticky k sobě do cache na základě HTTP hlaviček. Ne každý poskytovatel hostingu má dobře nastavené Popy a při dlouhých trasách tím ušetříte i stovky milisekund v každém requestu.

Vhodný formát obrázků
---------------------

Jeden z mých juniorů nedávno na hlavní stranu webu vložil PNG obrázek, ve kterém byla fotka a zabíral 3 MB. Podle něj to bylo v pořádku, protože se na jeho připojení stránka načte rychle (doma na optice jo, že jo...), navíc prý dneska přenášíme spoustu dat, třeba videa.

V tomto jsem staromódní a optimalizuji co se dá.

Fotky do JPG, nebo lépe WEBP. Uložit obrázek do JPG ale ještě nic neznamená, je potřeba prohnat optimalizační službou: https://tinyjpg.com

Pokud máte obrázky v Gitu, tento nástroj je umí automaticky zkomprimovat a poslat pull request: https://imgbot.net. Když se přidá nový obrázek, pošle PR znovu.

Pokud potřebujete zkomprimovat tisíce obrázků (třeba celou galerii produktů na webu), používám k tomu desktopovou aplikaci pro Mac: https://imageoptim.com/mac

Vhodnou komprimací obrázků ušetříte obvykle nejvíc dat. Někdy i 50 %.
