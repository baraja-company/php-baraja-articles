Útok hackerů na agenturu
========================

> id: "7d5edd94-cce4-4a32-9ec5-51260dcd1653"
> slug:
> 	cs: senior-5-utok-hackeru-na-agenturu
>
> publicationDate: "2023-02-11 14:20:00"
> mainCategoryId: 8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a

Příběh z roku 2017: Pracujete jako hlavní vývojář v agentuře, a ve správě máte cca 300 různě velkých projektů, které firma za tu dobu vyvinula. Většina z nich je jednoduchá Nette aplikace, kde je do 10 šablon, pár formulářů a databázové tabulky. Nic extra. O projektech toho zas tolik nevíte, protože každý vyvinul trochu jiný dodavatel, lidé se ve firmě střídají, a vy doufáte, že to nějak dáte. Protože firma hodně optimalizuje náklady, tak v té době hostuje třeba 50 projektů na jednom serveru.

Najednou za vámi přiběhne projekťák, že jeden z důležitých klientských webů přestal fungovat. Na místo reálného webu se zobrazuje černá obrazovka, a tam různé texty o tom, že byl web napaden hackerem, a má přístup k celému serveru.

Ještě jednou připomínám, že o architektuře a obsahu projektů toho zas tolik nevíte (a ani validně vědět nemůžete), a mnoho projektů běží na jenom serveru. Projekťák na vás tlačí, že web musí fungovat, a vy přitom nevíte, jaký je rozsah útoku a kam všude se útočník dostal, a jestli existují zadní vrátka z minulosti.

Jak se rozhodnete?

1. Neuděláte nic a pokusíte se incident nejprve analyzovat.
2. Server odstavíte od internetu. Nevíte rozsah škod a útočník může dál pronikat do serveru a krást data. Zároveň to ale znamená nedostupnost mnoha webů.
3. Provedete obnovu konkrétního webu ze zálohy o den zpět, a v mezičase budete řešit, co se stalo. Útočník ale může získat přístup znovu a pokračovat ve vykrádání dat.
4. Jiné řešení...
