Konfigurácia pripojenia k doktríne Baraja
=========================================

> id: fbd0961a-53fe-4713-8526-82e36bd1fb9b
> slug:
> 	cs: konfigurace-spojeni-s-baraja-doctrine
> 	sk: konfiguracia-pripojenia-k-doktrine-baraja
> 
> publicationDate: '2020-09-10 11:38:44'
> mainCategoryId: '818d311a-0f58-4df7-a9a4-da7d21489dd6'
> sourceContentHash: cee6f56731ae88d1de9aac0da49e0874

Na vytvorenie spojenia s databázou v rámci [Baraja Doctrine](https://github.com/baraja-core/doctrine) musíte použiť konfiguračný súbor Neon, ktorý je bežnou súčasťou rámca Nette.

Konfigurácia môže vyzerať takto:

```neon
baraja.database:
    pripojenie:
        hostiteľ: localhost
        dbname: my-database
        používateľ: root
        heslo: ******
```

Pri kompilácii kontajnera DI sa skontroluje správnosť konfigurácie a vyhodí sa chybová správa s popisom konkrétnej chyby.

Prihlasovacie údaje sa pri zostavovaní kontajnera bezpečne overia a potom sa fyzicky uložia do kontajnera. Prístup k prihlasovacím údajom má potom len služba, ktorá poskytuje pripojenie k databáze, a nemôže ich jednoducho získať externá služba alebo nečestný návštevník z panela Tracy.

Spätná kompatibilita
----------

V minulosti sa používali definície pomocou parametrov, napríklad:

``neon
parametre:
    databázy:
        primárne:
            hostiteľ: localhost
            ...
```

Toto nastavenie je však označené ako **zrušené**, aby sa zvýšila bezpečnosť aplikácie. Pri použití parametrov by totiž mohla akákoľvek služba (alebo dokonca časť aplikácie) požadovať prihlasovacie údaje alebo by ich mohol prezradiť aktívny panel Tracy na stránke.
