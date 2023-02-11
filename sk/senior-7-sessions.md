Ako sa vysporiadať s náhlymi pádmi skriptov PHP
===============================================

> id: '41edbf1b-3ca5-487f-a54c-fac9926bc3d2'
> slug:
> 	cs: senior-7-sessions
> 	sk: ako-sa-vysporiadat-s-nahlymi-padmi-skriptov-php
> 
> publicationDate: '2023-02-11 14:30:00'
> mainCategoryId: '8b6e2597-bee1-45d5-b0bf-48ccb2d0d94a'
> sourceContentHash: bdd1e207ee9c0ba19c39f0b5c7d83c43

Príbeh z konca roka 2016, keď ma doslova zachránil kolega: v aplikácii PHP sa rozhodnete kontrolovať obrázky prostredníctvom proxy skriptu, ktorý okrem iného dokáže upraviť ich rozmery a ďalšie parametre podľa prichádzajúcej požiadavky. Súčasťou optimalizácie je aj fyzické uloženie vygenerovaných variantov na disk.

V produkčnej prevádzke však zrazu začnete vidieť obrovské zaťaženie a tisíce požiadaviek v rade. Obrázky sa načítavajú postupne jeden po druhom pre každého používateľa. Obnovenie stránky a kliknutia na odkaz nefungujú. Aplikácia sa zdá byť úplne zmrazená. Funguje to len tak, že počkáte, kým sa všetko spracuje.

V čom môže byť problém? V texte som uviedol 3 hlavné stopy, ktoré umožňujú rýchle vyhľadávanie problému. Hotfix má triviálne riešenie.
