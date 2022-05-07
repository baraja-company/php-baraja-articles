Kontingenčná tabuľka v PHP
==========================

> id: '9bdc1004-8f06-48ec-8f56-8707fad5cef7'
> slug:
> 	cs: kontingencni-tabulka
> 	sk: kontingencna-tabulka-v-php
> 
> publicationDate: '2019-11-13 22:00:05'
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1
> sourceContentHash: '80fdc1436cd30bc39ffe9c11c3d86c41'

Kontingenčná tabuľka sa zvyčajne používa na zobrazenie vzťahu medzi dvoma štatistickými javmi. Pri vývoji webovej aplikácie budeme často potrebovať vizualizovať vzťah určitého javu v databáze k časovej postupnosti, zvyčajne v administrácii.

Máme napríklad tabuľku objednávok, v ktorej sú uvedené jednotlivé výrobky, a zaujíma nás, ako súvisí predaj určitých veľkoobjemových výrobkov s časom.

Na to by bola užitočná nasledujúca tabuľka:

| Dátum | Jablká | Jahody | Hrušky |
|---------|--------|--------|--------|
| 2019-05 | 10 | 15 | 18 |
| 2019-04 | 12 | 18 | 11 |
| 2019-03 | 13 | 9 | 21 |
| 2019-02 | 6 | 17 | 10 |
| 2019-01 | 7 | 4 | 6 |

Neexistuje jednoduchý spôsob, ako pripraviť údaje do tohto formulára v PHP, a dostať ich do tohto formulára priamo v SQL tiež nie je elegantné, pretože musíme zohľadniť, že existuje dynamický počet stĺpcov.

Preto musíme byť pri navrhovaní výstupu tejto dátovej štruktúry inteligentní.

Serializácia údajov pomocou kľúčov
----------------------------

Pri zostavovaní tabuľky často používam načítanie všetkých záznamov, ktoré spĺňajú danú podmienku, priamo z databázy, napríklad intervalové údaje.

Konkrétne:

SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC

Dotaz načíta všetky stĺpce v tabuľke order (`order`), filtruje všetky záznamy od začiatku veku až po ``2019-05-01`` a vráti ich zoradené od najnovších po najstaršie.

Pomocou jednoduchého dotazu SQL získame údaje takmer okamžite. Druhou príjemnou vlastnosťou je, že pri zostavovaní výsledkov možno efektívne využívať databázové indexy. Keďže však máme údaje v obyčajnom poli, musíme ich ďalej ručne serializovať do dátovej štruktúry, ktorú možno previesť na kontigenčnú tabuľku.

Keďže kontingenčná tabuľka opisuje vzťah dvoch alebo viacerých faktorov, má zmysel použiť viacrozmerný kľúč. Keďže však niektoré údaje nemusia existovať pre všetky kombinácie, je lepšie serializovať kľúč do jedného reťazca a uložiť údaje ako ploché pole.

Údaje možno zostaviť v jednom cykle (premenná `$selection` obsahuje výstup z databázy):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Dátum rok-mesiac
    foreach ($row->items as $product) { // prechádzame cez produkty
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // existuje, pridáme ďalší produkt
        } else {
            $data[$key] = 1; // neexistuje, spustíme prvý produkt
        }
    }
}
```

Ak by sme skúmali jednoduchšiu dátovú štruktúru, vnútorný cyklus by nebol potrebný na prehľadávanie produktov. V tomto prípade by sa celé zostavenie údajov mohlo vyriešiť jedným cyklom.

Týmto prístupom získame takzvané ploché pole hodnôt, ktoré vyzerá ako `key: value' a uchováva dvojrozmerné informácie.

Výstupom je potom napríklad (v `máji 2019` sa predalo `6` kusov produktu s ID `10`):

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Renderovanie údajov do tabuľky - šablóny
-------------------------------

Ak máme údaje vo forme plochého poľa, môžeme veľmi jednoducho vykresliť celú tabuľku. Na to stačí poznať polia všetkých produktov, ktoré nás zaujímajú, a polia všetkých dátumov, pre ktoré chceme vykresliť tabuľku.

```php
$products = [ ... ]; // pole produktu: id => názov
$dates = [ ... ]; // podľa dátumu: date => label

echo '<table>';
foreach ($products as $productId => $productName) {
    echo '<tr>';
    foreach ($dates as $date => $dateLabel) {
        echo '<td>';
        echo htmlspecialchars(
            (string) ($data[$date . '_' . $productId] ?? '0')
        );
        echo '<td>';
    }
    echo '</tr>';
}
echo '</tabuľka>';
```

Všimnite si, že pri prehľadávaní údajov sa hľadá konkrétny výskyt podľa reťazcového kľúča skladania. Tento prístup nám umožňuje ľubovoľne obmedziť alebo rozšíriť vykresľovanú tabuľku v závislosti od toho, aké údaje prezeráme. Ak údaje neexistujú, vyhodnotí sa ternárny operátor `??` a zobrazí sa nula.

Pole dostupných produktov a dátumov môžeme vytvoriť v rámci prvého cyklu, ktorý pripravuje údaje. Vtedy budeme mať istotu, že vždy vykresľujeme len údaje, ktoré skutočne existujú. V tomto prípade je veľmi dôležité, aby bol výstup z databázy SQL zoradený podľa dátumu vytvorenia, inak môže dôjsť k premiešaniu riadkov počas konečného vykresľovania tabuľky.
