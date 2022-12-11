Príznaky funkcií / prepínače zapnutia/vypnutia funkcií
======================================================

> id: '4209c32f-655c-46fa-9090-e5ec3883ea61'
> slug:
> 	cs: feature-flagy
> 	sk: priznaky-funkcii-prepinace-zapnutia-vypnutia-funkcii
> 
> publicationDate: '2022-12-11 15:00:00'
> mainCategoryId: '28a2aef7-7490-43a9-add7-80d8a051f8a9'
> sourceContentHash: '1e32c312eab2ca16539cdb7431e2dda9'

Pri vývoji zložitejšej aplikácie oceníte možnosť vyvinúť viac funkcií vopred, distribuovať ich s ďalšou verziou softvéru a funkciu aktivovať neskôr.

Práve na to boli vytvorené príznaky funkcií. V tomto článku sa dozviete, ako ich používať.

Základná implementácia
---------------------

Príznaky funkcií sú v podstate veľmi jednoduchým konceptom volania jednej funkcie/metódy, ktorá rozhodne, či je nová funkcia aktívna.

Napríklad:

```php
echo '<h1>Aplikácie počasia</h1>';
echo 'Dnes je to:' . getWeather();

if (feature('mapa')) {
   echo 'Mapa:' . getMap();
}
```

Na kontrolu dostupnosti konkrétnej novinky sa volá funkcia `feature()`, ktorá na základe názvu volania rozhodne, či môže danú funkciu povoliť alebo ignorovať.

Implementácia logiky rozhodovania
-------------------------------

Logika rozhodovania je často zložitá. Napríklad môžete spustiť určitú funkciu len od určitého dátumu alebo pre používateľov v určitej skupine. Často napríklad takto testujem nasadenie novej funkcie napríklad na 5 % používateľov, aby sa to nedotklo všetkých naraz.

Napríklad pri vývoji firemného sotfvéru takto spúšťame reklamné kampane a zľavy platné od určitého dátumu.

Ak sa nejaká nová funkcia pokazí, je možné ju jednoducho zakázať príznakom funkcie pre používateľov a povoliť ju skupine vývojárov, ktorí ju napríklad otestujú a prinesú opravu.
