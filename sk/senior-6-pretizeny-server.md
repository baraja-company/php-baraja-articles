Naliehavá oprava preťaženého servera
====================================

> id: '07c5d1ed-f854-4797-8efe-350a24594762'
> slug:
> 	cs: senior-6-pretizeny-server
> 	sk: naliehava-oprava-pretazeneho-servera
> 
> publicationDate: '2023-02-11 14:25:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: f8239dd6b0eee234efc999c0dd3e0aeb

Externý monitorovací nástroj vám oznámi, že priemerný čas odozvy 5 monitorovaných adries URL sa za posledných 30 minút zdvojnásobil. Projekt beží na jednom fyzickom serveri, ktorý nie je pod vašou správou a je spustený niekde v dátovom centre. Pripojíte sa cez SSH, spustíte htop a zistíte, že CPU je zaťažený na 95 % a pamäť je už dávno preplnená.

Podľa gitu viete, že asi pred týždňom robili migráciu databázy na novú štruktúru tabuliek a kolega píše v chate, že musel migráciu spustiť cez noc, pretože prepočítavanie stĺpcov a indexov trvalo asi 5 hodín, počas ktorých bola takmer celá databáza zamknutá a nefungoval INSERT ani SELECT.

Problémy s výkonom sú teda pravdepodobne spôsobené nesprávne navrhnutými indexmi, zle prepracovanými dotazmi SQL alebo veľkým združovaním spojení. Na revert nie je čas, na stránke je podľa Google Analytics 7 tisíc používateľov a výpadok na 5 hodín by pre klienta znamenal reputačné riziko a stratu desiatok až stoviek tisíc korún za ten čas (ťažko odhadnúť, projektanti si vymýšľajú dosť). Uvedomujete si, že testovanie iba funkčnosti v testovacom prostredí nestačí a je potrebné vykonať aj záťažový test.

Keďže ide o dôležitý e-shop vášho najväčšieho klienta a vy očakávate, že sa situácia môže zhoršiť, máte 30 sekúnd na rozhodnutie.

Ako postupujete?

1. Nerobíte nič. Stránka bude pomalšia, ale pokiaľ to server zvládne, myslím, že je to v poriadku...
2. Pokúsite sa pripraviť plán na vrátenie štruktúry DB a spustiť ho čo najskôr.
3. Migrujete stránku na výkonnejší server (to však znamená, že klientovi urýchlene zvýšite cenu a potom budete čakať možno 6 hodín na dokončenie migrácie, pretože máte stovky GB databázových tabuliek).
4. Zistíte, ktoré dotazy SQL a ktoré stránky zaberajú najviac času, a jednoducho ich zakážete.
5. Spustíte Cloudflare a necháte ho staticky skontrolovať, čo môže.
6. Niektoré ďalšie kúzla (nemyslím si, že je toho veľa, čo by sa dalo vymyslieť)...
