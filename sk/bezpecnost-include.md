Zahrnutie zabezpečenia do PHP a prílohy súboru
==============================================

> id: '820f8de6-ff1e-406c-8fe5-95080642656f'
> slug:
> 	cs: bezpecnost-include
> 	sk: zahrnutie-zabezpecenia-do-php-a-prilohy-suboru
> 
> publicationDate: '2019-08-22 20:48:46'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '1207d637c20fcb5e8f609be2eb449135'

Často sa môže stať, že chceme k stránke pripojiť súbor, ktorý máme uložený na disku niekde inde. Ak zadáme jeho presný názov priamo do funkcie attach, nemusíme sa ničoho obávať.

Bezpečné pripojenie súboru
--------------------------

```php
include 'menu.html';
```

Predchádzajúci zápis je úplne bezpečný, pretože vždy pripájame ten istý súbor. V tomto prípade nemôže dôjsť k bezpečnostnej chybe. Jediným problémom, ktorý môže nastať, je absencia súboru **menu.html**, čo vyvolá varovnú správu (ktorá sa pravdepodobne aj tak nezobrazí), ale táto situácia je zriedkavá, pretože zvyčajne pripájame súbor, ktorého existencia je prakticky istá.

Pripojenie súboru podľa vzoru
--------------------------

Ale čo ak chceme napríklad pripojiť články k jednoduchému webu s obsahom, ako je tento? Na tejto stránke mám fyzický priečinok, kde sú články uložené vo formáte HTML a pripájam ich priamo k zdrojovému kódu.

Samotné pripojenie však nestačí! Začiatočník môže takto nazývať jednotlivé články:

```php
include 'articles/' . $_GET["clanek] . '.html';
```

> Je to však veľmi nebezpečné. Útočník môže odovzdať odkaz na iný adresár pomocou `../` alebo niečoho podobného ako názov článku a niekedy je možné zbaviť sa koncovky odovzdaním nulového bajtu na konci. Mali by ste použiť aspoň funkciu `basename()`, ale lepšie je povoliť len hodnoty z bieleho zoznamu.

Prečo nenačítať nerelevantné súbory?
--------------------------

Často nám nevadí načítanie nesprávneho (neočakávaného) súboru - je to chyba používateľa, ktorý si vyžiadal stránku, ktorú v skutočnosti nechcel, ale môžu nastať situácie, keď to má význam. Najmä:

- Používateľ načíta súbor, ku ktorému verejnosť nemá prístup a môže sa k nemu dostať len server.
- Načítanie iného skriptu PHP môže vyvolať neočakávanú akciu alebo chybové hlásenie, ktoré môže napovedať, ako stránka funguje, a pomôcť k ďalším útokom.
- Načítanie iného súboru môže spôsobiť nielen jeho pridanie do dokumentu, ale aj jeho spustenie.

Biela listina a overovanie vstupov
--------------------------

Ak nemáme možnosť overiť vstup nejakým bezpečným spôsobom (napr. z whitelistu), nemali by sme sa spoliehať na poctivosť používateľa a brániť skripty aspoň na úrovni PHP.

Prvou dôležitou vecou je umiestniť všetky načítané súbory do rovnakého priečinka (adresára) a zakázať niektoré nebezpečné znaky, najmä lomítko a bodku. Tým sa znemožní prístup k iným priečinkom, ktoré obsahujú potenciálne zraniteľné súbory. Nebezpečné znaky možno zakázať aj jednoduchým odstránením zo vstupného reťazca.

```php
$load = '../index'; // tento vstup môže byť potenciálne nebezpečný
$load = strtr($load, './', ''); // odstráni všetky bodky a lomítka z reťazca
include $load .'.html';
```

Spúšťanie súborov?
--------------------------

Je dôležité poznamenať, že konštrukcia **include** vykonáva súbory po pripojení rovnakým spôsobom, ako keby išlo o kód PHP, preto je dobré túto možnosť umožniť.

Často však pripájame súbory, ktoré sa nemusia následne vykonať, a zaujíma nás len uložený text (obsah) vo forme reťazca. Preto môžeme súbor načítať do premennej a pracovať s ním ako s reťazcom, čo je celkom bezpečné.

```php
$load = '../index'; // tento vstup môže byť potenciálne nebezpečný
$load = strtr($load, './', ''); // odstráni všetky bodky a lomítka z reťazca
$file = file_get_contents($load . '.html'); // načítanie obsahu do premennej
echo $file; // výpis obsahu súboru
```

Toto riešenie vyzerá na prvý pohľad zaujímavo a bezpečne - a bezpečné aj je. Aj keď sa používateľovi podarí zavolať súbor PHP, nikdy sa nespustí. Môže ho však zobraziť (teda jeho zdrojový kód) a na to si musíme dať pozor.

Rozpoznanie požadovaného súboru zo skriptu
--------------------------

Neexistuje na to žiadna definitívna príručka, každý si to musí urobiť sám podľa potrieb scenára. Článok od iných súborov rozpoznám napríklad podľa toho, že obsahujú nadpis veľkosti H1. Ak teda niekto načíta súbor, v ktorom nie je žiadny nadpis, nezobrazím nič a stránka sa skončí chybovou správou. Vždy je dôležité nájsť nejakú jedinečnú vlastnosť, ktorú majú len požadované súbory a ktorú ostatné súbory nemajú, a potom môžete pokračovať.

Záver
--------------------------

Hoci je validácia a načítanie súborov pomerne jednoduché, veľké množstvo začiatočníkov stále robí chyby - a bude ich robiť aj naďalej. Najdôležitejšie je správne pochopiť význam toho, čo načítavame, a odlíšiť požadovaný obsah od ostatného. A čo je najdôležitejšie, pracujte s obsahom ako s reťazcom a nikdy ho nenahrávajte priamo do stránky.
