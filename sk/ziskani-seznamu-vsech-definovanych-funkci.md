Získanie zoznamu všetkých definovaných funkcií
==============================================

> id: '99ed7887-33c8-44d7-b0cb-ac37b2336f48'
> slug:
> 	cs: ziskani-seznamu-vsech-definovanych-funkci
> 	sk: ziskanie-zoznamu-vsetkych-definovanych-funkcii
> 
> publicationDate: '2021-04-10 18:59:21'
> mainCategoryId: f611e5d3-ed7b-4fe9-84ca-9271fc2bd2e3
> sourceContentHash: '9dbffadf6cf6473eedadbc0bf7eccbb4'

Niekedy môže byť užitočné získať zoznam všetkých dostupných funkcií v aktuálnom prostredí. To platí najmä vtedy, keď spravujeme cudzí server a potrebujeme sa zorientovať.

Zoznam funkcií možno získať zavolaním funkcie `get_defined_functions()`, ktorá vráti údaje vo forme poľa:

[
   internal => [
      ...,
   ],
   user => [
      ...,
   ]
]

Zoznam funkcií je rozdelený do dvoch veľkých zoznamov.

- Vnútorné funkcie sú funkcie definované samotným PHP a nainštalovanými rozšíreniami.
- Používateľské (`užívateľské`) funkcie sú funkcie definované samotným používateľským kódom. Ide o všetky funkcie, ktoré sme zapísali do zdrojového kódu alebo ktoré sú súčasťou nainštalovaných knižníc.

Tento zoznam sa dá dobre použiť na ladenie aplikácie.
