Funkcia eval() v jazyku PHP
===========================

> id: '861deebf-ad95-4b12-90df-62664843147e'
> slug:
> 	cs: funkce-eval
> 	sk: funkcia-eval-v-jazyku-php
> 
> publicationDate: '2020-02-16 18:37:20'
> mainCategoryId: '3666a8a6-f2a3-405d-8263-bd53c4301fb3'
> sourceContentHash: '9e8960ba9bb3b92cd606e796660e7e30'

Funkcia `eval` sa používa na vykonanie odovzdaného reťazca ako kódu PHP.

Návrh jazyka PHP a praktické funkcie
---------------------------------------

PHP je interpretovaný jazyk, čo znamená najmä to, že jeho kód vyhodnocuje **interpreter**, špeciálny typ programu, ktorý číta napísaný kód a vyhodnocuje ho priamo z reťazca v reálnom čase. Ostatné jazyky (napríklad C) sa musia pred spustením skompilovať do strojového kódu.

Keďže PHP je interpretované, existuje spôsob, ako zmeniť, čo presne sa bude vyhodnocovať za behu, a dokonca aj dynamicky kompilovať kód, čo je presne to, na čo slúži funkcia `eval()`.

Používajte na vlastné riziko!
---------------------------------

Funkciu `eval` používajte len vtedy, keď presne viete, čo robíte! To znamená najmä to, že ste skontrolovali všetky vstupy používateľov a nemôže dôjsť k narušeniu bezpečnosti. Ak sa totiž používateľovi podarí prepašovať svoj reťazec do funkcie `eval`, bude vyhodnotený ako skutočný kód a môže napríklad vymazať celú stránku, ukradnúť databázu alebo získať kontrolu nad celým serverom.

Príklad z reálneho sveta
--------------

Neexistuje veľa dobrých príkladov, kde by sa dalo použiť `eval`, pretože **prakticky vždy existuje lepší spôsob** riešenia problému.

Možno ho použiť napríklad pri vyhodnocovaní výrazov:

```php
// Dotaz používateľa
$query = '5 + 3 * 2';

// Spracujte výraz ako regulárny kód PHP
eval('$result = @(' . $query . ');');

// Výpis premennej s riešením výrazu
echo $result; // vypíše 11
```

Podrobnosti nájdete v <a href="/pokrocila-kalkulacka">Kalkulátor v PHP: Spracovanie matematického výrazu ako reťazca</a>.

Použitie na vykresľovanie šablón
-----------------------------

Niekedy sa `eval` používa na vyhodnotenie vygenerovaného kódu, typicky skompilovaných šablón.

Ako však bolo spomenuté, každý prípad sa dá riešiť inak a lepšie a v tomto prípade je rozumnejšie uložiť serializovanú šablónu do samostatného súboru PHP a načítať ju pomocou `require` alebo `include`. Okrem toho, že máte plnú kontrolu nad obsahom šablóny, zostane aj fyzicky na disku, čo podporuje vyšší výkon aplikácie vďaka možnosti ukladania do vyrovnávacej pamäte.
