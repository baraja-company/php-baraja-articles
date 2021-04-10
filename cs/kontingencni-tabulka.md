Kontingenční tabulka v PHP
==========================

> id: "9bdc1004-8f06-48ec-8f56-8707fad5cef7"
> slug:
> 	cs: kontingencni-tabulka
> 
> publicationDate: "2019-11-13 22:00:05"
> mainCategoryId: fbf79f0a-2287-4ca4-a9f5-97b0a0ec21a1

Kontingenční tabulka obecně slouží k zobrazení vztahu dvou statistických jevů. Při vývoji webové aplikace budeme často potřebovat typicky v administraci vizualizovat vztah určitého jevu v databázi vůči časové posloupnosti.

Například máme tabulku objednávek, ve které jsou vidět jednotlivé produkty a nás zajímá, jak souvisí prodejnost některých velkoobrátkových produktů s časem.

K tomu by se hodila tabulka ve stylu:

| datum   | Jablka | Jahody | Hrušky |
|---------|--------|--------|--------|
| 2019-05 | 10     | 15     | 18     |
| 2019-04 | 12     | 18     | 11     |
| 2019-03 | 13     | 9      | 21     |
| 2019-02 | 6      | 17     | 10     |
| 2019-01 | 7      | 4      | 6      |

V PHP neexistuje žádný jednoduchý způsob, jak si data do takové podoby připravit a získat je přímo do této formy přímo v SQL není také nic elegantního, protože musíme počítat s tím, že existuje dynamický počet sloupců.

Při návrhu výpisu této datové struktury je proto potřeba postupovat chytře.

Serializace dat pomocí klíčů
----------------------------

Při sestavování tabulky často používám získání všech záznamů, které splňují danou podmínku přímo z databáze, například interval data.

Konkrétně:

```sql
SELECT *
FROM `order`
WHERE `inserted_date` <= '2019-05-01'
ORDER BY `inserted_date` DESC
```

Dotaz získá všechny sloupce v tabulce objednávek (`order`), přičemž vyfiltruje všechny záznamy od počátku věků až do `'2019-05-01'`, přičemž se vrátí seřazené od nejnovějšího po nejstarší.

Díky jednoduchému SQL dotazu získáme data prakticky okamžitě. Druhou příjemnou vlastností je fakt, že se při sestavování výsledků mohou efektivně použít databázové indexy. Protože ale máme data v obyčejném poli, musíme je dále ručně serializovat do datové struktury, kterou lze převést na kontigenční tabulku.

Protože kontingenční tabulka popisuje vztah dvou a více faktorů, dává smysl použít vícerozměrný klíč. Protože však některá data nemusí existovat pro všechny kombinace, je lepší klíč serializovat na jeden string a data ukládat jako ploché pole.

Data lze sestavit jedním průchodem cyklu (v proměnné `$selection` je výstup z databáze):

```php
$data = [];

foreach ($selection as $row) {
    $date = date('Y-m', $row->insertedDate); // Datum rok-měsíc
    foreach ($row->items as $product) { // projdeme produkty
        $key = $date . '_' . $product->id;
        if (isset($data[$key])) {
            $data[$key]++; // existuje, přidáme další produkt
        } else {
            $data[$key] = 1; // neexistuje, založíme první produkt
        }
    }
}
```

Pokud bychom zkoumali jednoduší datovou strukturu, nebude vnitřní cykl pro procházení produktů potřeba. V takovém případě by šlo celé sestavení dat vyřešit jedním cyklem.

Díky tomuto přístupu získáme tzv. ploché pole hodnot, které vypadá jako `klíč: hodnota`, přičemž ukládá dvourozměnou informaci.

Výstup je pak například (v `květnu 2019` se produktu s ID `10` prodalo `6` kusů):

```php
$data = [
    '2019-05_10' => 6,
    ...
];
```

Render dat do tabulky - šablony
-------------------------------

Pokud máme data k dispozici ve formě plochého pole, dokážeme celou tabulku velmi jednoduše vykreslit. K tomu stačí jen znát pole všech produktů, které nás zajímají a pole všech datumů, pro které chceme tabulku vypsat.

```php
$products = [ ... ]; // pole produktů: ID => název
$dates = [ ... ]; // podle datumů: datum => label

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
echo '</table>';
```

Všimněte si, že se při procházení dat hledá konkrétní výskyt podle skládání řetězcového klíče. Tento přístup nám umožňuje vykreslenou tabulku libovolně omezit nebo rozšířit v závislosti na tom, jaká procházíme data. Pokud data neexistují, vyhodnotí se ternární operátor `??` a zobrazí nula.

Pole dostupných produktů a datumů můžeme sestavovat v rámci průběhu prvního cyklu, který připravuje data. V tu chvíli budeme mít jistotu, že vykreslujeme vždy jen data, která reálně existují. V takovém případě je velmi důležité, aby byl výstup z SQL databáze seřazen podle data vytvoření, jinak se mohou řádky při finálním renderu tabulky proházet.
