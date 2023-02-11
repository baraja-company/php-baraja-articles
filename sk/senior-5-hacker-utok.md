Hackerský útok na agentúru
==========================

> id: '7d5edd94-cce4-4a32-9ec5-51260dcd1653'
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
> 	sk: hackersky-utok-na-agenturu
> 
> publicationDate: '2023-02-11 14:20:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: '18511f7b3ff80124720244a93e140932'

Príbeh z roku 2017: pracujete ako vedúci vývojár v agentúre a spravujete približne 300 projektov rôznej veľkosti, ktoré spoločnosť za ten čas vyvinula. Väčšina z nich sú jednoduché aplikácie Nette s maximálne 10 šablónami, niekoľkými formulármi a databázovými tabuľkami. Nič zvláštne. O projektoch toho veľa neviete, pretože každý z nich vyvíjal trochu iný dodávateľ, ľudia sa v spoločnosti striedajú a vy dúfate, že to nejako zvládnete. Keďže spoločnosť robí veľa optimalizácie nákladov, na jednom serveri hostí naraz možno 50 projektov.

Zrazu za vami pribehne projektový manažér s tým, že jedna z dôležitých klientskych stránok prestala fungovať. Namiesto skutočnej stránky sa zobrazí čierna obrazovka a rôzne druhy textu, ktoré hovoria, že stránka bola hacknutá a má prístup k celému serveru.

Opäť platí, že neviete (a ani nemôžete vedieť) toľko o architektúre a obsahu projektov a mnohé projekty bežia len na serveri. Projektor vám vnucuje, že stránka musí fungovať, a pritom neviete, aký je rozsah útoku a kam sa útočník dostal, prípadne či existujú zadné vrátka z minulosti.

Ako sa rozhodnete?

1. Nerobíte nič a najprv sa pokúsite incident analyzovať.
2. Vypnete server. Neviete, aký je rozsah poškodenia, a útočník môže pokračovať v prenikaní do servera a krádeži údajov. Zároveň to znamená, že mnohé stránky sú nedostupné.
3. Vykonáte obnovu konkrétneho webu zo zálohy spred jedného dňa a medzitým riešite, čo sa stalo. Útočník však môže opätovne získať prístup a pokračovať v krádeži údajov.
4. Ďalšie riešenie...
