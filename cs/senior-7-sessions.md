Jak řešit náhlé zaseknutí PHP scriptu
======================================

> id: "41edbf1b-3ca5-487f-a54c-fac9926bc3d2"
> slug:
> 	cs: senior-7-sessions
>
> publicationDate: "2023-02-11 14:30:00"
> mainCategoryId: 8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a

Příběh z kraje roku 2016, kdy mě doslova zachránil kolega: V PHP aplikaci se rozhodnete odbavovat obrázky přes proxy script, který mimo jiné umí upravovat jejich rozměry a další parametry podle příchozího requestu. V rámci optimalizace také vygenerované varianty uložíte fyzicky na disk.

Při produkčním provozu ale najednou začnete sledovat obří zátěž a tisíce requestů stojících ve frontě. Obrázky se každému uživateli načítají sekvenčně jeden po druhém. Nefunguje obnovení stránky ani klikání na odkazy. Aplikace vypadá kompletně zamrzlá. Funguje jen počkat, než se vše zpracuje.

V čem může být problém? V textu jsem uvedl 3 zásadní indicie pro možnost rychlého pátrání po problému. Hotfix má triviální řešení.
